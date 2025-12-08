# The Semantic Operating System

**A 6-layer architecture for knowledge work and intelligent systems**

The Semantic OS is SIL's core technical vision: a modular, layered infrastructure for knowledge work—analogous to how Linux provides an operating system for computation.

Just as an operating system manages processes, memory, files, and devices, the **Semantic OS manages knowledge, meaning, agents, and deterministic computation**.

---

## Quick Overview

```
┌─────────────────────────────────────────────────────────┐
│  Layer 5: Human Interfaces / SIM                        │
│  Progressive disclosure, exploration, visualization      │
│  Projects: reveal, browserbridge                        │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│  Layer 4: Deterministic Engines                         │
│  MLIR compilation, reproducible execution               │
│  Projects: morphogen, riffstack                         │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│  Layer 3: Multi-Agent Orchestration                     │
│  Agent protocols, coordination                          │
│  Projects: agent-ether                                  │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│  Layer 2: Domain Modules                                │
│  Audio, CAD, UI, musical synthesis                      │
│  Projects: morphogen, tiacad, riffstack, sup            │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│  Layer 1: Universal Semantic IR                         │
│  Universal semantic representation                      │
│  Projects: pantheon                                     │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│  Layer 0: Semantic Memory                               │
│  Persistent provenance-complete semantic graph          │
│  Projects: semantic-memory                              │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Cross-Cutting Infrastructure                           │
│  Provenance: genesisgraph | Queries: prism              │
└─────────────────────────────────────────────────────────┘
```

---

## Explore Each Layer

### Foundation
- **[Layer 0: Semantic Memory](layer-0-semantic-memory.md)** - Persistent knowledge graphs with provenance

### Core Infrastructure
- **[Layer 1: Universal Semantic IR](layer-1-universal-ir.md)** - USIR for cross-domain composition
- **[Layer 2: Domain Modules](layer-2-domain-modules.md)** - Domain-specific semantics (audio, CAD, UI)

### Execution & Coordination
- **[Layer 3: Multi-Agent Orchestration](layer-3-agent-orchestration.md)** - Agent protocols and coordination
- **[Layer 4: Deterministic Engines](layer-4-deterministic-engines.md)** - MLIR compilation and execution

### Human Interfaces
- **[Layer 5: Human Interfaces / SIM](layer-5-human-interfaces.md)** - Progressive disclosure and exploration

### Cross-Cutting
- **[Cross-Cutting Infrastructure](cross-cutting-infrastructure.md)** - Provenance (GenesisGraph) and queries (Prism)

---

## The Universal Pattern

Every SIL system follows the **Intent → IR → Execution** pattern:

```
User Intent (Domain Language)
    ↓ Compile to
Universal Semantic IR
    ↓ Lower to
Execution (Hardware/Framework)
```

**Examples:**
- **Morphogen:** Audio DSL → Pantheon IR → MLIR → WebAudio/GPU
- **TiaCAD:** YAML geometry → Pantheon IR → CadQuery → STEP files
- **SUP:** UI intent → Pantheon IR → React/Vue components

See [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md) for complete framework.

---

## Why Layers?

**Separation of concerns:**
- Each layer has clear responsibilities
- Clean interfaces between layers
- Easier to test, verify, and evolve

**Composability:**
- Domain modules (Layer 2) plug into USIR (Layer 1)
- Engines (Layer 4) can optimize across domains
- Interfaces (Layer 5) work with any domain

**Provenance:**
- Transformations between layers are tracked
- Full audit trail from intent to execution
- Reproducible builds

---

## Connection to Other Docs

**For formal specification:**
→ [Technical Charter](../canonical/SIL_TECHNICAL_CHARTER.md)

**For mental models:**
→ [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md)

**For design principles:**
→ [Design Principles](../architecture/DESIGN_PRINCIPLES.md)

**For research foundations:**
→ [Research Papers](../research/)

**For all projects:**
→ [Project Index](../../projects/PROJECT_INDEX.md)

---

## Current Status

| Layer | Status | Projects |
|-------|--------|----------|
| Layer 0 | 📋 Planned | semantic-memory |
| Layer 1 | 🔬 Research | pantheon |
| Layer 2 | ✅ 4 Production | morphogen, tiacad, riffstack, sup |
| Layer 3 | 📋 Specification | agent-ether |
| Layer 4 | ✅ 2 Production | morphogen, riffstack |
| Layer 5 | ✅ 1 Production, 1 Alpha | reveal, browserbridge |
| Cross-Cutting | ✅ 1 Production, 1 Spec | genesisgraph, prism |

**5 production systems** spanning the stack validate the architecture.

---

## For Different Audiences

### New to SIL?
**Start:** Layer overview pages → [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md)

### Building on SIL?
**Start:** [Layer 2 (Domain Modules)](layer-2-domain-modules.md) → Project docs

### Researching?
**Start:** [Research Papers](../research/) → Layer-specific design docs

### Want to understand the vision?
**Start:** [Manifesto](../canonical/SIL_MANIFESTO.md) → This overview

---

## Navigation

- **Parent:** [Documentation Hub](../READING_GUIDE.md)
- **Related:** [Architecture](../architecture/), [Projects](../../projects/)
- **Deep Dive:** Individual layer pages (linked above)

---

**Last Updated:** 2025-11-30
**Layers:** 6 + cross-cutting infrastructure
**Projects:** 11 total (5 production)
**Status:** Layer 0, 1, 3 under active development
