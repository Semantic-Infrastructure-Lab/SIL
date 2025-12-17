# Layer Model Evaluation Framework

**Version:** 1.0
**Date:** 2025-12-15
**Status:** Framework ready, scoring pending

---

## Purpose

This document provides a structured framework for evaluating the competing layer models and selecting a canonical approach.

---

## Evaluation Criteria

### 1. Problem Fit (Weight: 30%)

Does the model help solve SIL's core problems?

| Problem | Description |
|---------|-------------|
| LLM Coexistence | Human-AI collaboration with trust and attribution |
| Cross-Domain Work | Composing tools across unrelated domains |
| Intent Decomposition | Breaking high-level goals into trackable work |
| Meaning Discovery | Finding similar concepts across contexts |

**Scoring:**
- 5: Directly addresses problem in layer structure
- 4: Problem clearly maps to specific layers
- 3: Problem addressable but not explicit
- 2: Problem requires spanning multiple layers awkwardly
- 1: Model doesn't help with this problem

### 2. Conceptual Clarity (Weight: 25%)

Can you explain the model to a new team member in 2 minutes?

**Scoring:**
- 5: Clear organizing principle, obvious layer boundaries
- 4: Mostly clear, one or two confusing aspects
- 3: Requires significant explanation
- 2: Confusing layer assignments
- 1: Contradictory or incoherent

### 3. Component Coherence (Weight: 20%)

Do project/tool assignments make intuitive sense?

**Scoring:**
- 5: Every component has obvious home, no ambiguity
- 4: Most components clear, few edge cases
- 3: Significant debate about where things go
- 2: Components assigned to multiple layers
- 1: Chaotic assignment, constant disagreement

### 4. Extension Path (Weight: 15%)

When we add new projects, can they find their place?

**Scoring:**
- 5: New projects obviously fit, layers are extensible
- 4: Most new projects fit easily
- 3: Sometimes need to reconsider layer boundaries
- 2: New projects often don't fit well
- 1: Model breaks with new additions

### 5. Implementation Guidance (Weight: 10%)

Does the model help developers build things?

**Scoring:**
- 5: Clear interfaces between layers, implementation path obvious
- 4: Good guidance for most scenarios
- 3: Helpful but requires interpretation
- 2: Abstract, limited practical guidance
- 1: No implementation value

---

## Model Scores

### Model 1: Canonical (Glossary)

| Criterion | Score | Notes |
|-----------|-------|-------|
| Problem Fit | 3 | LLM coexistence not explicit; product-centric design |
| Conceptual Clarity | 4 | Clear 7-layer stack with patron saints; hardware grounding intuitive |
| Component Coherence | 2 | Component assignment chaos (Agent Ether at L3 vs L6); tools as layers problematic |
| Extension Path | 4 | New projects can find home; Philbrick at L0 provides grounding |
| Implementation Guidance | 3 | OSI_LAYER_MAPPING provides detailed roadmap; but Layer 1 primitives missing |
| **Weighted Total** | **3.05** | (0.30×3 + 0.25×4 + 0.20×2 + 0.15×4 + 0.10×3) |

### Model 2: Original Semantic OS

| Criterion | Score | Notes |
|-----------|-------|-------|
| Problem Fit | 3 | Memory-grounded; but trust/provenance implicit |
| Conceptual Clarity | 3 | 6 layers simpler; but Human Interfaces at top feels arbitrary |
| Component Coherence | 3 | Agent Ether at L3 makes sense as middleware; fewer assignment debates |
| Extension Path | 2 | No meta-layer; observability unclear where to add |
| Implementation Guidance | 2 | Historical; partially updated; less actionable than Canonical |
| **Weighted Total** | **2.65** | (0.30×3 + 0.25×3 + 0.20×3 + 0.15×2 + 0.10×2) |

### Model 3: Feedback Loops

| Criterion | Score | Notes |
|-----------|-------|-------|
| Problem Fit | 3 | Feedback explicit (L3); but no L0 leaves foundation unclear |
| Conceptual Clarity | 2 | No L0 confusing; Storage at L1 mixes concerns |
| Component Coherence | 3 | Tool infrastructure explicit (L2); Scout/Morphogen at L6 reasonable |
| Extension Path | 2 | Missing L0 makes grounding new projects difficult |
| Implementation Guidance | 2 | Historical; less detailed than Canonical/Pantheon docs |
| **Weighted Total** | **2.45** | (0.30×3 + 0.25×2 + 0.20×3 + 0.15×2 + 0.10×2) |

### Model 4: Observability

| Criterion | Score | Notes |
|-----------|-------|-------|
| Problem Fit | 3 | Observability as L4.5 innovative; but trust/provenance implicit |
| Conceptual Clarity | 2 | L4.5 breaks clean layer model; 8 effective layers complex |
| Component Coherence | 2 | Tools-as-layers (TIA at L4, Beth at L3) causes confusion |
| Extension Path | 2 | L4.5 awkward for new components; where do they fit? |
| Implementation Guidance | 2 | Tool-centric but less implementation detail |
| **Weighted Total** | **2.30** | (0.30×3 + 0.25×2 + 0.20×2 + 0.15×2 + 0.10×2) |

### Model 5: Provenance-First

| Criterion | Score | Notes |
|-----------|-------|-------|
| Problem Fit | 5 | Directly addresses LLM coexistence, trust, cross-domain; problem-centric |
| Conceptual Clarity | 4 | Clean 7-layer with clear Unix parallel; "everything has provenance" memorable |
| Component Coherence | 4 | Tools span layers (like Unix utilities); explains assignment chaos |
| Extension Path | 4 | New projects ask: what layer? what layers span? how relates to provenance? |
| Implementation Guidance | 3 | Needs more detail; but GenesisGraph provides foundation |
| **Weighted Total** | **4.15** | (0.30×5 + 0.25×4 + 0.20×4 + 0.15×4 + 0.10×3) |

---

## Key Decision Questions

Before scoring, consider these fundamental questions:

### Q1: What is the foundation?

**Options:**
- A) Physical hardware (Philbrick, compute)
- B) Semantic memory (storage, indexing)
- C) Provenance (lineage, trust)
- D) Something else

**Current answer:** **C) Provenance**

**Rationale:** The core problem is LLM coexistence - humans and AI working together safely. Without provenance (who made what, when, from what), you cannot establish trust in LLM output. Hardware is necessary but not the architectural foundation for a *semantic* OS. Memory is implementation, not abstraction. Provenance is the primitive that makes everything else trustworthy. Scott's gut reaction ("provenance when I hear SOS") aligns with this.

### Q2: Are tools layers or utilities?

**Options:**
- A) Tools are layers (TIA at L4, Beth at L3)
- B) Tools span layers (like Unix utilities)
- C) Tools are meta (outside the layer stack)

**Current answer:** **B) Tools span layers**

**Rationale:** The component assignment chaos (Agent Ether at L3 vs L6, Beth at L1 vs L3, TIA at L4 vs spanning) is explained by tools naturally spanning layers based on function. Unix utilities don't live at one layer of the network stack - `curl` touches application, transport, and network layers. Similarly:
- Beth: L1 (Meaning) + L0 (Provenance of knowledge)
- Reveal: L6 (Reflection) + L4 (Composition structure)
- TIA: L3 (Intent) + L5 (Execution) + L6 (Reflection)

This resolves the "where does X go" debates by acknowledging tools are cross-cutting.

### Q3: Where does trust live?

**Options:**
- A) Implicit in Intent layer
- B) Explicit Trust layer
- C) Cross-cutting concern
- D) Part of Provenance

**Current answer:** **B) Explicit Trust layer (L2 in Provenance-First)**

**Rationale:** Trust is too important for LLM coexistence to be implicit. The TAP (Trust Assertion Protocol) and Hierarchical Agency Framework already exist as substantial specs. Making Trust an explicit layer (between Meaning and Intent) establishes:
- You can't express intent without authorization (Trust → Intent dependency)
- Trust requires understanding meaning (Meaning → Trust dependency)
- Trust is grounded in provenance (Provenance → Meaning → Trust chain)

Cross-cutting makes trust feel like an afterthought. An explicit layer makes it architectural.

### Q4: Is Observability a layer?

**Options:**
- A) Meta-layer (orthogonal)
- B) Explicit layer (L4.5 or L6)
- C) Part of Reflection/Feedback

**Current answer:** **C) Part of Reflection (L6 in Provenance-First)**

**Rationale:** Observability is *how we learn from execution*. The coral-shine sessions identified "uncertainty tracking" as a critical missing subsystem - this is observability in action. Placing it at L6 (Reflection) makes it:
- The top of the stack (learning from everything below)
- Part of a feedback loop (Reflection → informs future Intent)
- Not a weird L4.5 that breaks clean layering

The Canonical model's "meta-layer" treatment makes observability feel bolted-on. Making it L6 integrates it into the architecture.

---

## Comparison Matrix

| Aspect | Canonical | Original | Feedback | Observability | Provenance |
|--------|-----------|----------|----------|---------------|------------|
| L0 definition | Substrate | Memory | (none) | (none) | Provenance |
| Trust location | L5 | implicit | L3 | implicit | L2 |
| Tool treatment | layers | layers | layers | layers | utilities |
| Observability | meta | (none) | L3 | L4.5 | L6 |
| Layer count | 7+meta | 6 | 6 | 8 | 7 |

---

## Hybrid Possibilities

The models aren't mutually exclusive. Possible hybrids:

### Hybrid A: Canonical + Trust Layer

Keep Glossary model but add explicit Trust between Structures and Composition.

### Hybrid B: Provenance-First + Hardware Substrate

Keep Provenance at L0 but add L-1 for hardware/physics.

### Hybrid C: Tools-as-Utilities for Any Model

Any model could adopt "tools span layers" rather than "tools are layers."

---

## Decision Process

1. [x] Answer key decision questions *(completed 2025-12-15)*
2. [x] Score each model on criteria *(completed 2025-12-15)*
3. [x] Evaluate hybrid possibilities *(see below)*
4. [x] Draft recommendation *(see below)*
5. [ ] Stakeholder review
6. [ ] Final decision
7. [ ] Update canonical docs

---

## Recommendation

**Recommended Model: Provenance-First (with Hybrid B enhancement)**

### Score Summary

| Model | Weighted Score | Rank |
|-------|---------------|------|
| Provenance-First | **4.15** | 1st |
| Canonical (Glossary) | 3.05 | 2nd |
| Original Semantic OS | 2.65 | 3rd |
| Feedback Loops | 2.45 | 4th |
| Observability | 2.30 | 5th |

### Why Provenance-First Wins

1. **Problem Fit (5/5):** Directly addresses LLM coexistence, the core SIL mission
2. **Explains Assignment Chaos:** Tools-as-utilities resolves years of "where does X go" debates
3. **Scott's Intuition:** "Provenance when I hear SOS" - founder alignment matters
4. **Unix Parallel:** "Everything has meaning and provenance" is memorable and explanatory
5. **Trust as Explicit Layer:** Makes authorization architectural, not afterthought

### Recommended Hybrid: Provenance-First + Hardware Substrate (Hybrid B)

Add L-1 for physical/computational substrate:

```
L6: Reflection       - Learning from execution (observability)
L5: Execution        - Doing work under constraints (agents)
L4: Composition      - Cross-domain integration (Pantheon IR)
L3: Intent           - What we're accomplishing (contracts)
L2: Trust            - Who can do what (TAP, Authorization)
L1: Meaning          - Embeddings, types, similarity (Beth, Pantheon)
L0: Provenance       - Everything has lineage (GenesisGraph)
─────────────────────────────────────────────────────────────
L-1: Substrate       - Physical/computational reality (Philbrick, optional)
```

This preserves Philbrick's place without making hardware the semantic foundation.

### Critical Missing Subsystems (from coral-shine/brewing-sleet)

These must be built regardless of model choice:

| Subsystem | Primary Layer | Description |
|-----------|--------------|-------------|
| Intent Verification | L3 | Cryptographic verification of intent preservation |
| Uncertainty Tracking | L6 | Geometric uncertainty propagation monitoring |
| Cross-Domain Translation | L4 | Invariant-preserving semantic operators |

### Next Steps

1. **Stakeholder review** - Present this analysis for feedback
2. **Prototype the 3 subsystems** - They validate the layer model
3. **Update SIL_GLOSSARY.md** - After decision confirmed
4. **Create migration guide** - How to update existing docs

---

## Stakeholder Input

| Stakeholder | Preference | Rationale |
|-------------|------------|-----------|
| Scott | Provenance-first? | "Provenance when I hear SOS" |
| TIA sessions | Various | 4 models evolved independently |
| Pantheon docs | Cognitive OSI | Hardware-grounded |

---

## References

- [LAYER_MODELS_COMPARISON.md](LAYER_MODELS_COMPARISON) - Side-by-side view
- [PROVENANCE_FIRST.md](PROVENANCE_FIRST) - Provenance proposal detail
- Source documents in canonical/ and pantheon/docs/

---

## Addendum: Synthesis Session (pulsing-horizon-1215)

**Added:** 2025-12-15
**Session:** pulsing-horizon-1215

### Stakeholder Review Outcome

During review, a deeper question emerged: **Does any layer model serve the mission, or is there a different organizing principle?**

After reading 8 foundational documents (THE_FORK.md, SCOPE_OF_HOPE.md, SIL_VISION_COMPLETE.md, SIL_MORPHOGEN_PROJECT.md, etc.), a new frame was proposed:

### Invariants Over Layers

**Key insight:** Layer models organize structure. Invariants enforce mission.

The five invariants that must hold everywhere:
1. **Everything has lineage** (GenesisGraph)
2. **Reasoning is inspectable** (Glass Box)
3. **Computation is grounded** (Morphogen)
4. **Contracts are explicit** (Agent Ether)
5. **Efficiency is sustainable** (Reveal + Beth)

### Proposed Synthesis

- **Use Provenance-First layer model** for organizational structure
- **Use Invariants frame** for mission alignment and design decisions
- Both are valid. Question is which is primary for which purpose.

### New Documents

- [INVARIANTS_OVER_LAYERS.md](INVARIANTS_OVER_LAYERS) - Full proposal
- [SYNTHESIS_MAP.md](SYNTHESIS_MAP) - Meta-navigation for future sessions

### The Chief Scientist Test

Before approving architectural decisions:
1. Does this maintain traceable lineage?
2. Is reasoning inspectable?
3. Is computation connected to reality?
4. Are assumptions explicit?
5. Is this sustainable at scale?
6. Does this keep humans as conductors?

See [INVARIANTS_OVER_LAYERS.md](INVARIANTS_OVER_LAYERS) for full rationale.
