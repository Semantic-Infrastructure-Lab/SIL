---
title: "Trust Work: Consume-vs-Build Determination"
type: analysis
status: decision
author: "Scott Senkeresty"
created: 2026-06-22
session: mint-crystal-0622
session_provenance: "Validate-before-build study: close reads of IETF SCITT architecture I-D, 'A Logic of Sattestation', and EU DPP technical requirements"
beth_topics:
  - SIL
  - genesisgraph
  - reveal
  - trust
  - scitt
  - digital-product-passport
  - provenance
  - strategy
related_projects: [genesisgraph, reveal, SIL]
---

# Trust Work: Consume-vs-Build Determination

> **⚠️ Superseded in part — read with [`TRUST_SPIKE_RESULTS.md`](TRUST_SPIKE_RESULTS.md) (session `topaz-ray-0622`).** Both go/no-go spikes this doc recommends have now run. The SCITT keystone **passed** (GG document is a valid SCITT Signed Statement payload). The falsification spike **passed but sharpened the verdict**: the "trust-reasoning engine" framing below is an overstatement — OPA/Rego itself produces the evidence subgraph, so the real product is a thinner **normalization + Rego-policy-library + Reveal-renderer** layer on OPA, not a distinct engine. This doc's own caveats (§"Honest caveats") already anticipated this; the spike confirmed it. Everywhere below that says "trust-reasoning engine," read "trust *inspection* layer on OPA."

> The diligence gate before any trust-tool code. Built from close reads of three primary sources: the **IETF SCITT architecture I-D**, **"A Logic of Sattestation"** (arXiv 2405.01809), and the **EU DPP** technical/compliance requirements. Capstone of: [`SOFTWARE_TRUST_AI_ERA.md`](SOFTWARE_TRUST_AI_ERA.md) · [`TRUST_MINIMUM_SEMANTIC_MODEL.md`](TRUST_MINIMUM_SEMANTIC_MODEL.md) · [`TRUST_TOOLING_PLACEMENT.md`](TRUST_TOOLING_PLACEMENT.md) · [`../../../genesisgraph/docs/strategic/TRUST_INTEROP_MATRIX.md`](../../../genesisgraph/docs/strategic/TRUST_INTEROP_MATRIX.md). · **Results:** [`TRUST_SPIKE_RESULTS.md`](TRUST_SPIKE_RESULTS.md).

## Verdict (one paragraph)

**Stop building provenance plumbing; build the *inspection* layer on top of it.** The close reads collapse the strategy to one realization: **SCITT is content-agnostic and explicitly punts the "should I trust this?" decision to the Relying Party.** That means (a) GenesisGraph should *become a SCITT statement payload* rather than re-host its own half-built transparency log, and (b) the defensible product is a **Relying-Party trust-reasoning engine** that consumes SCITT Transparent Statements (+ in-toto/SBOM/C2PA/GG payloads), **embeds an existing policy engine (OPA/CUE)**, and answers *"trust X for purpose P, now?"* — but its differentiation is **narrow and brand-dependent**: an *inspectable evidence subgraph* rather than a boolean gate or opaque score (the SIL/Reveal angle), cross-domain, and selective-disclosure-aware. Policy-as-code over attestations is already mature (OPA/Cosign/Sigstore/Kyverno) and trust *scoring* products exist — so the bet is on **inspection + cross-domain + selective disclosure + purpose-as-query**, not on the evaluation itself. "A Logic of Sattestation" supplies the formal backbone; GG's graph-level selective disclosure + cross-domain semantic model are the two additive pieces worth keeping.

---

## The three pivotal findings

### 1. SCITT is content-agnostic — so GenesisGraph is a *payload*, not a competitor
SCITT's Signed Statement is a `COSE_Sign1` with mandatory CWT claims `iss` (issuer) + `sub` (artifact subject), and the **payload is anything** — "Statements are content-agnostic"; suggested payloads explicitly include in-toto, SBOM, SLSA, CoSWID, CycloneDX, SPDX. **A GenesisGraph document is a valid SCITT payload.** SCITT then provides — for free — the Transparency Service, Receipts (inclusion proofs), append-only/non-equivocation guarantees, federation (one statement → many Transparency Services → many Receipts), and a published Registration Policy. *This is exactly the transparency/registry layer GG only half-built (verifier + Rekor client, no hosted log).* GG should ride it, mapping `attestation.signer → iss`, `entity hash → sub`.

### 2. SCITT explicitly does NOT decide trust — that's the white space
The Transparency Service guarantees registration, non-equivocation, replayability — and **explicitly disclaims truthfulness and completeness**: *"Issuers can make false Statements"*; *"Issuers can refuse to register their Statements."* The trust decision is delegated to the Relying Party, which "MAY apply arbitrary validation policies using Envelope data, Receipt, payload, and local state." **Nobody is building a good, inspectable, purpose-scoped Relying-Party policy/reasoning engine.** That is the product. SCITT generalizes Certificate Transparency; we are building the *trust-reasoning layer CT/SCITT deliberately leave open.*

### 3. SCITT's privacy is thin; DPP's selective disclosure is underspecified — GG's A/B/C fits the gap
SCITT selective disclosure = detached/hash-only payloads. No encryption, no BBS+, no ZK. Meanwhile the **EU DPP** mandates tiered access (public / authority-restricted / proprietary) to *"prove compliance to a regulator without exposing the confidential supplier list,"* but — per the requirements read — **"specific implementation mechanisms for cryptographic separation or role-based encryption are not detailed."** So selective disclosure is a real, underserved need at both layers. GG's A/B/C (once it's *actually* built, not mocked) is additive privacy on top of SCITT and a candidate mechanism for DPP's tiered access.

---

## CONSUME (ride on — do not rebuild)

| Capability | Ride on | Why |
|---|---|---|
| Transparency log / registry / receipts | **IETF SCITT** Transparency Service + Receipts | Content-agnostic, federated, IETF-backed; generalizes CT. GG stops hosting a log |
| Statement envelope | **COSE_Sign1 / CBOR / CWT** (`iss`,`sub`) | SCITT-mandated; the price of interop |
| Signing | **Sigstore / Microsoft Artifact Signing** | Owned; keyless OIDC / cloud HSM |
| Build/dep provenance payloads | **in-toto, SLSA, SBOM (CycloneDX/SPDX), AIBOM** | All valid SCITT payloads; ingest, don't redefine |
| Media / AI-output provenance | **C2PA** | Won the domain; link only |
| Agent identity / reputation registry | **ERC-8004 / Agent Passport** | Off-chain complement, not competitor |
| Credential envelope | **W3C VC Data Model v2.1** | Align the `verifiable` mode |
| DPP rails | **GS1 Digital Link, CIRPASS-2 ontology, EBSI** | The mandated identifier/ontology/verification layer |
| Trust-reasoning formalism | **"A Logic of Sattestation"** | Labels = our purpose/context; delegation-w.r.t.-label = purpose-scoped chains; gives a *sound, complete, terminating* derivation algorithm |
| **Policy evaluation engine** | **OPA/Rego or CUE** | Policy-as-code over attestations is *mature* (Cosign, Sigstore policy-controller, Kyverno all use it). Do **not** build a policy evaluator — embed one and add the layer it lacks |

## BUILD (genuinely ours)

| What | Leans on | Differentiation |
|---|---|---|
| **Relying-Party trust-reasoning engine** — purpose-scoped, multi-hop, inspectable; consumes SCITT Transparent Statements + payloads; **embeds OPA/CUE for evaluation**; returns an **evidence subgraph** for "trust X for purpose P?" | SCITT (input), OPA/CUE (evaluation), Sattestation (logic), Reveal (rendering) | ⚠️ **NOT empty white space** — policy-as-code over attestations is mature (OPA/Cosign/Sigstore/Kyverno), and contextual trust *scoring* exists (Aembit, Strike Graph "Trust Chain"). The narrow, surviving differentiation: **inspectable evidence subgraph (not a boolean gate or opaque score) + cross-domain (not container/CI-bound) + selective-disclosure-aware + purpose-as-query** |
| **Selective-disclosure layer richer than hash-only** — GG A/B/C as real crypto, additive over SCITT; candidate for DPP tiered access | GG (fixed), SD-JWT/BBS+ (real libs) | SCITT lacks it; DPP under-specifies it |
| **GG cross-domain semantic statement** — atoms+bits+agents with capability/loss, as a SCITT payload type | SCITT envelope, PROV-O mapping | The one GG piece no standard owns |

## DON'T BUILD (someone owns it)

- A transparency log / registry → SCITT, Sigstore/Rekor, EBSI.
- A signing service → Sigstore, Microsoft Artifact Signing.
- A policy-evaluation engine → OPA/Rego, CUE (embed one).
- An admission controller → Sigstore policy-controller, Kyverno.
- Media provenance → C2PA.
- On-chain agent identity/reputation → ERC-8004.
- A DPP registry → EU Central Registry / GS1.

---

## The GenesisGraph reframe that falls out

> **GenesisGraph = a content-agnostic, cross-domain, selectively-disclosable SCITT *statement payload*, plus the Relying-Party trust-reasoning engine over it.**

This single reframe:
- **Dissolves the "GG half-built a transparency log" gap** — use SCITT's.
- **Gives GG distribution** — rides an IETF standard that will be everywhere.
- **Focuses GG on its only true differentiation** — the semantic model + selective disclosure + reasoning, dropping the registry/signing ambitions it can't win.
- **Makes the overclaim cleanup non-optional** — if GG payloads ride SCITT into auditable contexts, the mocked ZK / non-resolving DIDs / `mock:`-signature-bypass become liabilities, not roadmap footnotes.

## Recommended sequence

> **Update (session `topaz-ray-0622`): both go/no-go tests below have now run — see [`TRUST_SPIKE_RESULTS.md`](TRUST_SPIKE_RESULTS.md).** Both passed, but the falsification test narrowed the thesis a *third* time: the evidence subgraph is produced **by OPA itself** (a Rego rule), so the honest product is a **normalization + Rego-policy-library + Reveal-renderer layer on OPA**, not a distinct "trust-reasoning engine." Read "trust-reasoning engine/layer" below with that correction in mind.

**Falsify before you build.** The "white space" narrowed twice under research scrutiny (SCITT owns the registry; OPA/Sigstore own policy evaluation). So the next moves are two cheap go/no-go *tests of the surviving thesis*, not a build:

0a. **Falsification test (do first).** One worked **MCP-tool-trust** example end-to-end: real attestations (a GG doc + an in-toto/SCITT statement) → evaluate with **OPA/Rego** → render the **evidence subgraph via Reveal**. Then answer honestly: *is the inspectable subgraph + purpose-query meaningfully better than OPA's pass/fail for a real user?* If not, the BUILD thesis is thin — learned in a day, not a quarter. (~1 day)

0b. **Keystone spike.** Wrap one GG document as a SCITT `COSE_Sign1` (`signer→iss`, `entity→sub`) and verify the round-trip. Tests the single load-bearing assumption ("GG = SCITT payload"). (~hours)

If both tests pass, then:

1. **GG honesty + SCITT-payload reframe** (foundational, mostly docs + a real risk-reducer): align claims to code, **gate the `# Skip signature check for demo` / `mock:` bypass to tests-only** (this is a live liability in a *trust* product, worth doing regardless of the tests), downgrade "ZK"/"BBS+" claims to "commitment + binding," and rewrite GG positioning as "SCITT payload + reasoning engine."
2. **Trust-reasoning engine MVP** (the product): a small, auditable, separate tool ([placement doc](TRUST_TOOLING_PLACEMENT.md)) that ingests a SCITT Transparent Statement (or a bare GG/in-toto doc), **embeds OPA/CUE for evaluation**, and answers one purpose-scoped trust query → evidence subgraph. **Beachhead: MCP-tool trust.** Re-expose via `reveal trust`.
3. **Reveal Windows wedge** (parallel, independent): ship the signed-binary path; first dogfood case study.
4. **DPP-feeder (long game, optional):** GG→DPP profile with A/B/C as the tiered-access mechanism. High regulatory upside, slow timeline (mandatory 2027–2030), incumbent rails (CIRPASS/GS1) — pursue only if a real DPP-facing user appears.

## Honest caveats / risks

- **SCITT is still an I-D** (architecture draft, not final RFC). Aligning now is a bet on its trajectory — strong (IETF WG, generalizes CT, multi-vendor) but not zero-risk. Mitigate: depend on COSE/CWT (already STDs) and treat SCITT specifics as a thin adapter.
- **The reasoning engine is the unproven part** — Sattestation is theory (about addresses, not artifacts/agents); generalizing it to a usable, performant query engine is real research, not glue.
- **Fixing GG's crypto honestly is non-trivial** (real BBS+/ZK needs pairing libs the repo doesn't have). The interim honest move is to *downgrade the claims*, not fake the crypto.
- **DPP is a slow, crowded, regulatory arena** — treat as optional optionality, not a near-term build.
- **The reasoning engine's differentiation is narrow and unproven.** OPA/Sigstore/Kyverno already gate on attestations; Aembit/Strike Graph already score contextual trust. The bet rests entirely on the *inspectable-subgraph + cross-domain + selective-disclosure + purpose-query* combination being meaningfully better than a boolean/score for real users. **This must be falsified early** (see worked-example test in next steps) — if the subgraph output doesn't visibly beat an OPA pass/fail on a real case, the thesis is thin.

## Sources
[SCITT architecture I-D](https://ietf-wg-scitt.github.io/draft-ietf-scitt-architecture/draft-ietf-scitt-architecture.html) · [SCITT datatracker](https://datatracker.ietf.org/doc/draft-ietf-scitt-architecture/) · [scitt.io](https://scitt.io/) · [A Logic of Sattestation](https://arxiv.org/abs/2405.01809) · [EU DPP requirements](https://www.fiegenbaum.solutions/en/blog/digital-product-passport-from-european-regulation-to-global-standard) · [DPP by sector](https://www.circularise.com/blogs/dpps-required-by-eu-legislation-across-sectors) · [Sigstore/in-toto/OPA orientation](https://www.trustification.io/blog/2023/01/11/sigstore-in-toto-opa/) · [Sigstore policy-controller](https://docs.sigstore.dev/policy-controller/overview/) · [Policy-as-code in the supply chain (CNCF)](https://www.cncf.io/blog/2024/02/14/policy-as-code-in-the-software-supply-chain/) · [Aembit attestation-based identity](https://aembit.io/blog/attestation-based-identity-hardware-cloud-security/) · [Strike Graph Trust Chain](https://www.businesswire.com/news/home/20260506506216/en/Strike-Graph-Launches-Trust-Chain-Bringing-AI-Evidence-Validation-to-Third-Party-Risk-Management)
