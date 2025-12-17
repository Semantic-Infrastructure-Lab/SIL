# Architecture Guides

**Understanding SIL's system design and architectural principles**

This guide explains **how to think architecturally** about semantic systems. It provides the mental models, frameworks, and decision criteria for designing SIL-compliant components.

---

## Essential Reading

### [Unified Architecture Guide](UNIFIED_ARCHITECTURE_GUIDE) ⭐⭐⭐ The Rosetta Stone
**30-45 minutes** | The canonical framework for understanding all SIL projects

**This is THE most important document.** It:
- Defines canonical vocabulary (Intent, IR, Execution, Domain, Adapter, Primitive, Service, Kernel)
- Reveals the universal pattern that ALL projects follow (Intent → IR → Execution)
- Shows the two architectural styles (Adapter vs Microkernel)
- Maps every existing project to the unified framework
- Provides decision frameworks for adding new components

**Start here before diving into any individual project.**

**Who should read this:**
- ✅ New to SIL? Read this first
- ✅ Implementing a new component? This shows you where it fits
- ✅ Confused about terminology? This clarifies everything
- ✅ Want to understand how Pantheon, Morphogen, Prism relate? This connects them

---

### [Layer Models Comparison](models/) ⭐⭐ Architecture Decision
**15-30 minutes** | Comparing competing layer models for the Semantic OS

**Active evaluation** - Multiple layer models have evolved across documentation. This directory tracks and compares them:
- 5 documented models (Canonical, Original, Feedback, Observability, Provenance-First)
- Side-by-side comparison tables
- Evaluation framework for selecting canonical model
- Key question: Is Provenance the true L0?

---

### [Distributed Storage Architecture](DISTRIBUTED_STORAGE_ARCHITECTURE) ⭐⭐ Infrastructure Design
**20-30 minutes** | IPFS integration strategy for semantic memory, identity, and provenance

**Research & Planning** - Explores content-addressed distributed storage for SIL infrastructure.

**Key Topics:**
- Layer 0 (Semantic Memory) IPFS integration
- GenesisGraph provenance with content-addressed artifacts
- DID:IPFS method for agent identity
- Decentralized agent discovery via IPFS DHT
- Phased implementation strategy (12-month roadmap)

**Who should read this:**
- ✅ Working on Layer 0 (Semantic Memory) infrastructure
- ✅ Implementing GenesisGraph storage backends
- ✅ Designing distributed agent systems
- ✅ Interested in decentralized knowledge infrastructure

---

## How This Relates to Other Docs

```
UNIFIED_ARCHITECTURE_GUIDE.md (The Pattern)
    ↓ Applies
/foundations/SIL_PRINCIPLES.md (The Principles)
    ↓ Implements
/foundations/SIL_SEMANTIC_OS_ARCHITECTURE.md (The 6-Layer Stack)
    ↓ Realized in
/projects/PROJECT_INDEX.md (Concrete Implementations)
```

**Reading order:**
1. **Unified Architecture Guide** - Learn the universal pattern
2. **SIL Principles** - Learn the design philosophy
3. **Semantic OS Architecture** - Understand the 6-layer stack
4. Individual project docs - See concrete implementations

---

## Connection to Other Docs

### This is a guide, not a spec

**Architecture guide** (this directory):
- Mental models and frameworks
- Decision criteria
- "How to think about semantic systems"

**Canonical documents** (`/foundations/`):
- Formal specifications
- Definitive reference
- Principles and foundations

**Tools** (`/systems/`):
- Production implementations
- See principles in action

**Research papers** (`/research/`):
- Why things are designed this way
- Theoretical foundations

---

## For Different Audiences

### New Engineer/Contributor
**Read:** Unified Architecture Guide → SIL Principles
**Time:** 1 hour
**Outcome:** Understand the pattern and philosophy

### Architect/Steward
**Read:** Unified Architecture Guide + all Canonical docs
**Time:** 3-4 hours
**Outcome:** Can design SIL-compliant systems

### Researcher
**Read:** Unified Architecture Guide + Research Papers
**Time:** Variable
**Outcome:** Understand architectural foundations

---

## Key Concepts Explained

**From Unified Architecture Guide:**
- Intent → IR → Execution pattern
- Adapter vs Microkernel architectures
- When to use which architectural style
- How all 11 SIL projects map to the framework
- Universal vocabulary for all projects

**This is the Rosetta Stone** - it makes everything else make sense.

---

## Next Steps

After reading this guide:

**To understand the principles:**
→ Read [SIL Principles](/foundations/SIL_PRINCIPLES)

**To understand the 6-layer stack:**
→ Read [Semantic OS Architecture](/foundations/SIL_SEMANTIC_OS_ARCHITECTURE)

**To see concrete implementations:**
→ Read [Project Index](/projects/PROJECT_INDEX)

**To try working tools:**
→ Try [Reveal](/systems/REVEAL)

**To understand the research:**
→ Read [Research Papers](/research/)

---

## Navigation

- **Parent:** [Start Here](/foundations/START_HERE)
- **Related:** [Canonical Documents](/foundations/), [Tools](/systems/)
- **Projects:** [Project Index](/projects/PROJECT_INDEX)

---

**Last Updated:** 2025-12-15
**Documents:** 3 directories (Unified Architecture Guide, Distributed Storage Architecture, Layer Models)
**Reading Time:** ~75-105 minutes
**Status:** Active - Layer model evaluation in progress
