# SIL Unified Architecture Guide

**The Canonical Framework for Understanding All SIL Projects**

**Version:** 1.0
**Created:** 2025-11-27
**Status:** Definitive Reference
**Purpose:** Unified vocabulary and mental model for the entire SIL ecosystem

---

## 🎯 What This Document Does

This is the **Rosetta Stone** for SIL architecture. It:

1. **Defines canonical vocabulary** (one term for each concept)
2. **Reveals the universal pattern** (that ALL projects follow)
3. **Shows two architectural styles** (and when to use each)
4. **Maps every existing project** to the unified framework
5. **Provides decision frameworks** for adding new components

**Read this first** before diving into individual project docs.

> 💡 **New to SIL terminology?** Keep the [Glossary](/foundations/SIL_GLOSSARY) open in another tab.

---

## 🧭 Who Should Read This & When

### **You should read this document if:**
- ✅ You're new to SIL and want to understand the architecture
- ✅ You're implementing a new component and need to know where it fits
- ✅ You're confused about SIL terminology (Intent vs IR vs Execution)
- ✅ You need to decide: Adapter or Microkernel architecture?
- ✅ You want to understand how Pantheon, Morphogen, Prism, etc. relate

### **Read this BEFORE:**
- Technical Charter (provides formal spec - this provides mental model)
- Individual project docs (Pantheon, Morphogen, etc.)
- Implementation guides

### **Read this AFTER:**
- `/foundations/SIL_MANIFESTO.md` (optional, 15 min - gives you context on "why")

### **Time Required:** 30-45 minutes

---

## 📖 Related Documents Navigation

### **"I need something simpler first"**
→ Start with **`/foundations/SIL_MANIFESTO.md`** (15 min) for the high-level vision

### **"I need the formal specification"**
→ After reading this, go to **`/foundations/SIL_TECHNICAL_CHARTER.md`** (2 hours)

### **"I need to look up terminology"**
→ Keep **`/foundations/SIL_GLOSSARY.md`** open while reading this

### **"I need design principles"**
→ Read **`./DESIGN_PRINCIPLES.md`** (15 min) for evaluation criteria

### **"I need to see concrete implementation"**
→ See Pantheon's documentation for concrete 7-layer Cognitive OSI Stack implementation

### **"I need the entry point"**
→ See **[Start Here](/start)** for the front door to SIL

### **"I'm looking for examples of how to use this"**
→ See Part 8 (Quick Reference Examples) and Part 10 (The Meta-Pattern) below

---

## 🎯 What You'll Learn

By the end of this document, you will:

1. ✅ Understand the **Intent → IR → Execution** pattern (and see it everywhere)
2. ✅ Know canonical vocabulary (Intent, IR, Execution, Domain, Adapter, Service, Kernel)
3. ✅ Recognize the **two architectural styles** (Adapter vs Microkernel)
4. ✅ Be able to **map any project** to the framework
5. ✅ Know how to **decide where new components belong**

---

## 📚 Part 1: Canonical Vocabulary

### The Universal Terms (Use These)

| Term | Definition | Replaces/Clarifies |
|------|------------|-------------------|
| **Intent** | What the user wants to express (high-level, semantic) | "Declarative layer", "semantic layer", "input" |
| **IR** (Intermediate Representation) | The canonical semantic representation | "USIR", "Semantic IR", "graph representation" |
| **Execution** | How it runs on hardware | "Backend", "runtime", "lowering", "device execution" |
| **Domain** | A specific problem space (audio, analytics, UI, geometry) | "Vertical", "specialization", "domain-specific" |
| **Adapter** | Translator between domain language and IR | "Frontend", "dialect", "domain-specific compiler" |
| **Primitive** | Minimal, irreducible building block | "Core abstraction", "kernel operation" |
| **Service** | Pluggable policy implementation (userspace) | "Plugin", "module", "implementation" |
| **Kernel** | Minimal mechanism (NOT policy) | "Core", "TCB", "primitives layer" |

---

## 🧬 Part 2: The Universal Pattern

**Every SIL system follows this 3-layer pattern:**

```
┌─────────────────────────────────────────────┐
│  LAYER 1: INTENT                            │
│  What the user wants to express             │
│  (Domain-specific languages, high-level)    │
└──────────────────┬──────────────────────────┘
                   │ Translate to
┌──────────────────▼──────────────────────────┐
│  LAYER 2: IR (Intermediate Representation)  │
│  Canonical semantic representation          │
│  (Universal graph, types, constraints)      │
└──────────────────┬──────────────────────────┘
                   │ Lower to
┌──────────────────▼──────────────────────────┐
│  LAYER 3: EXECUTION                         │
│  How it runs on hardware                    │
│  (CPU, GPU, MLIR, frameworks)               │
└─────────────────────────────────────────────┘
```

**This is THE pattern. Everything else is elaboration.**

---

## 🏗️ Part 3: The Two Architectural Styles

SIL systems use one of two architectural patterns:

### **Style A: Adapter Architecture** (Pantheon, RiffStack, SUP, TiaCAD)

**Purpose:** Cross-domain composition and universal representation

```
┌────────────────────────────────────────────────────┐
│  DOMAIN ADAPTERS (Layer 1)                         │
│  Multiple domain-specific frontends                │
│  ┌────────┐  ┌────────┐  ┌────────┐              │
│  │ Audio  │  │ UI     │  │ Geo    │              │
│  │ DSL    │  │ DSL    │  │ DSL    │              │
│  └────┬───┘  └───┬────┘  └───┬────┘              │
└───────┼──────────┼───────────┼────────────────────┘
        │          │           │ Emit IR
┌───────┴──────────┴───────────┴────────────────────┐
│  UNIVERSAL IR (Layer 2)                            │
│  Single canonical representation                   │
│  (Enables cross-domain operations)                 │
└──────────────────┬─────────────────────────────────┘
                   │ Lower to
┌──────────────────▼─────────────────────────────────┐
│  EXECUTION BACKENDS (Layer 3)                      │
│  Multiple execution targets                        │
│  ┌────────┐  ┌────────┐  ┌────────┐              │
│  │ MLIR   │  │ WebAU  │  │ React  │              │
│  └────────┘  └────────┘  └────────┘              │
└────────────────────────────────────────────────────┘
```

**Characteristics:**
- ✅ Cross-domain composition (audio + UI + CAD)
- ✅ Multiple frontends → single IR → multiple backends
- ✅ Universal semantic graph
- ✅ Enables novel combinations
- ✅ Examples: Pantheon, Morphogen, SUP, TiaCAD, RiffStack

---

### **Style B: Microkernel Architecture** (Prism, SEM)

**Purpose:** Competing policies with minimal trusted core

```
┌────────────────────────────────────────────────────┐
│  SERVICE BUNDLES (Userspace - Layer 1+2)          │
│  Competing policy implementations                  │
│  ┌─────────────┐        ┌─────────────┐          │
│  │ Service A   │        │ Service B   │          │
│  │ (SetStack)  │        │ (SEM)       │          │
│  ├─────────────┤        ├─────────────┤          │
│  │ Parser      │        │ Parser      │          │
│  │ Optimizer   │        │ Optimizer   │          │
│  │ Scheduler   │        │ Scheduler   │          │
│  └──────┬──────┘        └──────┬──────┘          │
└─────────┼────────────────────┼────────────────────┘
          │                    │ Use kernel API
┌─────────┴────────────────────┴────────────────────┐
│  MICROKERNEL (Layer 3)                            │
│  Minimal primitives (mechanism only)              │
│  ┌──────────────────────────────────────────┐    │
│  │ Primitives: Operators, Buffers, Channels │    │
│  │ Syscalls: op_create, buf_alloc, chan_send│    │
│  └──────────────────────────────────────────┘    │
└────────────────────────────────────────────────────┘
```

**Characteristics:**
- ✅ Minimal trusted core (formal verification possible)
- ✅ Competing service implementations
- ✅ Users choose service at runtime
- ✅ Isolation and security
- ✅ Examples: Prism microkernel (SetStack vs SEM services)

---

## 🗺️ Part 4: Mapping All Projects

### **Pantheon** (Universal Adapter Architecture)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent** | Domain Adapters | Morphogen DSL, TiaCAD YAML, SUP SCM, RiffStack Harmony |
| **IR** | Pantheon Semantic IR | Universal graph (nodes, edges, types, metadata) |
| **Execution** | Domain Backends | MLIR, CadQuery, React/Vue, WebAudio |

**Pattern:** Adapter Architecture (Style A)
**Purpose:** Cross-domain composition

---

### **Prism** (Analytics Microkernel)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent** | Service Parsers | SetLang (SetStack), SQL (SEM) |
| **IR** | Service Optimizers | Cascades (SetStack), Mesh Scheduler (SEM) |
| **Execution** | Prism Microkernel | 3 primitives: operators, buffers, channels |

**Pattern:** Microkernel Architecture (Style B)
**Purpose:** Competing query execution strategies

---

### **RiffStack/Harmony** (Audio Multi-Layer IR)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent (IR 0)** | Harmony DSL | `Am9.lush.hold`, `+4:Dm9.smooth` |
| **IR (IR 1-2)** | Event IR + Timbre IR | Notes/time + DSP graphs |
| **Execution (IR 3)** | Audio Engine | WebAudio, MLIR, GPU kernels |

**Pattern:** Adapter Architecture (Style A)
**Purpose:** Musical intent → sound
**Note:** Uses 4 sub-layers within the 3-layer pattern

---

### **SEM** (Set Execution Mesh)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent (L1-2)** | Query Parser + Optimizer | SQL → Logical Plan |
| **IR (L3)** | Physical Plan Mesh | Strategy + Resource + Execution meshes |
| **Execution (L4-5)** | Device Kernels + Trace | GPU kernels, telemetry |

**Pattern:** Service implementation for Prism microkernel
**Purpose:** GPU-first query execution
**Note:** Uses 5 sub-layers within the 3-layer pattern

---

### **SUP** (Semantic UI Platform)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent** | SCM (Semantic Component Model) | YAML UI definitions |
| **IR** | Semantic UI IR | Component graphs, token systems |
| **Execution** | Multi-Framework Compiler | React, Vue, Svelte, HTML |

**Pattern:** Adapter Architecture (Style A)
**Purpose:** Semantic UI → multiple frameworks

---

### **TiaCAD** (Parametric CAD)

| Layer | Component | Description |
|-------|-----------|-------------|
| **Intent** | YAML Geometry | Declarative constraints |
| **IR** | Constraint Graph | Geometry + relationships |
| **Execution** | CadQuery Backend | OpenCASCADE, STL export |

**Pattern:** Adapter Architecture (Style A)
**Purpose:** Declarative geometry

---

## 🎓 Part 5: Universal Patterns Explained

### Pattern 1: The 3-Layer Principle

**Always exactly 3 conceptual layers:**
1. **Intent** - What you want
2. **IR** - Universal representation
3. **Execution** - How it runs

**Even when projects claim "4 layers", "5 layers", "8 layers":**
- Those are **subdivisions** within the 3-layer pattern
- Example: SEM's "5 layers" = Intent (L1-2) + IR (L3) + Execution (L4-5)
- Example: RiffStack's "4 IRs" = Intent (IR0) + IR (IR1-2) + Execution (IR3)

**The rule:** If it compiles/interprets/transforms, it follows Intent → IR → Execution

---

### Pattern 2: When to Use Each Architecture Style

| Use Adapter Architecture (A) When... | Use Microkernel Architecture (B) When... |
|--------------------------------------|------------------------------------------|
| ✅ Need cross-domain composition | ✅ Need competing implementations |
| ✅ Multiple frontends → one IR | ✅ Need formal verification (small TCB) |
| ✅ Building a universal platform | ✅ Need security isolation |
| ✅ Enabling novel combinations | ✅ Performance-critical core |
| **Example:** Pantheon, RiffStack, SUP | **Example:** Prism, OS kernels |

---

### Pattern 3: IR Design Principles

**Every IR must have:**

1. **Nodes/Operators** - Computational units
2. **Edges/Dataflow** - How data moves
3. **Types** - What data means (semantic types, not just int/float)
4. **Metadata** - Provenance, annotations, domain info
5. **Validation** - Type checking, constraint satisfaction

**This applies to:**
- Pantheon IR (universal graph)
- Prism operators (query execution)
- RiffStack Event IR (musical events)
- SEM Physical Plan (execution mesh)

---

## 🧭 Part 6: Decision Framework

### "Where does my new component go?"

**Ask these questions in order:**

#### Q1: Is it domain-specific or universal?
- **Domain-specific** → Create adapter (Style A)
- **Universal** → Extend Pantheon IR (Style A core)

#### Q2: Does it need competing implementations?
- **Yes** → Use microkernel pattern (Style B)
- **No** → Use adapter pattern (Style A)

#### Q3: Is it mechanism or policy?
- **Mechanism** → Belongs in kernel/core
- **Policy** → Belongs in service/adapter

#### Q4: What layer does it operate at?
- **Intent** → Parser, DSL, frontend
- **IR** → Graph operations, transformations
- **Execution** → Backend, runtime, lowering

---

## 📊 Part 7: Unified Terminology Map

### Old Terms → New Canonical Terms

| You Might Say | Say This Instead | Why |
|---------------|------------------|-----|
| "USIR" | **IR** or **Pantheon IR** | Simpler, clear context |
| "Semantic IR" | **IR** | All our IRs are semantic |
| "Frontend" | **Adapter** (Style A) or **Parser** (Style B) | More precise |
| "Backend" | **Execution Target** or **Lowering** | Clearer intent |
| "Layer 1, 2, 3..." | **Intent, IR, Execution** | Universal pattern |
| "Vertical" | **Domain** | Clearer meaning |
| "Stack" | **Architecture** or **Pipeline** | Avoids confusion |

---

## 🎯 Part 8: Quick Reference Examples

### Example 1: "I want to add chemistry simulation"

**Decision process:**
1. Q1: Domain-specific → Create adapter
2. Q2: No competing implementations → Adapter pattern (A)
3. Q3: Mostly policy → Adapter
4. Q4: All three layers needed

**Implementation:**
```
Intent:     ChemistryDSL (YAML molecules, reactions)
IR:         Pantheon IR (molecule nodes, reaction edges)
Execution:  Simulation backend (molecular dynamics engine)
```

**Location:** `pantheon/adapters/chemistry/`

---

### Example 2: "I want to optimize database queries"

**This is Prism!** Already specified.

**Pattern:** Microkernel (B) - competing query execution strategies

**Why:** Multiple valid approaches (SetStack explainability vs SEM GPU-performance)

---

### Example 3: "I want to generate music from natural language"

**Decision process:**
1. Q1: Domain-specific (music) → Use RiffStack
2. Q2: No competition → Adapter
3. Q4: Intent layer (NL → Harmony DSL)

**Implementation:**
```
Intent:     NL Prompt → Harmony DSL adapter
            "Create a jazzy chord progression"
            → "Dm9.lush.smooth / +5.bright / ..."
IR:         RiffStack Event IR
Execution:  WebAudio / MLIR
```

**Location:** `riffstack/adapters/nlp/` (new adapter for RiffStack)

---

## 🔬 Part 9: Advanced Concepts

### Composability Across Domains

**One of SIL's superpowers:** Cross-domain operations via universal IR

**Example:**
```yaml
# Pantheon enables this:
audio_waveform = morphogen.synthesize(freq=440)
cad_shape = tiacad.extrude_along_path(
    path: audio_waveform.envelope()
)
ui_visualizer = sup.create_visualizer(
    data: audio_waveform.fft()
)
```

**How it works:**
- Each domain emits Pantheon IR
- Pantheon IR is composable (all use same graph structure)
- Cross-domain edges are valid (audio signal → CAD path)

**This is only possible with Adapter Architecture (Style A)**

---

### Microkernel Composition

**Microkernels enable competing policies:**

```bash
# User chooses execution strategy at runtime
prism --service=setstack query.sql   # Explainability-first
prism --service=sem query.sql        # GPU-first

# Or mix-and-match
prism --parser=setlang --scheduler=mesh query.sql
```

**This is only possible with Microkernel Architecture (Style B)**

---

## 📐 Part 10: The Meta-Pattern

**Here's the deepest insight:**

### Everything is Intent → IR → Execution

**Even meta-systems follow this:**

| System | Intent | IR | Execution |
|--------|--------|-----|-----------|
| **Pantheon** | Domain DSLs | Semantic Graph | MLIR/Frameworks |
| **Prism** | SQL/SetLang | Physical Plan | Kernel Operators |
| **RiffStack** | Harmony DSL | Event+Timbre IR | Audio Engine |
| **SEM** | Query | Physical Mesh | GPU Kernels |
| **Compilers** | Source Code | AST/IR | Machine Code |
| **Databases** | SQL | Query Plan | B-Trees/Storage |
| **Graphics** | Shader Code | SPIR-V | GPU |
| **SIL** | Research Vision | Specifications | Implementations |

**The pattern is universal.**

---

## 🎓 Part 11: How to Use This Guide

### For New Team Members
1. Read this document first
2. Understand: Intent → IR → Execution
3. Learn the two architectural styles (A and B)
4. See how your project maps to the framework
5. Use canonical vocabulary

### For Architects
1. Use decision framework (Part 6) for new components
2. Choose architectural style based on requirements
3. Follow SIL design principles (Clarity, Simplicity, Composability, Correctness, Verifiability)
4. Map your layers to: Intent → IR → Execution

### For Implementers
1. Identify which layer you're working in
2. Use established patterns from similar projects
3. Reference specific project docs for details
4. Maintain vocabulary consistency

---

## 📚 Part 12: Related Documentation

**Core SIL:**
- [SIL Design Principles](/foundations/design-principles) - The 5 principles
- [Project Index](/projects/PROJECT_INDEX) - All projects mapped

**Concrete Implementations:**
- Pantheon - Adapter architecture (USIR implementation)
- Prism - Microkernel architecture (semantic reasoning kernel)
- RiffStack - Domain-specific IR for audio/music
- Morphogen - Cross-domain computation engine

See individual project repositories for detailed architecture documentation.

---

## ✨ Summary: The One-Page Takeaway

### The Universal Pattern
```
Intent → IR → Execution (always)
```

### The Two Architectural Styles
```
A) Adapter:      Multiple Frontends → Universal IR → Multiple Backends
B) Microkernel:  Services (policy) → Kernel API → Primitives (mechanism)
```

### The Canonical Vocabulary
- **Intent** (not "input", "frontend", "declarative layer")
- **IR** (not "USIR", "semantic IR", "graph")
- **Execution** (not "backend", "runtime", "lowering")
- **Domain** (not "vertical", "specialization")
- **Adapter** (not "frontend", "dialect") - for Style A
- **Service** (not "plugin", "module") - for Style B
- **Kernel** (not "core", "primitives") - for Style B

### The Decision Framework
1. Domain-specific or universal?
2. Need competing implementations?
3. Mechanism or policy?
4. Which layer? (Intent / IR / Execution)

### The Design Principles (Always)
1. **Clarity** - Can you see it?
2. **Simplicity** - Minimal complexity?
3. **Composability** - Can it combine?
4. **Correctness** - Are invariants preserved?
5. **Verifiability** - Can you prove it?

---

**This is the unified framework. Everything else is implementation detail.**

---

**Document Version:** 1.0
**Last Updated:** 2025-11-27
**Status:** Canonical Reference
**Maintained By:** SIL Core Team
