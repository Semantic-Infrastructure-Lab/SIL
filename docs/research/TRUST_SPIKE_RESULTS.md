---
title: "Trust Spikes: SCITT Keystone + Falsification Test Results"
type: analysis
status: complete
author: "Scott Senkeresty"
created: 2026-06-22
session: topaz-ray-0622
beth_topics:
  - SIL
  - genesisgraph
  - trust
  - scitt
  - opa
  - in-toto
  - falsification
related_projects: [genesisgraph, SIL]
---

# Trust Spikes: Results

Two cheap go/no-go tests from the [consume-vs-build determination](TRUST_CONSUME_VS_BUILD_DETERMINATION.md).
Both ran 2026-06-22 in session topaz-ray-0622.

## Spike 1: SCITT Keystone — GG document as COSE_Sign1 ✅ PASS (corrected)

**File:** `/home/scottsen/src/projects/genesisgraph/spike_scitt_wrap.py`

**Question:** Does "GG document = valid SCITT Signed Statement payload" actually hold?

> **Honesty correction (same session):** the first version of this spike was **wrong** —
> it put `iss`/`sub` in the *payload* as a CWT and stuffed a *summary* of the GG doc as a
> custom claim. That proved "GG metadata → a CBOR map," not the load-bearing claim.
> I verified the real structure against the SCITT architecture I-D via WebFetch:
> - Signed Statement = `#6.18(COSE_Sign1)`
> - CWT claims (`iss`=1, `sub`=2) MUST go in the **protected header** under **label 15** (CWT_Claims, RFC 9597), not the payload
> - The **payload IS the Statement content itself** (the GG document), MAY be detached
> - Protected header also carries `alg` (1), `content-type` (3); `kid` (4) required when no x5t/x5chain
>
> The spike was rewritten to be faithful and re-run. This is the result that counts.

**Method:** Load `level-a-full-disclosure.gg.yaml`, use the **raw document bytes as the
COSE_Sign1 payload**, put `attestation.signer → iss` and entity SHA-256 → `sub` in the
**protected header CWT_Claims (label 15)**, set `content-type`, hand-roll `COSE_Sign1`
(tag 18) with `cbor2` + `cryptography` Ed25519. Verify round-trip + SCITT conformance
(CWT_Claims present with iss+sub) + two tamper checks (wrong key; flipped payload byte).

**Result:**
```
Signed Statement : 5442 bytes
Payload (GG doc) : 5215 bytes      ← the actual document, not a summary
✓  Signed Statement verified — signature valid
✓  SCITT conformance: CWT_Claims present with iss + sub
   Decoded iss   : did:svc:retrieval-prod
   content-type  : application/vnd.genesisgraph+yaml
   Payload re-parses as GG doc, profile=gg-ai-basic-v1
✓  Tamper check 1 passed — wrong key rejected
✓  Tamper check 2 passed — modified payload rejected
```

**Conclusion:** The mapping is clean and direct, and now actually demonstrates what it
claims: the **GG document bytes are the SCITT payload**, with iss/sub in the protected
header per spec. Only dependency: `cbor2` (small; C accelerator with a pure-Python fallback).

**Honest caveat:** this is a hand-rolled COSE_Sign1, not a conformance-tested library.
It is *structurally* faithful but has not been validated against official SCITT/COSE test
vectors. Production should use `pycose`/`python-cwt` and run the test vectors.

**What this unlocks:**
- Replace the ephemeral key with a real `did:key` or Sigstore keyless identity
- Submit `cose_bytes` to any SCITT Transparency Service → get a Receipt for free
- GG drops its half-built transparency log ambitions; rides IETF infrastructure instead

---

## Spike 2: Falsification Test — MCP Tool Trust End-to-End ✅ PASS (NARROWLY)

**Files:** `/home/scottsen/src/projects/genesisgraph/spike_trust_reasoning/`

**Question:** Is the inspectable evidence subgraph + purpose-query meaningfully better
than OPA pass/fail for a real user?

**Method:** 
- Real attestations from `level-a-full-disclosure.gg.yaml` (4 GG operations)
- Synthetic in-toto/SLSA statement (normalized to GG attestation shape)
- Synthetic C2PA statement (for cross-domain scenario)
- OPA/Rego policy: `policy.rego` — evaluates signer trust, age, capability, purpose
- Evidence subgraph: OPA returns structured `evidence` rule alongside `allow` bool
- Four scenarios: ALLOW, expired DENY, untrusted-signer DENY, cross-domain ALLOW

**Verdict by scenario type:**

| Scenario | OPA boolean | Evidence subgraph adds |
|---|---|---|
| ALLOW (happy path) | Sufficient for automation | Audit trail + expiry monitoring (e.g., "signer X renews in 14 days") |
| DENY: expired | `DENY\n• 234.9 days old (max 365)` | Which specific operation's attestation expired; other operations still valid |
| DENY: untrusted | `DENY\n• untrusted signer: did:unknown:shadyvendor` | Empty trust chain + `purpose_match: missing` clarifies scope of failure |
| Cross-domain (GG + in-toto + C2PA) | `ALLOW` (flat list) | Three-source chain visible: GG process (234d) + Sigstore build (30d) + C2PA media (7d) normalized together |

**Where the subgraph wins:**
1. **DENY debugging** — shows exactly which attestation broke and the others that still pass. Not just "failed", but "op_retrieval_001's attestation is stale; op_inference_001 is fine."
2. **Cross-domain normalization** — when attestations come from GG, in-toto, and C2PA simultaneously, the subgraph is the only place to see all three normalized. OPA evaluates them but returns a flat result.
3. **Expiry monitoring** — `age_check` per signer enables "which attestations need renewal in the next 30 days?" without re-running policy.

**Where it doesn't beat OPA:**
- Single-source ALLOW: `ALLOW` is sufficient; the subgraph is noise.
- Automation gates: a CI/CD pipeline only needs the boolean.

**Honest narrow conclusion:** The BUILD thesis survives, but only barely, and only for
multi-source cases. The beachhead (multi-source MCP tool attestations) is real. Do not
build for single-source use cases — OPA alone covers those.

### ⚠️ The finding I initially buried — and it narrows the thesis again

**The "evidence subgraph" in this spike is produced *by OPA itself*** — the `evidence`
object is a plain Rego rule in `policy.rego`. I did not build a reasoning engine to
generate it; OPA generated it. This is the *fourth* time the white space narrowed under
scrutiny (after: SCITT owns the registry; OPA/Sigstore own policy eval; scoring products
exist). It matters because it reframes what "BUILD" even means:

- **What is NOT differentiated:** the evaluation *and the evidence structure itself*.
  OPA/Rego does both. There is no "trust-reasoning engine" to build as a distinct artifact —
  Rego is the reasoning engine.
- **What IS left, honestly:** three thin layers around OPA —
  1. **Normalization** — GG / in-toto / C2PA / SBOM → one common attestation shape.
     This is the real work and the only genuinely cross-domain piece. (In the spike,
     `normalize_intoto()` is ~12 lines — promising, but the breadth across formats is the effort.)
  2. **A curated library of purpose-scoped Rego policies** (e.g. "trust an MCP tool for
     code-execution"). This is *content*, not novel technology.
  3. **A renderer** — evidence object → inspectable subgraph via Reveal. Novel but thin.

- **So the honest product is not "a trust-reasoning engine."** It is a
  **normalization layer + Rego policy library + Reveal renderer, sitting on OPA.**
  That is smaller, more clearly scoped, and more achievable than the determination doc's
  framing implied — but it is also *less* defensible as technology. Its moat is breadth of
  normalization + the SIL/Reveal inspection brand, not any reasoning cleverness.

**Does the thesis still survive? Yes — but rename it.** The spike confirms the
determination doc's own warning ("narrow and brand-dependent") rather than discovering new
white space. Proceed only with eyes open: the value is *cross-domain normalization* and
*inspectable rendering*, both of which are integration/UX work, not research. If neither
of those visibly beats "OPA + jq" for a real multi-source user, the thesis is dead.

---

## What the spikes changed / clarified

1. **`mock:` bypass was broader than documented — ✅ FIXED this session.** The old
   `validator.py` bypassed on `sig_data.startswith(('mock:', 'sig'))`, so every example doc
   signature (`ed25519:sig1_aabbccdd`) silently bypassed real verification, not just
   `mock:`-prefixed ones — a naming accident, not intent. **Fix applied:** the validator now
   bypasses only on an explicit `mock:` prefix (`validator.py`, `_verify_signature`); the
   accidental `sig` match is gone. Example files were updated to `ed25519:mock:sig…` so they
   remain valid in test runs. Full GG suite: 361 passed, 0 regressions.

2. **in-toto normalization is trivial.** The `subject`+`predicate` shape is a structural
   twin of a GG attestation. An ingest adapter is ~50 lines: map `subject[0].digest.sha256 → entity_hash`,
   `builder.id → signer`, `predicateType → profile`. This single move makes GG a consumer of
   the entire Sigstore/SLSA/GitHub attestation ecosystem.

3. **The SCITT spike needed only `cbor2`.** No exotic pairing libraries, no WASM,
   no external service for the round-trip. The real SCITT integration adds a Transparency
   Service submission step (one HTTP call) but the structural mapping is done.

---

## Revised sequence (post-spikes)

Both tests passed → proceed to build phase.

> **`infinite-void-0622` update: all four steps shipped.**

1. **GG honesty cleanup** — ✅ **Done** (`infinite-void-0622`):
   - ✅ Removed the accidental `sig`-prefix bypass; validator bypasses only on
     explicit `mock:` prefix; example signatures renamed to `ed25519:mock:sig…`.
   - ✅ Downgraded ZK/BBS+/SD-JWT claims in 7 docs to "demonstration-grade."
     `zkp-templates.md` is now the canonical crypto-status table.

2. **SCITT-payload reframe** — ✅ **Shipped** (`genesisgraph/scitt.py`, `infinite-void-0622`):
   - `gg_to_cose_sign1(doc_bytes, private_key, kid)` → `bytes`
   - `verify_cose_sign1(cose_bytes, public_key)` → `dict`
   - CLI: `gg sign --scitt level-a.gg.yaml > statement.cose`
   - 20 tests, 100% coverage. `cbor2` added to core deps.

3. **in-toto ingest adapter** — ✅ **Shipped** (`genesisgraph/interop.py`, `infinite-void-0622`):
   - `normalize_intoto(stmt)` maps SLSA Provenance v0.2, in-toto Link v0.2, GitHub Actions
     attestations to the GG attestation shape. 17 tests.
   - Breadth remaining: C2PA, CycloneDX SBOM.

4. **Trust inspection layer MVP** — ✅ **Shipped** (`genesisgraph/trust/`, `infinite-void-0622`):
   - `trust.evaluate(doc, purpose)` — OPA/Rego subprocess; datetime-aware GG extractor;
     `trusted_signers` defaults to all signers in doc (safe first-run UX).
   - `trust.render(evidence, purpose, tool_id)` — **kill criterion: PASS.** Tested
     multi-source GG + SLSA against raw `opa eval | jq`: rendered output readable in 10s,
     the JSON blob requires real parsing. Product exists.
   - `policy.rego` bundled as package data. CLI: `gg trust <file> --purpose <p>` (exit 0/1).
   - 28 tests.
   - **Remaining moat work:** C2PA + SBOM normalization adapters in `interop.py`.
   - **Renderer → Reveal integration:** terminal text currently; next step.
