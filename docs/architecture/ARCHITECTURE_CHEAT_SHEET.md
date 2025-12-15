---
title: SIL Architecture Cheat Sheet
type: reference
status: current
created: 2025-12-15
session_id: turbulent-current-1215
beth_topics:
  - sil-architecture
  - invariants
  - messaging
  - quick-reference
summary: One-page reference for SIL's architectural philosophy and decision framework
---

# SIL Architecture Cheat Sheet

**Use this for:** Quick reference, stakeholder conversations, design reviews.

---

## The Core Insight

**Two frames, one architecture:**

| Frame | Purpose | Question It Answers |
|-------|---------|---------------------|
| **Layers** | Organization | "Where does this code belong?" |
| **Invariants** | Mission | "Does this prevent the worst day?" |

---

## The Five Invariants

These must hold everywhere. They prevent the structural failures SIL committed to fix.

| Invariant | Prevents | Enforced By | Status |
|-----------|----------|-------------|--------|
| Everything has lineage | Epistemic collapse | GenesisGraph | Production |
| Reasoning is inspectable | Black box dictatorship | Reveal | Production |
| Computation is grounded | Hallucinated science | Morphogen | Prototype |
| Contracts are explicit | Brittle complexity | Agent Ether | **Gap** |
| Efficiency is sustainable | Compute as privilege | Beth + Reveal | Production |

---

## The Chief Scientist Test

Before approving any architectural decision, ask:

1. **Lineage:** Does this maintain traceable provenance?
2. **Transparency:** Is reasoning inspectable?
3. **Grounding:** Is computation connected to reality?
4. **Contracts:** Are assumptions explicit?
5. **Efficiency:** Is this sustainable at scale?
6. **Agency:** Does this keep humans as conductors?

If any answer is "no" or "unclear," revise the architecture.

---

## The Provenance-First Stack

For organizational questions ("where does this go?"):

```
L6: Reflection    - Learning from execution (observability)
L5: Execution     - Doing work under constraints (agents)
L4: Composition   - Cross-domain integration (Pantheon IR)
L3: Intent        - What we're accomplishing (contracts)
L2: Trust         - Who can do what (authorization)
L1: Meaning       - Embeddings, types, similarity (Beth)
L0: Provenance    - Everything has lineage (GenesisGraph)
───────────────────────────────────────────────────────
L-1: Substrate    - Physical/computational reality (optional)
```

---

## Key Phrases

**For funders/partners:**
> "We build transparent infrastructure for AI—semantic foundations that make reasoning visible, provenance verifiable, and intelligence grounded in reality."

**For technical audiences:**
> "Use layers for organization, invariants for mission. Five guarantees that must hold everywhere."

**The differentiator:**
> "AGI as colleague, not god. Integrated into human trust frameworks, not transcending them."

---

## Quick Reference

| If you need to... | Go to... |
|-------------------|----------|
| Understand why SIL exists | THE_FORK.md |
| See the full invariants proposal | INVARIANTS_OVER_LAYERS.md |
| Navigate reading paths | SYNTHESIS_MAP.md |
| See layer model comparison | LAYER_MODELS_COMPARISON.md |
| Check implementation status | models/README.md |

---

## The One Sentence

**SIL builds the semantic substrate that makes AI transparent, grounded, and trustworthy—preventing epistemic collapse, not by stopping AI, but by building the steel beneath the wood.**

---

*Last updated: 2025-12-15 (turbulent-current-1215)*
