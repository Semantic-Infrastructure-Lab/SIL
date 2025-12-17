# Layer Models Comparison

**Version:** 1.0
**Date:** 2025-12-15
**Status:** Working document

---

## Overview

This document compares the different layer models that have emerged across SIL documentation. The goal is to understand the differences, evaluate trade-offs, and converge on a canonical model.

---

## Model Summary Table

| Layer | Canonical (Glossary) | Original Semantic OS | Feedback Loops | Observability | Provenance-First |
|-------|---------------------|---------------------|----------------|---------------|------------------|
| **L7** | - | - | - | Applications | - |
| **L6** | Intelligence | - | Applications | Agent Orchestration | Reflection |
| **L5** | Intent | Human Interfaces | Agent Orchestration | Pantheon | Execution |
| **L4.5** | - | - | - | **Observability** | - |
| **L4** | Dynamics | Deterministic Engines | Semantic Primitives | TIA | Composition |
| **L3** | Composition | Agent Ether | Feedback & Reflection | Beth | Intent |
| **L2** | Structures | Domain Modules | Tool Infrastructure | Domain modules | Trust |
| **L1** | Primitives | Pantheon IR | Storage & Indexing | Semantic primitives | Meaning |
| **L0** | Substrate | Semantic Memory | - | - | **Provenance** |
| **L-1** | Arena (implicit) | - | - | - | - |
| **Meta** | Observability | - | - | - | - |

---

## Model 1: Canonical (Glossary v2.2)

**Source:** `SIL_GLOSSARY.md`
**Status:** Current canonical reference

```
L6: Intelligence    - Agents, Planning, Adaptation (Agent Ether, BrowserBridge)
L5: Intent          - Goals, Constraints, Roles (Pantheon validation)
L4: Dynamics        - Time, Behavior, Execution (Morphogen scheduler)
L3: Composition     - Graphs, Routing, Topology (Pantheon IR, SUP)
L2: Structures      - Semantic Units, Types (TiaCAD, GenesisGraph)
L1: Primitives      - Irreducible Operations (Morphogen domains, RiffStack)
L0: Substrate       - Physical/Computational Reality (Philbrick hardware)
─────────────────────────────────────────────────────────────────────────
Meta: Observability - Cross-cutting (Reveal)
```

**Characteristics:**
- Hardware-grounded (Philbrick at L0)
- Observability as meta-layer
- Product-centric assignment
- 7 layers + meta

**Patron Saints (from OSI_LAYER_MAPPING):**
- L6: Marvin Minsky (Agents)
- L5: Douglas Engelbart (Augmentation)
- L4: Alan Turing (Computation)
- L3: Claude Shannon (Information)
- L2: George Philbrick (Modularity)
- L1: Harold Black (Feedback)
- L0: Richard Feynman (Physics)

---

## Model 2: Original Semantic OS (6-Layer)

**Source:** `SIL_SEMANTIC_OS_ARCHITECTURE.md` (pre-alignment)
**Status:** Historical, partially updated

```
L5: Human Interfaces
L4: Deterministic Engines (Morphogen)
L3: Agent Ether
L2: Domain Modules
L1: Pantheon IR
L0: Semantic Memory
```

**Characteristics:**
- Memory-grounded (Semantic Memory at L0)
- Human interfaces at top
- Agent Ether as middleware (L3)
- 6 layers, no meta

---

## Model 3: Feedback Loops (6-Layer)

**Source:** `SEMANTIC_FEEDBACK_LOOPS.md`
**Status:** Historical

```
L6: Applications (Scout, Morphogen)
L5: Agent Orchestration (agent-ether)
L4: Semantic Primitives (USIR, knowledge graphs)
L3: Feedback & Reflection
L2: Tool Infrastructure (reveal, tia)
L1: Storage & Indexing (Beth, Gemma)
```

**Characteristics:**
- No L0 defined
- Feedback as explicit layer (L3)
- Tool infrastructure prominent
- Application-focused top layers

---

## Model 4: Observability (7-Layer + L4.5)

**Source:** `SEMANTIC_OBSERVABILITY.md` (pre-alignment)
**Status:** Historical, had unique L4.5

```
L7: Applications (Scout, Reveal, Agent-Ether)
L6: Agent Orchestration
L5: Pantheon
L4.5: SEMANTIC OBSERVABILITY  ← Unique!
L4: TIA
L3: Beth
L2: Domain modules
L1: Semantic primitives
```

**Characteristics:**
- Observability as full layer (L4.5)
- Tools as layers (TIA at L4, Beth at L3)
- 8 effective layers
- Most tool-centric model

---

## Model 5: Provenance-First (Proposed)

**Source:** `sessions/heating-snow-1214/README_2025-12-15_08-52.md`
**Status:** Proposed alternative

```
L6: Reflection      - Learning from execution (observability)
L5: Execution       - Doing work under constraints (agents)
L4: Composition     - Cross-domain integration (Pantheon IR)
L3: Intent          - What we're accomplishing (contracts)
L2: Trust           - Who can do what (TAP, Authorization)
L1: Meaning         - Embeddings, types, similarity (Beth, Pantheon)
L0: Provenance      - Everything has lineage (GenesisGraph)
```

**Characteristics:**
- Problem-centric (solves LLM coexistence)
- Provenance as foundation
- Trust as explicit layer
- Philbrick becomes optional backend
- Tools span layers (like Unix utilities)

**Design Rationale:**
If core problems are:
1. Decomposing intent into trackable work
2. Trust relationships for LLM/AGI coexistence
3. Cross-domain tooling
4. Meaning manifolds for analogies

Then layers should reflect those problems, not products.

**Unix Philosophy Parallel:**
- Unix insight: "Everything is a file"
- Semantic OS insight: "Everything has meaning and provenance"

---

## Component Assignment Comparison

| Component | Canonical | Original | Feedback | Observability | Provenance-First |
|-----------|-----------|----------|----------|---------------|------------------|
| **Agent Ether** | L6 | L3 | L5 | L6/L7 | L5 (Execution) |
| **Morphogen** | L1+L4 | L4 | L6 | - | spans L4-L5 |
| **Pantheon IR** | L3 | L1 | L4 | L5 | L4 (Composition) |
| **GenesisGraph** | L2 | - | - | - | L0 (Foundation) |
| **Beth** | L2 | - | L1 | L3 | L1 (Meaning) |
| **Reveal** | Meta | - | L2 | L7 | spans layers |
| **TIA** | - | - | L2 | L4 | spans L3-L6 |
| **SUP** | L3 | - | - | - | L4 (Composition) |
| **Philbrick** | L0 | - | - | - | backend (optional) |
| **Human Interfaces** | L5 | L5 | - | - | L6 (Reflection) |

---

## Key Architectural Questions

### 1. What is L0?

| Model | L0 Definition | Implication |
|-------|---------------|-------------|
| Canonical | Substrate (hardware) | Architecture is hardware-up |
| Original | Semantic Memory | Architecture is memory-up |
| Provenance-First | Provenance (lineage) | Architecture is trust-up |

**Question:** Is the foundation hardware, memory, or trust?

### 2. Where do tools fit?

| Approach | Examples | Implication |
|----------|----------|-------------|
| Tools as layers | TIA at L4, Beth at L3 | Tools are architectural components |
| Tools span layers | TIA spans L3-L6 | Tools are utilities, not layers |
| Tools as meta | Reveal as meta-layer | Tools observe, don't participate |

**Question:** Should TIA/Beth/Reveal be layers or cross-cutting utilities?

### 3. Is Observability a layer or meta?

| Model | Observability Location | Implication |
|-------|----------------------|-------------|
| Canonical | Meta-layer | Orthogonal concern |
| Observability | L4.5 | First-class layer |
| Provenance-First | L6 (Reflection) | Part of learning loop |

### 4. Where does Trust/Authorization fit?

| Model | Trust Location | Implication |
|-------|---------------|-------------|
| Canonical | L5 (Intent) | Trust is part of goal-setting |
| Provenance-First | L2 | Trust is structural foundation |

---

## Cross-Cutting Concerns (All Models)

Regardless of layer assignment, these span the stack:

1. **Provenance** - Who made what, when, from what
2. **Trust** - Who can do what to whom
3. **Observability** - What's happening, how well
4. **Feedback** - Learning from execution

The Provenance-First model makes two of these (Provenance, Trust) explicit layers rather than cross-cutting.

---

## Critical Missing Subsystems

The `coral-shine-1212` and `brewing-sleet-1212` sessions identified three subsystems that don't yet exist but are critical for any model:

### 1. Intent Verification Subsystem

**Problem:** No mechanical way to verify that an action preserves the original intent.

**Solution:** Intent as cryptographic primitive - asymmetric verification where it's easy to check but hard to fake.

| Model | Where It Lives |
|-------|---------------|
| Canonical | L5 (Intent) |
| Original | L5 (Human Interfaces) |
| Feedback | L3 (Feedback & Reflection) |
| Observability | L6 (Agent Orchestration) |
| **Provenance-First** | **L3 (Intent)** - explicit intent layer |

**Components needed:**
- `intent.py` - Intent object schema
- `signature.py` - Intent signature verification
- `contract.py` - Contract enforcement
- `amendment.py` - Intent versioning

### 2. Uncertainty Tracking Subsystem

**Problem:** Uncertainty compounds geometrically through operations; system can't detect or prevent runaway uncertainty.

**Solution:** Uncertainty as first-class field - track assumptions, coupling, and propagation gradients.

| Model | Where It Lives |
|-------|---------------|
| Canonical | Meta (Observability) |
| Original | (implicit) |
| Feedback | L3 (Feedback & Reflection) |
| Observability | L4.5 |
| **Provenance-First** | **L6 (Reflection)** - learning from execution |

**Components needed:**
- `uncertainty.py` - UncertaintyProfile schema
- `propagation.py` - Gradient tracking
- `brakes.py` - Abort semantics, checkpoints

### 3. Cross-Domain Translation Subsystem

**Problem:** Domains translate meaning ad-hoc; invariants not preserved; loss untracked.

**Solution:** Semantic operators with contracts - like FFT preserving information while changing representation.

| Model | Where It Lives |
|-------|---------------|
| Canonical | L3 (Composition) |
| Original | L1 (Pantheon IR) |
| Feedback | L4 (Semantic Primitives) |
| Observability | L5 (Pantheon) |
| **Provenance-First** | **L4 (Composition)** - cross-domain integration |

**Components needed:**
- `operator.py` - SemanticOperator base class
- `invariant.py` - Invariant schema
- `registry.py` - Operator catalog
- `verification.py` - Round-trip testing

### Subsystem Layer Mapping Summary

| Subsystem | Provenance-First Layer | Math Analogy |
|-----------|----------------------|--------------|
| Intent Verification | L3 (Intent) | Cryptography (asymmetric verification) |
| Uncertainty Tracking | L6 (Reflection) | Thermodynamics (entropy flow) |
| Cross-Domain Translation | L4 (Composition) | FFT (invariant preservation) |

**Key Insight:** These aren't new ideas - we're applying proven mathematical patterns to semantic computing.

---

## Evaluation Criteria

To decide on a canonical model, evaluate against:

1. **Problem fit** - Does it help with LLM coexistence, cross-domain work?
2. **Conceptual clarity** - Can you explain it in 2 minutes?
3. **Component coherence** - Do assignments make sense?
4. **Extension path** - Can new projects find their place?
5. **Implementation guidance** - Does it help developers?

See [MODEL_EVALUATION.md](MODEL_EVALUATION) for detailed analysis.

---

## Session References

- `enchanted-centaur-1214` - Discovery of 4 competing models
- `heating-snow-1214` - Provenance-first proposal
- `noble-kraken-1125/COGNITIVE_OS_MASTER_MAP.md` - Comprehensive mapping attempt
- `temporal-fractal-1214` - Documentation audit

---

## Next Steps

1. [ ] Complete MODEL_EVALUATION.md with criteria scoring
2. [ ] Draft PROVENANCE_FIRST.md with full rationale
3. [ ] Get stakeholder input on key questions
4. [ ] Propose canonical model
5. [ ] Update source documents to align
