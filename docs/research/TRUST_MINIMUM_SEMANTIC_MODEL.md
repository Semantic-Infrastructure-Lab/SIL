---
title: "The Minimum Semantic Representation of Trust (MVP-of-Trust)"
type: research
status: idea-capture
author: "Scott Senkeresty"
created: 2026-06-22
session: mint-crystal-0622
beth_topics:
  - SIL
  - genesisgraph
  - trust
  - provenance
  - verifiable-credentials
  - selective-disclosure
  - semantic-model
related_projects: [genesisgraph, reveal, SIL]
---

# The Minimum Semantic Representation of Trust (MVP-of-Trust)

> Companion to [`SOFTWARE_TRUST_AI_ERA.md`](SOFTWARE_TRUST_AI_ERA.md) (the landscape) and [`TRUST_TOOLING_PLACEMENT.md`](TRUST_TOOLING_PLACEMENT.md) (where the tooling lives). This doc is the **model**: what is the smallest semantic structure that can represent a trust judgment, and how does it relate to what GenesisGraph already has?

**The question trust actually answers:** *"Should I rely on X, for purpose P, right now?"*

Every word matters. Drop **X** and there's nothing to evaluate. Drop **purpose P** and "trust" is meaningless (you trust your dentist to fix teeth, not to fly a plane). Drop **right now** and you ignore revocation and decay. Today's systems mostly answer a degenerate version — "is X signed by a known publisher?" — which is purpose-free, time-weak, and binary. That gap is the opportunity.

---

## The seven irreducible parts of a trust judgment

A single trust judgment decomposes into seven elements. The ChatGPT thread that seeded this proposed `Entity · Claim · Evidence · Provenance · Relationship · Context · Time`; refined:

| # | Element | What it is | Without it… |
|---|---------|------------|-------------|
| 1 | **Subject** | The thing being trusted — artifact (hash), agent (DID), person, tool, dataset, model output | nothing to evaluate |
| 2 | **Purpose / Context** | The use the trust is *for* ("run on prod", "delegate payments", "cite as fact") | "trust" is undefined |
| 3 | **Claim** | A typed proposition that, if true, supports trusting for that purpose ("no critical CVEs", "authored by Scott", "stayed within safety params", "human-reviewed") | trust is vibes |
| 4 | **Evidence** | What makes the claim *checkable* — a signature, attestation doc, scan report, transparency-log entry, or observed history — plus a **verification method** and a **strength** | claim is hearsay |
| 5 | **Issuer / Attestor** | Who asserts the claim. Recursively a Subject themselves → trust chains | no accountability |
| 6 | **Validity** | Issued-at, expiry, freshness, revocation status. Trust decays. | stale/revoked trust looks live |
| 7 | **Relationship** | Edges among issuers/subjects that let trust *propagate* — `vouches-for`, `delegated-by`, `mentored-by` | no web-of-trust, no multi-hop |

**The atom** — a *Trust Assertion*:

```
TrustAssertion = ⟨ issuer, subject, claim, evidence, context, validity ⟩
```

**The edges** — *Relationships* connect assertions/entities (issuer A `vouches-for` issuer B; agent X `delegated-by` user Y).

**The judgment** — a *query*, not a stored value:

```
trust(subject, purpose, anchors) →
    walk assertions + provenance + relationships reachable from `anchors`,
    filter to those relevant to `purpose`,
    verify evidence, apply validity/revocation,
    return an EVIDENCE SUBGRAPH (not a number)
```

The output is the **inspectable evidence subgraph**, optionally summarized. This is the SIL move: trust as *"inspect me,"* not *"trust me"* — and it's the thing none of the existing registries/signers give you (see landscape doc).

---

## MVP: what you cannot remove

For a first buildable version, strip to the load-bearing minimum:

```yaml
trust_assertion:
  subject:   { id: "sha256:abc…", type: artifact }     # or did:agent:…, did:person:…
  context:   "install-on-developer-machine"            # purpose tag — REQUIRED, even if coarse
  claim:     { type: no-critical-cves, value: true }   # typed
  evidence:  { kind: scan-report, ref: "rekor://…", verify: "trivy>=0.50", strength: medium }
  issuer:    { id: "did:web:scan.example.com" }        # must be resolvable (later: itself a subject)
  validity:  { issued_at: "2026-06-22T…", expires: "2026-07-22T…", revocation: "https://…/status" }
```

Two MVP constraints that keep the door open for the real value:
1. **`issuer` must be referenceable as a `subject`** — so assertions can chain into a web of trust later (element 7) without a schema break.
2. **`context` is mandatory** — even a coarse tag. This is the single field that separates trust *reasoning* from credential *checking*, and it's the one every existing system omits.

**Relationship (element 7) is the v2** that turns credential-checking into multi-hop trust reasoning. The MVP doesn't *implement* propagation, but its shape must not preclude it.

---

## How this maps onto GenesisGraph (the honest version)

GenesisGraph is **strong on provenance, weak on standalone trust** — and that's fine, because the trust layer is exactly what's missing (GG's own vision calls trust "the missing layer"). Verified against the repo (2026-06-22):

| MVP element | GG today | Reuse / gap |
|-------------|----------|-------------|
| **Subject** | `Entity` (id+hash) and `Tool/Agent` (DID) | ✅ reuse directly |
| **Claim** | `attestation.claims.results{ temperature: {lte:0.3, satisfied:true} }` | ✅ GG's claim shape is *exactly* claim+evidence — reuse |
| **Evidence** | signature, transparency anchor, predicate commitment | ⚠️ partial — many "proofs" are commit-and-reveal mocks (no ZK soundness); a `mock:` prefix bypasses signature verify in the prod path. Treat GG evidence as *binding/auditable*, not *independently verifiable while sealed* |
| **Issuer** | `attestation.signer` (a DID) | ⚠️ only `did:key/web/ion/ethr` resolve; the `did:person/org/machine/agent` in every example **do not resolve** |
| **Validity** | `timestamp`; revocation is **roadmap v0.5 (Mar 2026), unbuilt** | ❌ gap — MVP must add expiry + revocation |
| **Context/Purpose** | `metadata.compliance_framework` tag (unenforced) | ❌ **not first-class** — the key thing to add |
| **Relationship** | delegation designed for **v0.6 (May 2026), unbuilt**; no `vouches-for`/reputation edge exists (the "trust_assertion edge" from the ChatGPT thread is **confabulated — it is not in the repo**) | ❌ **the genuine white space** |

**Crucial structural fact:** GG binds attestations to **Operations** (provenance edges — "how it was made"). The MVP-of-trust needs a **standalone, subject-anchored, purpose-scoped assertion** ("X is trustworthy-for-P"), liftable out of the operation graph and composable into relationships. GG is the *substrate the assertions point into*, not the assertion layer itself.

---

## Theory & prior art (this isn't unfounded — but it's early)

The "purpose/context is the missing dimension" claim has academic backing, which is reassuring *and* a signal the space is real but under-built:

- **"A Logic of Sattestation"** (arXiv [2405.01809](https://arxiv.org/pdf/2405.01809)) — a formal logic for reasoning about trust where **contextual trust is grounded in the attesting principal**: a claim is only as trustworthy as the principal *contextually trusted to make that claim*. This is element #5 (issuer-as-subject) + #7 (relationship) formalized. Worth a close read before fixing the model.
- **Trust frameworks contextualize identical signatures** — the VC community's own observation: a degree credential audited under a national framework and one from an institution that merely registered a key have *identical cryptographic signatures but different assurance*. This is precisely why a signature is not a trust judgment, and why **context** (#2) and **issuer assurance** (#5) must be first-class. ([Dock VC guide](https://www.dock.io/post/verifiable-credentials), [The Trust Graph — "The Proof Gap"](https://thetrustgraph.substack.com/p/the-proof-gap))
- **W3C Verifiable Credentials Data Model v2.1** (FPWD 2026) — the credential *envelope* to align with; our trust atom is a *reasoning layer over* VCs/attestations, not a replacement for them.

Implication: the MVP atom should be **expressible as / derivable from** a W3C VC and a SCITT signed statement, so the reasoning layer consumes the standards rather than forking them.

## What this implies for building

- The trust layer **consumes** GG provenance docs *and* external attestations (in-toto, Sigstore/Rekor, W3C VC, SBOM/AIBOM) and **normalizes** them into trust atoms (see [`TRUST_INTEROP_MATRIX.md`](../../../genesisgraph/docs/strategic/TRUST_INTEROP_MATRIX.md)). It does not invent a competing signing/registry format.
- The first deliverable is the **trust query** over normalized atoms, returning an evidence subgraph — naturally renderable via progressive disclosure (Reveal's brand) and naturally selective-disclosure-aware (GG's brand).
- Keep it **small and independently auditable** — a trust verifier you can't audit is a contradiction. This is why it wants to be its own tool, re-exposed through Reveal rather than buried in it ([placement doc](TRUST_TOOLING_PLACEMENT.md)).

## Open questions

- Is `context/purpose` a free tag, a controlled vocabulary, or a capability expression (reusing GG's `capabilities`/`realized_capability`)?
- How is **evidence strength** typed so a query can threshold on it ("require ≥ medium for prod")?
- Does relationship propagation need decay/attenuation (PGP-web-of-trust lessons) to avoid runaway transitive trust?
- Where does **observed behavior / reputation** enter? GG explicitly has none; the MVP atom can express a single behavioral claim, but accumulation is a separate concern (and a separate trust risk).
