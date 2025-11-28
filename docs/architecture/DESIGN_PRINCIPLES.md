# SIL Design Principles

**Created:** 2025-11-27
**Status:** Core Design Philosophy
**Applies to:** All SIL projects (Pantheon, Prism, Morphogen, TiaCAD, GenesisGraph)

---

## The Five Principles

```
Clarity       → lets you see the system
Simplicity    → lets you model the system
Composability → lets you extend the system
Correctness   → keeps the system safe
Verifiability → lets you trust the system
```

**In action:**

```
Clarity reveals.
Simplicity explains.
Composability scales.
Correctness guarantees.
Verifiability proves.
```

---

## 1. Clarity

**Definition:** The system's structure, behavior, and intent are immediately apparent.

**Why it matters:**
- Complex systems fail because nobody understands them
- Hidden behavior breeds bugs
- Opacity prevents debugging and reasoning

**In practice:**
- **Small interfaces** - Prism kernel: 6 syscalls
- **Obvious names** - `op_execute()` not `transform_dataflow_graph()`
- **Introspection built-in** - Every system has `explain()` and `trace()`
- **No magic** - Explicit over implicit always

**Examples:**
- ✅ Pantheon IR is YAML (human-readable, not binary)
- ✅ Prism operators are pure functions (input → output, no hidden state)
- ✅ Morphogen GraphIR shows the entire signal graph explicitly
- ❌ Magic context that changes operator behavior (violates clarity)

**Tests:**
- Can you draw the system on a whiteboard in 5 minutes?
- Can you explain it to a new engineer in 15 minutes?
- Can you see what's happening at runtime?

**Quote:**
> "If you can't explain it simply, you don't understand it well enough." - Einstein

---

## 2. Simplicity

**Definition:** The system has minimal essential complexity and zero accidental complexity.

**Why it matters:**
- Complexity is the enemy of correctness
- Simple systems are easier to verify, debug, and maintain
- Simplicity is hard work (design discipline)

**In practice:**
- **Minimal primitives** - Prism: 3 primitives (operators, buffers, channels)
- **No special cases** - One mechanism, applied uniformly
- **Remove features** - Start with everything, remove until it breaks, add back one thing
- **Orthogonal abstractions** - Each does one thing well

**Examples:**
- ✅ Liedtke's L4: 3 primitives (address spaces, threads, IPC)
- ✅ Prism kernel: 2000 lines (vs 8 layers, 10000 lines of spec)
- ✅ Go: removed inheritance, exceptions, generics (initially) → simpler language
- ❌ 8-layer stack where 5 could be userspace (violates simplicity)

**Tests:**
- Can you remove this feature without losing essential functionality?
- Can a new contributor understand the core in one day?
- Does the implementation match the mental model?

**Quote:**
> "Simplicity is the ultimate sophistication." - da Vinci
> "Perfection is achieved when there is nothing left to take away." - Saint-Exupéry

---

## 3. Composability

**Definition:** Components combine cleanly to build complex systems without special glue.

**Why it matters:**
- Reusability without rewriting
- Systems grow organically
- Innovation at the edges (Unix philosophy)

**In practice:**
- **Small, focused components** - Do one thing well
- **Standard interfaces** - Components speak the same language
- **No side channels** - All communication via explicit interfaces
- **Recursive construction** - Build complex from simple

**Examples:**
- ✅ Unix pipes: `grep | sort | uniq` - programs compose
- ✅ Prism services: SQL parser + Mesh scheduler + CUDA backend (mix & match)
- ✅ Pantheon: Morphogen → Pantheon IR ← TiaCAD (cross-domain composition)
- ❌ Hardcoded assumptions about upstream/downstream (violates composability)

**Tests:**
- Can you swap this component for a different implementation?
- Can you combine components in ways you didn't anticipate?
- Can you build new features without modifying the core?

**Quote:**
> "The power of the unaided mind is highly overrated. The real powers come from devising external aids that enhance cognitive abilities." - Norman, *Things That Make Us Smart*

---

## 4. Correctness

**Definition:** The system does what it claims to do, every time, without exception.

**Why it matters:**
- Incorrect systems are worthless (or dangerous)
- Bugs compound in complex systems
- Trust requires correctness

**In practice:**
- **Type safety** - Compiler catches errors before runtime
- **Isolation** - Failures stay contained (capabilities, sandboxing)
- **Contracts** - Explicit preconditions/postconditions
- **Defensive design** - Fail fast, fail safely

**Examples:**
- ✅ Prism capabilities: can't access buffer without grant (enforced by kernel)
- ✅ Prism operators: pure functions (testable in isolation)
- ✅ seL4: formally verified kernel (mathematically proven correct)
- ❌ Shared mutable state across components (violates correctness guarantees)

**Tests:**
- Can you prove this component is correct?
- What are the invariants, and how are they enforced?
- If this fails, what's the blast radius?

**Quote:**
> "Beware of bugs in the above code; I have only proved it correct, not tried it." - Knuth (tongue-in-cheek)

---

## 5. Verifiability

**Definition:** You can prove (formally or empirically) that the system works correctly.

**Why it matters:**
- Trust requires evidence
- Manual testing doesn't scale
- Critical systems demand proof

**In practice:**
- **Small TCB** - Minimize what needs verification (Prism kernel: 2000 lines)
- **Deterministic** - Same input → same output (reproducible)
- **Observable** - Introspection and tracing built-in
- **Testable** - Pure functions, dependency injection, property tests

**Examples:**
- ✅ seL4: formal proof of memory safety, isolation, deadlock-freedom
- ✅ Prism: 100% test coverage of kernel, property tests for services
- ✅ TPC-H: standard benchmark suite (empirical verification of correctness)
- ❌ Non-deterministic behavior, irreproducible bugs (violates verifiability)

**Tests:**
- Can you write a test that proves this works?
- Is the behavior deterministic and reproducible?
- Can you formally verify critical properties?

**Quote:**
> "Testing shows the presence, not the absence of bugs." - Dijkstra
> "Verification shows the absence, not just the presence of correctness." - Liedtke

---

## How to Use These Principles

### During Design

**Every design decision passes through this filter:**

```
Question: Should we add this feature?

Clarity:       Does it make the system easier to understand?
Simplicity:    Does it add essential complexity or accidental complexity?
Composability: Does it enable new combinations or lock in assumptions?
Correctness:   Does it preserve invariants and isolation?
Verifiability: Can we prove it works?

If "no" to any → Redesign or reject.
```

### During Code Review

**Every PR is evaluated:**

```
Clarity:       Can I understand what this does in 5 minutes?
Simplicity:    Could this be simpler without losing functionality?
Composability: Could this be reused or combined with other components?
Correctness:   Are there tests? Edge cases covered?
Verifiability: Is this deterministic? Traceable?
```

### During Debugging

**When something breaks:**

```
Clarity:       Can I see what happened? (trace, explain, introspect)
Simplicity:    Is the system small enough to reason about?
Composability: Is the failure isolated to one component?
Correctness:   Which invariant was violated?
Verifiability: Can I reproduce this reliably?
```

---

## Principles in Conflict

Sometimes principles conflict. **Hierarchy for resolution:**

### 1. Correctness > Everything
Never sacrifice correctness for simplicity, performance, or features.

**Example:** Adding a cache for performance is fine, BUT only if you can prove cache coherence is maintained.

### 2. Simplicity > Composability
A simple system that solves the core problem beats a composable system too complex to understand.

**Example:** Prism microkernel (3 primitives) vs full query planner in kernel (composable but complex).

### 3. Clarity > Performance
Understandable slow code beats incomprehensible fast code.

**Example:** Readable AST traversal vs hand-optimized bit-twiddling (optimize later if needed).

**Note:** This doesn't mean "never optimize." It means write clear semantics first, then use infrastructure to provide optimization. See [Optimization in SIL](../guides/OPTIMIZATION_IN_SIL.md) for the "free lunch" value proposition.

### 4. Verifiability scales with risk
High-risk components (security, isolation, correctness-critical) need formal verification.
Low-risk components (UI, CLI flags) need good tests.

**Example:** Prism kernel needs formal proof. Prism SQL parser needs test coverage.

---

## Cross-Project Examples

### Pantheon (Universal Semantic IR)

| Principle | Implementation |
|-----------|----------------|
| **Clarity** | YAML format (human-readable), semantic nodes with explicit intent |
| **Simplicity** | 4 core abstractions (nodes, edges, types, validation) |
| **Composability** | Domain frontends emit Pantheon IR → Domain backends consume |
| **Correctness** | Type system enforces domain constraints |
| **Verifiability** | Schema validation, round-trip tests (IR → MLIR → IR) |

### Prism (Analytics Query Engine)

| Principle | Implementation |
|-----------|----------------|
| **Clarity** | 3 primitives, 6 syscalls, introspect() API |
| **Simplicity** | Microkernel (2000 lines) vs monolithic (50000 lines) |
| **Composability** | Services plug in (parsers, optimizers, schedulers) |
| **Correctness** | Capabilities enforce isolation, pure operators |
| **Verifiability** | Formal proof of kernel, property tests for services |

### Morphogen (Audio Synthesis)

| Principle | Implementation |
|-----------|----------------|
| **Clarity** | GraphIR shows signal flow explicitly |
| **Simplicity** | Small operator set (6 core types) |
| **Composability** | Operators compose into patches |
| **Correctness** | Type system prevents invalid connections |
| **Verifiability** | Unit tests (28/28 passing), deterministic output |

### TiaCAD (Parametric CAD)

| Principle | Implementation |
|-----------|----------------|
| **Clarity** | YAML-defined geometry (declarative, not imperative) |
| **Simplicity** | Constraint-based (describe what, not how) |
| **Composability** | Primitives combine into assemblies |
| **Correctness** | Constraint solver validates feasibility |
| **Verifiability** | 1080+ tests, 92% coverage |

### GenesisGraph (Provenance)

| Principle | Implementation |
|-----------|----------------|
| **Clarity** | Explicit provenance graph (who, what, when, why) |
| **Simplicity** | Minimal schema (entities, activities, agents) |
| **Composability** | Provenance from any domain (CAD, audio, queries) |
| **Correctness** | PROV-O compliance (W3C standard) |
| **Verifiability** | Cryptographic signatures, tamper-evident logs |

---

## Anti-Patterns (Violations)

### ❌ Violates Clarity
- Magic global state that changes behavior
- Implicit context ("it just knows")
- Undocumented side effects
- Binary formats without introspection

### ❌ Violates Simplicity
- Special cases ("except when...")
- Premature abstraction (AbstractFactoryFactory)
- Kitchen-sink interfaces (100 methods)
- Layers that don't add value

### ❌ Violates Composability
- Hardcoded assumptions about upstream/downstream
- Side channels (global variables, filesystem tricks)
- Non-standard interfaces
- Tight coupling

### ❌ Violates Correctness
- Shared mutable state without synchronization
- Unchecked invariants
- Error swallowing
- Ambient authority (access without capabilities)

### ❌ Violates Verifiability
- Non-determinism (random, time-dependent)
- Irreproducible bugs
- No tests for critical paths
- Opaque execution (can't trace what happened)

---

## The Litmus Test

**Before merging ANY code, ask:**

```
1. Clarity:       Can a new engineer understand this in < 30 minutes?
2. Simplicity:    Could I remove 20% of this code without breaking it?
3. Composability: Could this be used in a way I didn't anticipate?
4. Correctness:   What are the invariants, and how are they enforced?
5. Verifiability: How would I prove this works?
```

**If you can't answer all 5 confidently → Redesign.**

---

## Influence and Lineage

These principles synthesize wisdom from:

- **Dennis Ritchie & Ken Thompson** (Unix, C) - Simplicity, composability
- **Linus Torvalds** (Linux) - Clarity through code, pragmatism
- **Rob Pike** (Go, Plan 9) - Simplicity is hard work, less is more
- **Jochen Liedtke** (L4) - Minimality, verifiability, correctness
- **Conor Cunningham** (SQL Server) - Verifiability through benchmarks
- **Gernot Heiser** (seL4) - Formal verification, correctness proofs
- **Fred Brooks** (*Mythical Man-Month*) - Essential vs accidental complexity

**But distilled into 5 core principles for SIL.**

---

## Evolution

These principles are not static. They evolve as we learn.

**Version history:**
- v1.0 (2025-11-27): Initial formulation

**Future refinements:**
- Add concrete metrics (what % code coverage = "verifiable"?)
- Case studies of principles in action
- Failure retrospectives (when we violated principles and paid the price)

---

## Conclusion

**Clarity, Simplicity, Composability, Correctness, Verifiability.**

These are not abstract ideals.
They are **engineering discipline**.

Every line of code.
Every design decision.
Every architecture.

**Passes through this filter.**

---

**"The best programs are written to be read."** - Kernighan & Plauger

**"Simplicity is prerequisite for reliability."** - Dijkstra

**"Make it correct, make it clear, make it concise, make it fast. In that order."** - Wes Dyer

---

**Document Version:** 1.0
**Last Updated:** 2025-11-27
**Status:** Core SIL Philosophy
