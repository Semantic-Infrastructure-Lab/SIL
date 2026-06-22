---
title: "Where Trust Tooling Should Live: Separate Tool, Re-exposed via Reveal"
type: research
status: draft-decision
author: "Scott Senkeresty"
created: 2026-06-22
session: mint-crystal-0622
beth_topics:
  - SIL
  - reveal
  - genesisgraph
  - trust
  - architecture
  - tooling
related_projects: [reveal, genesisgraph, SIL]
---

# Where Trust Tooling Should Live

> Companion to [`SOFTWARE_TRUST_AI_ERA.md`](SOFTWARE_TRUST_AI_ERA.md) (landscape) and [`TRUST_MINIMUM_SEMANTIC_MODEL.md`](TRUST_MINIMUM_SEMANTIC_MODEL.md) (the model). This doc records the **placement decision** for the trust-verification capability.

## The question

Should "show me why I should trust X" be a Reveal feature (`reveal trust` / `trust://` adapter), or its own tool?

## The decision

**A separate, narrowly-scoped trust tool that exposes trust semantics — re-exposed *through* Reveal where integration is wanted.** (Scott: *"a separate tool to expose trust semantics could just be re-exposed via Reveal if we want that integration."*)

The three layers, cleanly separated:

```
GenesisGraph (library)   →  provenance + attestation substrate; assertions point INTO it
        ↓ consumed by
Trust tool (new, small)  →  CONSUMES GG + SCITT + in-toto/Sigstore + C2PA + VC/SBOM
                            (never reinvents them); normalizes into trust atoms;
                            answers purpose-scoped trust queries → evidence subgraph
        ↓ optionally re-exposed by
Reveal (surface)         →  `reveal trust …` / a trust:// adapter wraps the tool for
                            progressive-disclosure rendering + distribution reach
```

## Why separate, not built-in

The decisive argument is almost a pun but it's real: **a trust verifier must itself be easy to trust.**

1. **Auditability scales inversely with surface area.** Reveal is large (23+ adapters, 73 rules, 190+ languages, ~6,800 tests). A security-critical verifier buried in that surface is hard for a third party — or an enterprise security team — to audit in isolation. A small single-purpose tool can be read end-to-end. *Trustworthiness is a function of how little you have to trust.*
2. **Blast radius.** A trust tool will touch crypto, DID resolution, network fetches (Rekor, did:web), and signature verification — exactly the code where a bug is a vulnerability, not an inconvenience. Coupling that to Reveal's release cadence and dependency tree enlarges Reveal's attack surface and CVE exposure for a concern most Reveal users don't have.
3. **Independent provenance story.** The trust tool should be able to make *its own* clean trust claims (small dependency set, reproducible build, signed, attested). Easier to achieve and demonstrate for a focused binary than for a large multipurpose CLI.
4. **Distinct dependency & trust boundary.** Reveal is deliberately local-first/zero-network for its core. A trust verifier is inherently network-touching (revocation, transparency logs, DID resolution). Different trust boundary → different tool.
5. **Composability over absorption (the SIL pattern).** SIL systems are adapters/IRs that compose; the trust tool consuming GG and being consumed by Reveal *is* the architecture working as designed.

## Why re-expose through Reveal (don't ignore it)

Reveal is the **distribution wedge and the inspection brand** the trust tool otherwise lacks:
- Reveal's whole identity is progressive disclosure — rendering an evidence subgraph "structure first, drill on demand" is exactly on-brand.
- `reveal trust <artifact>` / a `trust://` adapter gives the capability reach into existing Reveal/MCP workflows without the trust core living inside Reveal.
- The integration is a **thin wrapper** (adapter calls the trust tool's library/CLI), not a merge — preserving the separation above.

## Implications / guardrails

- **Trust tool = its own repo + its own binary**, minimal dependencies, reproducible + signed build (it should pass its own `reveal trust`).
- **GG stays a library/substrate** — the trust tool depends on GG for provenance/attestation parsing; GG does not depend on the trust tool.
- **Reveal integration is optional and thin** — an adapter/subcommand that shells out to or imports the trust tool, never a fork of the logic.
- **Naming:** keep the tool's name distinct from Reveal so its audit boundary is obvious; `reveal trust` is just the *surface alias*.
- **Standards posture:** the tool's value is *consuming* SCITT signed statements, in-toto/Sigstore attestations, C2PA manifests, W3C VCs, and GG provenance — and reasoning over them with purpose + selective disclosure. It must not become another signer/registry/format (see [`../../../genesisgraph/docs/strategic/TRUST_INTEROP_MATRIX.md`](../../../genesisgraph/docs/strategic/TRUST_INTEROP_MATRIX.md)).

## Open questions
- Does the trust tool ship a library + thin CLI (so Reveal imports the library directly, no subprocess)? Probably yes — mirrors how reveal-mcp wraps reveal.
- MCP exposure: a dedicated trust MCP tool, or fold into reveal-mcp? Likely dedicated first (same auditability logic), bridged later.
- First milestone: the trust *query* over normalized atoms for one concrete case — **MCP-tool trust** is the recommended beachhead (acute, narrow, ours). See landscape doc.
