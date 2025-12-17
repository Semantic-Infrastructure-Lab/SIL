# SIL Architecture Models

**Purpose:** Track architectural frames for the Semantic OS and evaluate which best serves the SIL/SIF mission.

**Status:** Layer evaluation complete. New "Invariants Over Layers" frame proposed. Under stakeholder review (2025-12-15).

---

## The Evolution

1. **Discovery** - 4 competing layer models found (enchanted-centaur-1214)
2. **Proposal** - Provenance-first model proposed (heating-snow-1214)
3. **Evaluation** - All models scored, Provenance-First won (oracular-throne-1215)
4. **Reframe** - "Invariants Over Layers" proposed (pulsing-horizon-1215)
5. **Validation** - Frame validated, Agent Ether gap identified (turbulent-current-1215)

**Key insight:** Layer models organize structure. Invariants enforce mission. Both are needed.

---

## Documents in This Directory

### Evaluation Documents

| Document | Purpose |
|----------|---------|
| [LAYER_MODELS_COMPARISON.md](LAYER_MODELS_COMPARISON) | Side-by-side comparison of 5 layer models |
| [MODEL_EVALUATION.md](MODEL_EVALUATION) | Scoring criteria, analysis, and recommendation |
| [PROVENANCE_FIRST.md](PROVENANCE_FIRST) | Deep dive on the winning layer model |

### Synthesis Documents

| Document | Purpose |
|----------|---------|
| [INVARIANTS_OVER_LAYERS.md](INVARIANTS_OVER_LAYERS) | **NEW** Mission-centric reframe: guarantees over structure |
| [SYNTHESIS_MAP.md](SYNTHESIS_MAP) | **NEW** Meta-doc: which sources contribute which insights |

---

## Quick Reference

### The Two Frames

**Frame 1: Layer Models** (organizational structure)
1. Canonical (Glossary) - 7 layers, L0=Substrate → Score: 3.05
2. Original Semantic OS - 6 layers, L0=Semantic Memory → Score: 2.65
3. Feedback Loops - 6 layers, L1=Storage → Score: 2.45
4. Observability - 7 layers with L4.5 → Score: 2.30
5. **Provenance-First** - 7 layers, L0=Provenance → Score: **4.15** (Winner)

**Frame 2: Invariants** (mission enforcement)
- Everything has lineage (GenesisGraph)
- Reasoning is inspectable (Glass Box)
- Computation is grounded (Morphogen)
- Contracts are explicit (Agent Ether)
- Efficiency is sustainable (Reveal + Beth)

### The Synthesis

> **Use layers for organization. Use invariants for mission alignment.**
>
> Layer models answer "where does this go?"
> Invariants answer "does this prevent the worst day?"

---

## Source Documents

### Primary Sources
- `/home/scottsen/src/projects/SIL/docs/canonical/SIL_GLOSSARY.md` - Current canonical
- `/home/scottsen/src/projects/SIL/docs/canonical/SIL_SEMANTIC_OS_ARCHITECTURE.md` - Original 6-layer
- `/home/scottsen/src/projects/SIL/docs/canonical/SEMANTIC_FEEDBACK_LOOPS.md` - Feedback model
- `/home/scottsen/src/projects/SIL/docs/canonical/SEMANTIC_OBSERVABILITY.md` - Observability model

### Pantheon Sources
- `/home/scottsen/src/projects/pantheon/docs/OSI_LAYER_MAPPING.md` - Pantheon mapping
- `/home/scottsen/src/projects/pantheon/docs/UNIFIED_ARCHITECTURE.md` - Unified vision

### Session Context
- `sessions/enchanted-centaur-1214/` - Discovery of 4 competing models
- `sessions/heating-snow-1214/` - Provenance-first proposal
- `sessions/noble-kraken-1125/COGNITIVE_OS_MASTER_MAP.md` - Master map attempt

---

## Decision Status

**Current State:** Invariants frame validated. Ready for final acceptance.

### Layer Model Decision
✅ **Provenance-First** selected (Score: 4.15)

### Frame Decision
✅ **Invariants Over Layers** validated (90% confidence on frame)

### Implementation Reality
| Invariant | Enforcement | Status |
|-----------|-------------|--------|
| Everything has lineage | GenesisGraph | **Production** |
| Reasoning is inspectable | Reveal | **Production** |
| Computation is grounded | Morphogen | **Prototype** |
| Contracts are explicit | Agent Ether | **Gap** (spec only) |
| Efficiency is sustainable | Reveal + Beth | **Production** |

### Proposed Synthesis
- Use **Provenance-First layer model** for organizational structure
- Use **Invariants frame** for mission alignment and design decisions
- The Chief Scientist Test: 6 questions before approving architecture
- **Acknowledge Agent Ether gap** - prioritize implementation

**Next Steps:**
1. ~~Complete side-by-side comparison~~ ✅
2. ~~Evaluate against real problems~~ ✅
3. ~~Propose canonical model~~ ✅
4. ~~Stakeholder review of layers~~ ✅
5. ~~Propose invariants frame~~ ✅ (pulsing-horizon-1215)
6. ~~Validate against implementations~~ ✅ (turbulent-current-1215)
7. **Accept synthesis** ← YOU ARE HERE
8. Prioritize Agent Ether Phase 1
9. Create design review checklist based on invariants

---

## For Future Sessions

**Start here:** [SYNTHESIS_MAP.md](SYNTHESIS_MAP) - Reading paths by goal

**The Chief Scientist Test** (from INVARIANTS_OVER_LAYERS.md):
1. Does this maintain traceable lineage?
2. Is reasoning inspectable?
3. Is computation connected to reality?
4. Are assumptions explicit?
5. Is this sustainable at scale?
6. Does this keep humans as conductors?
