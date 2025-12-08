# Layer 2: Domain Modules

**Domain-specific semantic frontends for audio, CAD, UI, and more**

Layer 2 provides domain-specific languages and semantics that compile to Layer 1's Universal Semantic IR. Each domain module encapsulates the semantics, constraints, and operations unique to its problem space.

---

## What This Layer Does

**Purpose:** Translate domain expertise into structured semantic representations.

**Key Responsibilities:**
- Define domain-specific types and constraints
- Provide intuitive domain language (YAML, DSL, visual)
- Encode domain knowledge (physics, geometry, interaction patterns)
- Compile domain intent → Universal IR

**Analogy:** Like high-level programming languages (Python, Rust) that compile to LLVM IR.

---

## Projects at This Layer

### ✅ Production Projects

#### [Morphogen](https://github.com/semantic-infrastructure-lab/morphogen)
**Status:** Production v0.11 | **Tests:** 900+ | **Coverage:** 85%

**Domain:** Audio synthesis, physics simulation, circuit design, geometry

**What it does:** Universal deterministic computation platform unifying multiple domains in one type system and language.

**Key innovations:**
- Cross-domain composition (audio + physics + circuits in same program)
- Multirate scheduling (audio @ 48kHz, physics @ 240Hz)
- Deterministic execution (bitwise-identical results)
- MLIR-based compilation

**Use cases:**
- Audio synthesis with physical modeling
- Multi-domain simulation
- Generative art
- Cross-domain optimization

**Try it:**
```bash
git clone https://github.com/semantic-infrastructure-lab/morphogen
cd morphogen
# See README for setup
```

---

#### [TiaCAD](https://github.com/semantic-infrastructure-lab/tiacad)
**Status:** Production v3.1.1 | **Tests:** 1027 | **Coverage:** 92%

**Domain:** Parametric CAD, 3D geometric modeling

**What it does:** Declarative parametric CAD using YAML instead of code. Reference-based composition for explicit, verifiable geometry.

**Key innovations:**
- YAML-based syntax (no programming required)
- Reference-based composition (parts as peers, not hierarchy)
- Auto-generated spatial anchors
- Comprehensive schema validation

**Use cases:**
- Parametric 3D modeling
- Manufacturing design
- Design automation
- CAD workflow integration

**Try it:**
```bash
pip install tiacad
# See tutorial at github.com/semantic-infrastructure-lab/tiacad/TUTORIAL.md
```

---

### 🚧 Active Development

#### [RiffStack](https://github.com/semantic-infrastructure-lab/riffstack)
**Status:** MVP (Alpha)

**Domain:** Musical synthesis, live performance

**What it does:** Stack-based live looping and audio synthesis with YAML patch configuration.

**Key innovations:**
- Stack-based composition model
- Live looping primitives
- MLIR compilation for performance
- Declarative patch description

**Use cases:**
- Live musical performance
- Real-time audio patching
- Experimental synthesis

---

#### [SUP](https://github.com/scottsen/sup) 🔒 Private
**Status:** Alpha (Early Development)

**Domain:** User interfaces, interaction design

**What it does:** Semantic UI platform translating intent into reactive UI components.

**Key innovations:**
- Intent → UI compilation
- Backend-agnostic (React, Vue, native)
- Semantic layout constraints
- Accessibility-first design

**Use cases:**
- Declarative UI construction
- Multi-platform UI
- Accessibility automation

---

## How Domain Modules Fit

```
User works in domain language
    ↓
Domain Module (Layer 2)
    ↓ Compile to
Universal Semantic IR (Layer 1)
    ↓ Store in
Semantic Memory (Layer 0)
    ↓ Execute via
Deterministic Engines (Layer 4)
```

**Example Flow (Audio):**
1. User writes morphogen audio program
2. Morphogen compiles to Pantheon IR
3. IR stored in semantic memory (with provenance)
4. Morphogen engine executes → WebAudio/GPU

---

## Design Patterns for Domain Modules

### Pattern 1: Declarative DSL
**Example:** TiaCAD's YAML syntax

**Benefits:**
- Non-programmers can use it
- Schema validation catches errors early
- Clear separation of intent and implementation

### Pattern 2: Embedded Language
**Example:** Morphogen's Python-embedded DSL

**Benefits:**
- Full programming power when needed
- Gradual learning curve
- Interop with existing tools

### Pattern 3: Visual/Interactive
**Example:** SUP's UI intent specification

**Benefits:**
- Direct manipulation
- Immediate feedback
- Lowers barrier to entry

---

## Connection to Other Layers

### Depends on Layer 1 (Universal IR)
Domain modules compile to Pantheon IR:
- Audio → Pantheon audio graph
- Geometry → Pantheon spatial relations
- UI → Pantheon interaction graph

### Feeds Layer 3 (Multi-Agent)
Domain semantics enable agent reasoning:
- Agents understand domain constraints
- Cross-domain composition possible
- Semantic queries over domain knowledge

### Executed by Layer 4 (Engines)
Domain IR gets lowered to hardware:
- Morphogen → MLIR → WebAudio
- TiaCAD → CadQuery → STEP
- SUP → React compiler

### Visualized via Layer 5 (Interfaces)
Progressive disclosure of domain structure:
- `reveal` shows code structure
- Browser tools explore semantic graphs
- Future: domain-specific inspectors

---

## Key Capabilities

### 1. Domain Type Systems
Each module defines domain-specific types:
- **Audio:** Signals, Filters, Generators
- **CAD:** Parts, Operations, Assemblies
- **UI:** Components, Layouts, Interactions

### 2. Domain Constraints
Modules encode domain rules:
- **Audio:** Sample rate alignment, causality
- **CAD:** Geometric consistency, feature dependencies
- **UI:** Layout constraints, accessibility requirements

### 3. Domain Operations
Modules provide domain-appropriate primitives:
- **Audio:** Mix, Filter, Modulate
- **CAD:** Fillet, Boolean, Transform
- **UI:** Bind, Layout, Animate

### 4. Optimization Opportunities
Domain knowledge enables optimizations:
- **Audio:** Multirate scheduling, SIMD vectorization
- **CAD:** Geometry caching, incremental updates
- **UI:** Virtual DOM diffing, lazy rendering

---

## Research Themes

### 1. Cross-Domain Composition
**Question:** How do domains compose semantically?

**Example:** Morphogen's audio + physics coupling
- Fluid dynamics influences acoustics
- Acoustic waves affect particle motion
- Requires shared semantic substrate (Pantheon IR)

**See:** [Universal Semantic Representations Research](../research/)

---

### 2. Domain-Specific Compilers
**Question:** How do we preserve semantics during lowering?

**Challenge:**
- Intent: "Place this part adjacent to that one"
- IR: Geometric constraints graph
- Execution: Numerical solver

**See:** [Research Agenda](../canonical/SIL_RESEARCH_AGENDA_YEAR1.md)

---

## Building a New Domain Module

**Step 1: Define domain semantics**
- What are the primitives?
- What constraints exist?
- What operations are meaningful?

**Step 2: Design the language**
- Declarative YAML?
- Embedded DSL?
- Visual interface?

**Step 3: Create Pantheon adapter**
- Map domain types → IR types
- Implement compilation
- Preserve provenance

**Step 4: Test and document**
- Write domain-specific tests
- Document patterns and idioms
- Provide examples

**See:** [Contributing Guide](../../CONTRIBUTING.md) (when available)

---

## For Different Audiences

### Users
**Try:** morphogen, tiacad (production systems)
**Learn:** Domain-specific tutorials and examples

### Developers
**Read:** Individual project documentation
**Explore:** Pantheon adapter implementation

### Researchers
**Study:** Cross-domain composition patterns
**Investigate:** Domain-specific optimization opportunities

---

## Navigation

- **Overview:** [Semantic OS](README.md)
- **Below:** [Layer 1: Universal IR](layer-1-universal-ir.md)
- **Above:** [Layer 3: Multi-Agent Orchestration](layer-3-agent-orchestration.md)
- **Projects:** [morphogen](https://github.com/semantic-infrastructure-lab/morphogen), [tiacad](https://github.com/semantic-infrastructure-lab/tiacad)
- **Research:** [Domain-Specific Compilers](../research/)

---

**Last Updated:** 2025-11-30
**Production Projects:** 2 (morphogen, tiacad)
**Development Projects:** 2 (riffstack, sup)
**Total Lines of Code:** ~25,000 (production)
**Status:** Active development and expansion
