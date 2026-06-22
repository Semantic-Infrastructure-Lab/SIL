---
title: "Software Trust in the AI Era: Landscape, Bets, and Tools to Build"
type: research
status: idea-capture
author: "Scott Senkeresty"
created: 2026-06-22
session: mint-crystal-0622
session_provenance: "Captured from a ChatGPT exploration (Scott, while driving) + web research + repo verification"
beth_topics:
  - SIL
  - genesisgraph
  - reveal
  - trust
  - provenance
  - software-supply-chain
  - code-signing
  - agent-identity
  - verifiable-credentials
  - selective-disclosure
related_projects: [genesisgraph, reveal, SIL, agent-ether]
---

# Software Trust in the AI Era: Landscape, Bets, and Tools to Build

> *"Provenance ≠ trust. A valid attestation on a malicious package is still malicious. The question isn't 'who signed this?' — it's 'why should I trust this, for this purpose, right now?'"*

**What this is.** An idea-capture doc. It started as a Reveal-CLI Windows code-signing headache and expanded — through a ChatGPT thread and follow-up research — into a question about how software (human- and agent-authored) earns trust in an era where AI lets millions of tiny publishers ship daily. This doc records the interesting problems, maps them against real existing work (so we know where the white space actually is), and lists research areas and tools worth building.

---

## ⚠️ Provenance note: a confabulation to ignore

The originating ChatGPT thread attributed several specific features to GenesisGraph that **do not exist in the repo**:

- ❌ "Trust Assertion Protocol (TAP)"
- ❌ "Semantic Passports"
- ❌ `trust_assertion` typed graph edges with `did:key` issuers
- ❌ a "Trust Layer Integration" doc section

These were hallucinated and then fed back as evidence that GG is "already well-positioned." **Do not cite them.** What GenesisGraph *actually* has (verified 2026-06-22) is a stronger real foundation:

- **Four-pillar core model**: Entity · Operation · Tool/Agent · Attestation (`docs/specifications/main-spec.md` §4)
- **Capability vs. actuality** — "what a tool *can* do vs. what it *did* do" (§5.1), plus fidelity/loss tracking (§5.2)
- **Real SSI machinery, coded**: did:web / did:ion / did:ethr resolvers, SD-JWT, BBS+ selective disclosure, ZKP templates, transparency log, predicate attestations
- **A/B/C selective disclosure** (Full / Partial / Sealed)
- An explicit **"Interoperability With Existing Standards"** section (PROV-O) — §8

The *spirit* of "GG is a trust substrate, not just a provenance format" is defensible — but from these real primitives, not the invented protocol names.

---

## The three nested problems (keep them separate)

The conversation conflated three distinct problems that need three distinct decisions:

| | Problem | Status / verdict |
|---|---|---|
| **P1 — Tactical** | Reveal-CLI can't earn Windows trust: `python.exe` is signed, the source isn't. SmartScreen reputation + AV false positives. | **Solved today.** Nuitka → Microsoft Artifact Signing → winget. Ship it. Not a business. |
| **P2 — Workflow** | "One command to produce a trusted artifact" for AI-generated tools (SBOM + sign + scan + provenance + publish). | Real gap for solo/small devs. But the *primitives* are crowded; hard to monetize as glue. |
| **P3 — Strategic** | "Why should I trust this artifact / agent / person *for this purpose*?" — as inspectable, queryable evidence. | The SIL-shaped idea. **No longer the empty white space the thread assumed** (see landscape). |

Key throughline (and the SIL-aligned thesis): **trust today is opaque scoring** (SmartScreen, GitHub stars, app stores). SIL's move — *make it explicit, traceable, inspectable* — applies directly. "Trust me" → "inspect me."

---

## Landscape / related work (the thread was ~a year out of date)

The thread repeatedly claimed "nobody owns this space." As of mid-2026 that is wrong — it's a land rush. Mapping what exists is how we find the *real* white space.

### Software supply-chain provenance (mature, standardized)
- **Sigstore** — keyless OIDC signing; Fulcio certs; Rekor transparency log. [overview](https://aquilax.ai/blog/supply-chain-artifact-signing-slsa)
- **SLSA** — build-provenance levels. The 2026 *Shai-Hulud* npm attack carried **valid** SLSA provenance on compromised packages → proves provenance ≠ trust (our core point). [practical guide](https://www.practical-devsecops.com/slsa-framework-guide-software-supply-chain-security/)
- **in-toto (ITE-6)** — attestation envelope = `subject` + `predicate`. **Structurally identical to GG's Attestation pillar and to W3C VCs.** Strong interop signal.
- **GitHub artifact attestations**, **npm provenance** — provenance now shipping in mainstream developer platforms. [InfoQ](https://www.infoq.com/news/2025/08/provenance/)
- **SBOM** — CycloneDX / SPDX; CycloneDX now has an **ML-BOM / AI-BOM** profile.
- **IETF SCITT** (Supply Chain Integrity, Transparency, and Trust) — the **standards-body capstone**: interoperable building blocks for transparent, accountable supply-chain attestation. Active architecture I-D; roadmap explicitly includes *privacy for sensitive attestations, cross-registry federation, and automated policy languages for complex trust requirements* — i.e. the institutional version of GG's transparency + trust-policy layer. **Align/consume, don't reinvent.** [scitt.io](https://scitt.io/) · [architecture I-D](https://datatracker.ietf.org/doc/draft-ietf-scitt-architecture/)

### AI agent identity / trust (exploding right now)
- **ERC-8004 "Trustless Agents"** — MetaMask + Ethereum Foundation + Google + Coinbase; **live on Ethereum mainnet Jan 29 2026**. Three on-chain registries: Identity / Reputation / Validation. This *is* the "trust graph for agents," backed by heavyweights. [explainer](https://www.allium.so/blog/onchain-ai-identity-what-erc-8004-unlocks-for-agent-infrastructure/) · [magicians thread](https://ethereum-magicians.org/t/erc-8004-trustless-agents/25098)
- **Agent Passport** (Cubitrek, Apr 2026) — `/.well-known/agent-passport.json`, Ed25519 + DNS. [spec post](https://cubitrek.com/blog/agent-passport)
- **Indicio ProvenAI** — VC-based agent identity; **RNWY** soulbound (non-transferable) passports. [Indicio](https://indicio.tech/blog/why-verifiable-credentials-will-power-ai-in-2026/)

### MCP / agent-tool trust (fresh, acute, under-tooled)
- First in-the-wild **malicious MCP server** (postmark-mcp backdoor, Sept 2025); **tool-name shadowing**; consensus that "no centralized trust mechanism exists — manual review is still the most reliable method." [MCP supply-chain risk](https://tianpan.co/blog/2026-04-10-mcp-server-supply-chain-risk) · [state of MCP security 2026](https://pipelab.org/blog/state-of-mcp-security-2026/)
- This is the freshest open wound **and our daily world** (we build tools for agents). See [`AGENTIC_INCIDENTS.md`](AGENTIC_INCIDENTS.md).

### Windows specifics (for P1)
- **Nuitka** — true compiler (Python → C → binary); far fewer AV false positives than PyInstaller (whose bootloader is on AV threat lists). [comparison](https://dev.to/weisshufer/from-pyinstaller-to-nuitka-convert-python-to-exe-without-false-positives-19jf)
- **Microsoft Artifact Signing** (formerly Trusted Signing) — GA April 2026, **now open to self-employed individuals** (old 3-year-history requirement dropped). CI/CD-native, no hardware token. Directly relevant. [walkthrough](https://melatonin.dev/blog/code-signing-on-windows-with-azure-trusted-signing/)
- **Caveat**: an April 2026 intermediate-CA change regressed SmartScreen *even for signed binaries*. Signing helps; it does not instantly buy reputation. [SmartScreen reputation](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation)

### Regulatory forcing functions (2026–2027) — tailwinds, not competitors
- **EU Digital Product Passport (DPP)** — ESPR-mandated; **Central DPP Registry go-live July 19 2026**, sector rollouts through 2027. The official framing is *GG's exact pitch*: "selective disclosure protocols let manufacturers **prove compliance to a regulator without exposing their confidential supplier list to competitors.**" Every EU-selling manufacturer will be forced to do atoms-provenance + selective disclosure. **This is GG's strongest, most concrete, most timely use case** — stronger than the software-CLI angle that started this thread. Build *to* it (feed DPP), don't try to be the registry. [overview](https://www.fiegenbaum.solutions/en/blog/digital-product-passport-from-european-regulation-to-global-standard) · [sectors/legislation](https://www.circularise.com/blogs/dpps-required-by-eu-legislation-across-sectors)
- **EU AI Act (effective Aug 2026)** — requires transparency labeling for AI-generated content; **C2PA's AI assertion type directly satisfies it.** Plus US Digital Authenticity & Provenance Act (2025), CISA C2PA advisory. Regulation is pulling provenance from "nice-to-have" to "mandatory."

### Adjacent standards to track
- **C2PA / Content Credentials** — **this is now WON for media / AI-output provenance.** 6,000+ members (Jan 2026); OpenAI, Google DeepMind (SynthID, 20B+ images), Meta, Adobe, Amazon embed it; native signing on Leica/Samsung S25/Pixel 10; the EU AI Act / US law make it the de-facto requirement. **GG must link to C2PA (its planned `gg-media-c2pa-v1`), never compete on media.** [adoption status](https://www.eyesift.com/faq/c2pa-content-credentials-2026-cryptographic-provenance-adoption/) · [state of content authenticity 2026](https://contentauthenticity.org/blog/the-state-of-content-authenticity-in-2026)
- **W3C Verifiable Credentials Data Model v2.1** — First Public Working Draft published 2026; align GG's `verifiable` attestation mode + DID usage. [W3C news](https://www.w3.org/news/2026/first-public-working-draft-verifiable-credentials-data-model-v2-1/)
- See also [`IDENTITY_MAPPING.md`](IDENTITY_MAPPING.md) (cross-domain identity resolution) and [`../architecture/models/PROVENANCE_FIRST.md`](../architecture/models/PROVENANCE_FIRST.md).

---

## Where the real white space is (for us)

**Post-research refinement (the map got clearer):** the *provenance* layer is now crowded by institutions and regulation — **SCITT** owns transparent attestation registries, **C2PA** has won media/AI-output provenance, **Sigstore/SLSA/in-toto** own software signing/build provenance, **DPP** owns regulated physical-product provenance. Trying to compete in any of those is a loser. **What remains under-built is the layer *above* them: contextual, purpose-scoped trust *reasoning* that consumes all of them.** The academic backbone for this exists but is early — see *"A Logic of Sattestation"* (reasoning about contextual trust grounded in the attesting principal) and the recognition that *"trust is not binary; frameworks contextualize identical signatures with different assurance levels."* That's the air.

> **⚠️ Spike refinement (`topaz-ray-0622`, see [`TRUST_SPIKE_RESULTS.md`](TRUST_SPIKE_RESULTS.md)):** "trust *reasoning* is under-built" was the overstatement here. A worked MCP-tool-trust example showed **OPA/Rego already does the reasoning *and* emits the evidence subgraph** as a plain policy rule. The genuinely under-built piece is narrower: **cross-domain *normalization*** (GG/in-toto/C2PA/SBOM → one attestation shape) feeding OPA, plus **inspectable rendering** of OPA's output. So "reasoning, not registries" (angle 1 below) survives only as *normalization + rendering on OPA*, not as a novel reasoning engine. Angle 2 (`reveal trust`) and angle 3 (MCP beachhead) are unaffected.

Competing head-on with ERC-8004 (on-chain registries) or Sigstore (signing) is a loser. Three differentiated angles fit SIL's philosophy *and* lean on assets we already have:

1. **Trust *reasoning*, not trust *registries*.** Everyone above gives signatures, registries, attestations. Almost nobody gives *"show me the multi-hop evidence chain for trusting X for this specific purpose, with selective disclosure."* That's the SIL thesis applied to trust, and the searches confirm only early academic work (*Logic of Sattestation*) and nascent "trust graph" writing occupy it. GG's selective-disclosure + capability + attestation primitives are the right substrate.

2. **`reveal trust <artifact>` — inspection, not assertion.** Reveal's whole brand *is* progressive disclosure. A command that *consumes* in-toto / Sigstore / SBOM / GG and renders the provenance + trust graph progressively is on-brand, and Reveal is the **distribution wedge GG lacks**. The Shai-Hulud lesson is the pitch: don't trust the badge, inspect the evidence.

3. **MCP / agent-tool trust as the beachhead.** Narrow, acute, unsolved, and ours. "Provenance + capability verification for the tools your agent is about to call" is a far more defensible first GG use case than "trust fabric for everything."

**Architectural principle to commit to:** *GenesisGraph consumes and interops; it does not reinvent.* The Attestation pillar is the same shape as in-toto ITE-6 and VCs — be the **semantic / reasoning + selective-disclosure layer on top**, not another signer or registry.

---

## Research areas to drill

- **Consume-vs-compete boundary** vs. ERC-8004 / Agent Passport / Sigstore: where does GG complement (off-chain, semantic, selective-disclosure) and where must it *not* compete?
- **"Minimum semantic representation of trust."** The thread's best question: is trust always `Entity · Claim · Evidence · Provenance · Relationship · Context · Time`? If so, VCs, code signatures, agent passports, and reputation all become instances of one structure. Does this collapse cleanly onto GG's four pillars?
- **Trust as a *query*** — "why should I trust X for purpose P?" returns an evidence subgraph. What's the query language / output shape? (Natural fit for Reveal's URI-adapter model: a `trust://` adapter?)
- **Agent + prompt provenance** — model id, prompt lineage, human-review flag, dependency deltas as first-class attestation subjects. (GG roadmap already targets "AI agent provenance" at v0.7.0, Aug 2026 — this is the hook.)
- **Reputation vs. credentials** — static credentials ("worked at Microsoft") vs. dynamic behavior ("40 releases, 0 malware incidents"). Behavior is often more predictive. How does GG represent accumulating reputation without becoming a registry?
- **Selective disclosure for trust** — prove "reviewed by a trusted maintainer" without naming them; prove "no critical CVEs" without the full SBOM. GG's A/B/C + BBS+ already aim here.

## Tools / products that could be built

- **`reveal trust <artifact>`** — progressive-disclosure trust inspector. Ingests Sigstore/Rekor + in-toto + SBOM + GG docs; renders identity, build provenance, dependency tree, scan results, signing chain, review chain. (The strongest near-term wedge: real, shippable, on-brand.)
- **`<tool> release` pipeline** (P2) — one command → dependency audit + SBOM + signed build + malware scan + provenance attestation + GitHub release + winget update + a human-readable **trust report**. "Let's Encrypt for software trust" for solo devs. (Crowded primitives, but the *orchestration + report* is the value.)
- **MCP-tool trust gate** — before an agent calls a tool, check its provenance/capability attestation. Concrete GG v0.7 deliverable; directly defends our own agent stack.
- **GG ⇄ in-toto / Sigstore / CycloneDX adapters** — make GG a consumer/translator of existing attestations, not a parallel format.
- **Reveal-on-Windows reference build** (P1) — Nuitka + Artifact Signing + winget; double as GG's **first dogfood case study** ("we felt this exact pain; here's the receipt").

## Open questions

- Can trust become **graph-native instead of certificate-native** — and is that GG's actual identity, distinct from its "provenance format" framing today?
- Is the winning play a **product** (`reveal trust`), a **standard** (GG attestation interop), or a **research artifact** (the semantic-trust model)? Possibly all three in sequence, product-first for distribution.
- Does competing in agent identity at all make sense post-ERC-8004, or is the only sane lane the **off-chain semantic/selective-disclosure layer** that on-chain registries can't provide?

---

## Sources

- Supply chain: [Sigstore/SLSA/in-toto](https://aquilax.ai/blog/supply-chain-artifact-signing-slsa) · [SLSA guide](https://www.practical-devsecops.com/slsa-framework-guide-software-supply-chain-security/) · [provenance mainstreaming](https://www.infoq.com/news/2025/08/provenance/)
- Agent identity: [ERC-8004 explainer](https://www.allium.so/blog/onchain-ai-identity-what-erc-8004-unlocks-for-agent-infrastructure/) · [ERC-8004 magicians](https://ethereum-magicians.org/t/erc-8004-trustless-agents/25098) · [Agent Passport](https://cubitrek.com/blog/agent-passport) · [Indicio VC+AI](https://indicio.tech/blog/why-verifiable-credentials-will-power-ai-in-2026/)
- MCP trust: [supply-chain risk](https://tianpan.co/blog/2026-04-10-mcp-server-supply-chain-risk) · [state of MCP security 2026](https://pipelab.org/blog/state-of-mcp-security-2026/)
- Windows: [Artifact/Trusted Signing](https://melatonin.dev/blog/code-signing-on-windows-with-azure-trusted-signing/) · [SmartScreen reputation](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation) · [Nuitka vs PyInstaller](https://dev.to/weisshufer/from-pyinstaller-to-nuitka-convert-python-to-exe-without-false-positives-19jf)
- Standards & regulation: [IETF SCITT](https://scitt.io/) · [SCITT architecture I-D](https://datatracker.ietf.org/doc/draft-ietf-scitt-architecture/) · [EU DPP guide](https://www.fiegenbaum.solutions/en/blog/digital-product-passport-from-european-regulation-to-global-standard) · [DPP by sector](https://www.circularise.com/blogs/dpps-required-by-eu-legislation-across-sectors) · [C2PA adoption 2026](https://www.eyesift.com/faq/c2pa-content-credentials-2026-cryptographic-provenance-adoption/) · [W3C VC 2.1 FPWD](https://www.w3.org/news/2026/first-public-working-draft-verifiable-credentials-data-model-v2-1/)
- Trust-reasoning theory: [A Logic of Sattestation (arXiv 2405.01809)](https://arxiv.org/pdf/2405.01809) · [The Trust Graph — The Proof Gap](https://thetrustgraph.substack.com/p/the-proof-gap)
