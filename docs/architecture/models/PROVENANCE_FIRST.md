# Provenance-First Architecture

**Version:** 1.0
**Date:** 2025-12-15
**Status:** Proposal for evaluation
**Origin:** Session `heating-snow-1214`

---

## The Core Thesis

> The current 7-layer Cognitive OSI Stack was designed **product-centric**
> (where do our 12 projects fit?) rather than **problem-centric**
> (what abstractions solve LLM coexistence?).

If we start from problems instead of products, **Provenance** emerges as L0, not Philbrick hardware.

---

## The Problem Space

The Semantic OS must solve:

1. **Decomposing intent into trackable work** - How do we know what we're trying to do?
2. **Trust relationships for LLM/AGI coexistence** - How do humans and AI work together safely?
3. **Cross-domain tooling** - How do unrelated domains compose?
4. **Meaning manifolds for analogies** - How do we find similar things across contexts?

The key insight: **Without provenance, you can't trust LLM output.**

---

## The Proposed Model

```
┌─────────────────────────────────────────────────────────────┐
│ L6: REFLECTION                                              │
│ Learning from execution, observability, feedback loops      │
│ Tools: Reveal (structure), metrics, dashboards              │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L5: EXECUTION                                               │
│ Doing work under constraints, agent orchestration           │
│ Tools: Agent Ether, TIA task execution, schedulers          │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L4: COMPOSITION                                             │
│ Cross-domain integration, graph routing, topology           │
│ Tools: Pantheon IR, SUP components, workflow engines        │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L3: INTENT                                                  │
│ What we're accomplishing, contracts, goals, constraints     │
│ Tools: IntentContract, task definitions, acceptance criteria│
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L2: TRUST                                                   │
│ Who can do what, authorization, delegation, capability      │
│ Tools: TAP, AuthorizationGrant, DelegationGrant             │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L1: MEANING                                                 │
│ Embeddings, types, similarity, semantic units               │
│ Tools: Beth (discovery), Pantheon types, domain schemas     │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ L0: PROVENANCE                                              │
│ Everything has lineage - who made what, when, from what     │
│ Tools: GenesisGraph, hash chains, Merkle trees              │
└─────────────────────────────────────────────────────────────┘
```

---

## Unix Philosophy Parallel

**Unix insight:** "Everything is a file"
- Files are the universal abstraction
- Tools compose through file descriptors
- The filesystem is the namespace

**Semantic OS insight:** "Everything has meaning and provenance"
- Semantic objects are the universal abstraction
- Tools compose through typed interfaces
- The provenance graph is the trust foundation

### Fundamental Primitives

| Unix | Semantic OS | Purpose |
|------|-------------|---------|
| File | Semantic Object | Typed, explicit meaning |
| Permission bits | Trust Assertion | Verified capability |
| - | Provenance Record | Lineage, transformations |
| Process | Intent Contract | What we're trying to do |

Unix lacked provenance - you could copy a file, modify it, and lose all history. The Semantic OS makes lineage a first-class concern.

---

## How Tools Fit (Spanning Layers)

Tools are like Unix utilities - they span layers based on function, not architecture:

| Tool | Layers Touched | Function |
|------|----------------|----------|
| **Beth** | L1 (Meaning), L0 (Provenance of knowledge) | Discovery, indexing |
| **Reveal** | L6 (Reflection), L4 (Composition structure) | Inspection, structure |
| **TIA** | L3 (Intent), L5 (Execution), L6 (Reflection) | Orchestration |
| **GenesisGraph** | L0 (Provenance), L2 (Trust chains) | Lineage tracking |
| **Pantheon** | L1 (Types), L4 (Composition) | Semantic IR |
| **Agent Ether** | L5 (Execution), L3 (Intent interpretation) | Agent orchestration |
| **Morphogen** | L4 (Composition), L5 (Execution) | Domain simulation |

This is why component assignment has been chaotic - tools naturally span layers.

---

## Why Provenance at L0?

### The Trust Problem

When an LLM generates code, documentation, or analysis:
- Who prompted it?
- What context did it have?
- What model version?
- What transformations occurred?

Without provenance, you can't answer these questions. Without answers, you can't trust the output.

### The Coexistence Problem

Human-AI collaboration requires:
1. Knowing what came from where
2. Verifying transformations
3. Attributing decisions
4. Rolling back changes

All of these depend on provenance.

### The Philbrick Problem

The current model puts Philbrick hardware at L0 because:
- It's physical
- It's foundational to computing

But Philbrick is:
- A fun hardware project
- Not essential for modern LLM architectures
- Not solving the trust/coexistence problem

**Philbrick becomes an optional backend**, not the foundation.

---

## Layer Dependencies

```
L6 Reflection   ← needs execution results from L5
     ↑
L5 Execution    ← operates within composition from L4
     ↑
L4 Composition  ← composes elements with intent from L3
     ↑
L3 Intent       ← intent authorized by trust from L2
     ↑
L2 Trust        ← trust based on meaning from L1
     ↑
L1 Meaning      ← meaning grounded in provenance from L0
     ↑
L0 Provenance   ← foundation: everything has lineage
```

Each layer depends on the layer below. This dependency chain makes sense:
- You can't trust without knowing meaning
- You can't have intent without trust
- You can't compose without intent
- You can't execute without composition
- You can't reflect without execution

---

## Comparison with Current Model

| Aspect | Current (Glossary) | Provenance-First |
|--------|-------------------|------------------|
| **Foundation** | Physical hardware | Trust/lineage |
| **Problem focus** | Where do projects fit? | How do we coexist with AI? |
| **Trust** | Implicit in Intent | Explicit layer |
| **Tools** | Assigned to layers | Span layers |
| **Philbrick** | L0 foundation | Optional backend |
| **Observability** | Meta-layer | L6 (Reflection) |

---

## Implications

### For GenesisGraph

Becomes foundational infrastructure, not just "provenance tooling." Every operation in the Semantic OS should create provenance records.

### For Trust/Authorization

TAP and the Hierarchical Agency Framework become L2 infrastructure, not just "intent-layer features."

### For Agent Development

Agents operate at L5 (Execution) but must:
- Respect L2 (Trust) constraints
- Express L3 (Intent) contracts
- Produce L6 (Reflection) observability

### For New Projects

When adding a project, ask:
1. What layer does it primarily serve?
2. What layers does it span?
3. How does it relate to provenance?

---

## Open Questions (Addressed)

### 1. Is this too abstract? Does problem-centric lose implementation guidance?

**Answer:** No - it provides *better* guidance.

The brewing-sleet synthesis found "we have more implemented than documented, but critical Layer 1 primitives remain unbuilt." The problem-centric framing tells us *what* to build (Intent Verification, Uncertainty Tracking, Cross-Domain Translation) rather than just *where to put things*.

Implementation guidance comes from the layer dependencies:
- Build L0 (GenesisGraph provenance) first
- Then L1 (Beth meaning/types)
- Then L2 (TAP trust infrastructure)
- Then L3-L6 as needed

### 2. Where does memory fit? The Original model had Semantic Memory at L0.

**Answer:** Memory is implementation, not abstraction.

Semantic memory (storage, indexing) is *how* we implement L1 (Meaning) and L0 (Provenance). It's infrastructure, not architecture. Just as TCP/IP doesn't have a "RAM layer," the Semantic OS doesn't need memory as a layer - it's assumed.

Beth's index, Gemma's storage, and S3/database backends are all L1 implementation details.

### 3. Are 7 layers right? Could Trust and Meaning collapse?

**Answer:** 7 layers is defensible; collapsing would lose important distinctions.

- **Trust ≠ Meaning:** You can understand what something means without being authorized to act on it
- **Execution ≠ Reflection:** Doing work and learning from work are different concerns

The OSI model has 7 layers. The Cognitive OSI has 7+meta. 7 seems to be a natural granularity for layered architectures.

### 4. What about hardware? If Philbrick isn't L0, where does physical computing fit?

**Answer:** L-1 (Substrate) - below the semantic layers.

The recommended hybrid adds:
```
L0: Provenance       - Everything has lineage
─────────────────────────────────────────────
L-1: Substrate       - Physical/computational reality (Philbrick, optional)
```

This preserves Philbrick's place without making hardware the *semantic* foundation. The Semantic OS runs on hardware but isn't defined by it.

### 5. How do we migrate? Can we align existing docs incrementally?

**Answer:** Yes, with a phased approach.

1. **Phase 1:** Accept this model as canonical (stakeholder review)
2. **Phase 2:** Update SIL_GLOSSARY.md with new layer definitions
3. **Phase 3:** Add frontmatter to existing docs noting which model they use
4. **Phase 4:** Incrementally update canonical docs (SEMANTIC_OS_ARCHITECTURE, SEMANTIC_OBSERVABILITY, etc.)
5. **Phase 5:** Archive historical models with "superseded by" notes

---

## Evaluation Criteria

Score this model against:

| Criterion | Question | Score (1-5) |
|-----------|----------|-------------|
| Problem fit | Does it help with LLM coexistence? | **5** |
| Conceptual clarity | Can you explain it in 2 minutes? | **4** |
| Component coherence | Do assignments make sense? | **4** |
| Extension path | Can new projects find their place? | **4** |
| Implementation guidance | Does it help developers? | **3** |

**Weighted Total: 4.15** (highest of all 5 models evaluated)

See [MODEL_EVALUATION.md](MODEL_EVALUATION) for full comparison and rationale.

---

## References

- `sessions/heating-snow-1214/README_2025-12-15_08-52.md` - Origin of proposal
- K&R/Unix Philosophy documentation
- GenesisGraph project documentation
- Trust Assertion Protocol specification

---

## Synthesis: Sessions That Informed This Model

This proposal synthesizes insights from 6+ sessions:

| Session | Key Contribution |
|---------|-----------------|
| `heating-snow-1214` | Original provenance-first proposal |
| `enchanted-centaur-1214` | Discovery of 4 competing models |
| `coral-shine-1212` | 3 critical missing subsystems (Intent, Uncertainty, Translation) |
| `brewing-sleet-1212` | Layer completeness percentages, "Layer 1 is the glue" insight |
| `noble-kraken-1125` | 696-line COGNITIVE_OS_MASTER_MAP with patron saints |
| `temporal-fractal-1214` | Documentation audit revealing inconsistencies |

### Key Synthesis Insights

1. **"We have more implemented than documented"** (brewing-sleet)
   - Infrastructure exists; Layer 1 primitives missing
   - Problem isn't building from scratch; it's formalizing and connecting

2. **"Layer 1 is the glue"** (brewing-sleet)
   - Intent Verification, Uncertainty Tracking, Cross-Domain Translation
   - These connect Agent coordination (L3) ↔ Pantheon IR (L1) ↔ Memory (L0)

3. **"4 competing models"** (enchanted-centaur)
   - Models evolved independently in different docs
   - No single authoritative version until now

4. **"Provenance when I hear SOS"** (Scott, heating-snow)
   - Founder intuition aligned with problem-centric analysis
   - Trust and lineage are core to the SIL mission

### The Case for Adoption

This model should become canonical because:

1. **Highest evaluation score** (4.15 vs 3.05 for Canonical)
2. **Resolves component assignment chaos** (tools span layers)
3. **Addresses core SIL mission** (LLM coexistence requires trust)
4. **Founder alignment** (Scott's instinct matches the analysis)
5. **Explains historical divergence** (product-centric vs problem-centric)

### Status

**Proposed for adoption.** Awaiting stakeholder review before updating canonical docs.
