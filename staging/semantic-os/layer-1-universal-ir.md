# Layer 1: Universal Semantic IR

**Cross-domain semantic intermediate representation**

Layer 1 defines the Universal Semantic IR (USIR)—a unified intermediate representation that all domain-specific languages compile to. This enables semantic composition across domains and provides a single target for reasoning, optimization, and transformation.

---

## What This Layer Does

**Purpose:** Provide a common semantic representation that spans all domains.

**Key Responsibilities:**
- Define universal semantic types and operations
- Enable cross-domain composition
- Provide target for domain language compilation
- Support semantic analysis and transformation
- Enable domain-agnostic reasoning

**Analogy:** Like LLVM IR for compilers—a common target that enables optimization and cross-language interop.

---

## Projects at This Layer

### ✅ Production Projects

#### [Pantheon](https://github.com/semantic-infrastructure-lab/pantheon)
**Status:** Production v1.0 | **Type System:** Complete

**What it does:** Universal semantic type system and intermediate representation for cross-domain knowledge work.

**Key innovations:**
- Unified type system across domains
- Compositional semantics (types compose naturally)
- Bidirectional type inference
- Provenance-preserving transformations
- Domain-agnostic query language

**Domains supported:**
- Audio synthesis and DSP
- Parametric CAD and geometry
- UI component composition
- Agent protocols and workflows
- Mathematical expressions

**Example:**
```yaml
# Audio synthesis compiles to Pantheon IR
oscillator:
  type: pantheon.audio.Oscillator
  frequency: 440Hz
  waveform: sine

# CAD geometry compiles to Pantheon IR
box:
  type: pantheon.geometry.Box
  width: 10mm
  height: 20mm
  depth: 5mm
```

Both compile to the same IR type system, enabling cross-domain reasoning.

---

## Why This Layer Matters

**Without Layer 1:**
- Domain languages are isolated silos
- No cross-domain composition
- Reasoning tied to specific domains
- Every tool reinvents semantics

**With Layer 1:**
- Unified semantic representation
- Compose audio + CAD + UI in same system
- Domain-agnostic optimization
- Single source of truth for types

---

## Design Patterns

### The Compilation Pipeline

```
Domain DSL (Layer 2)
    ↓ Compile
Universal IR (Layer 1)
    ↓ Lower
Execution Target (Layer 4)
```

Every domain language follows this pattern.

### Type Composition

Pantheon types compose naturally:

```
Audio + Physics → Audio with physical modeling
CAD + Constraints → Parametric design
UI + State → Interactive components
```

### Semantic Preservation

IR transformations preserve semantic meaning:
- Optimization doesn't change behavior
- Lowering maintains correctness
- Provenance tracks every transformation

### Cross-Domain Queries

Query the IR regardless of source domain:

```
"Find all frequency parameters > 1kHz"
"Show geometric constraints involving 'width'"
"List agent tasks with priority > 5"
```

---

## Technical Details

### Type System Features

- **Algebraic types:** Sum types, product types, recursive types
- **Effect system:** Track side effects, IO, non-determinism
- **Units tracking:** Physical units as first-class types
- **Constraints:** Express domain invariants
- **Provenance:** Every value knows its source

### IR Operations

- **Projection:** Extract subgraphs
- **Composition:** Merge compatible graphs
- **Transformation:** Apply semantic rewrites
- **Validation:** Check type/constraint correctness
- **Serialization:** Round-trip to storage

---

## Related Layers

- **Layer 2 (Domain Modules):** Compile domain languages to Layer 1
- **Layer 3 (Agent Orchestration):** Agents operate on Layer 1 representations
- **Layer 4 (Deterministic Engines):** Lower Layer 1 to executable code
- **Layer 0 (Semantic Memory):** Store Layer 1 graphs persistently

---

## Real-World Examples

### Morphogen Audio
```yaml
# Domain DSL
sine: { freq: 440Hz }

# Compiles to Pantheon IR
PantheonExpr(
  op='audio.oscillator',
  params={'frequency': Quantity(440, 'Hz'), 'waveform': 'sine'}
)
```

### TiaCAD Geometry
```yaml
# Domain DSL
box: { width: 10mm, height: 20mm }

# Compiles to Pantheon IR
PantheonExpr(
  op='geometry.box',
  params={'width': Quantity(10, 'mm'), 'height': Quantity(20, 'mm')}
)
```

Same IR type system, different domains.

---

## Technical Status

**Current State:** Production-ready, actively used by Morphogen, TiaCAD, SUP

**Features Complete:**
- ✅ Core type system
- ✅ Bidirectional inference
- ✅ Cross-domain composition
- ✅ Provenance tracking
- ✅ Serialization/deserialization

**In Progress:**
- Query language extensions
- Advanced optimization passes
- Better error messages

**Learn More:**
- [Pantheon Repository](https://github.com/semantic-infrastructure-lab/pantheon)
- [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md)
- [Layer 2: Domain Modules](layer-2-domain-modules.md) for compilation examples
