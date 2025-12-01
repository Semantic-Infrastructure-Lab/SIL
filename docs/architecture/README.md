# Architecture Guides

**Understanding SIL's system design and architectural principles**

These guides explain **how to think architecturally** about semantic systems. They provide the mental models, frameworks, and decision criteria for designing SIL-compliant components.

---

## Essential Reading

### [Unified Architecture Guide](UNIFIED_ARCHITECTURE_GUIDE.md) ⭐⭐⭐ The Rosetta Stone
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

### [Design Principles](DESIGN_PRINCIPLES.md)
**15 minutes** | The 5 principles for evaluating designs

Every SIL system follows these design principles:

1. **Clarity** - Structure is visible, not hidden
2. **Simplicity** - Minimal essential complexity
3. **Composability** - Components combine cleanly
4. **Correctness** - Invariants are preserved
5. **Verifiability** - Reasoning is provable

This document explains **how to use these principles** to evaluate design decisions. It's not abstract philosophy—it's a practical decision framework.

**Use this when:**
- Reviewing architecture proposals
- Debugging why a design feels wrong
- Teaching SIL principles to new contributors
- Deciding between multiple implementation approaches

---

## How These Documents Relate

```
UNIFIED_ARCHITECTURE_GUIDE.md (The Pattern)
    ↓ Uses
DESIGN_PRINCIPLES.md (The Criteria)
    ↓ Applied to
Individual project architectures
    ↓ Formalized in
../canonical/SIL_TECHNICAL_CHARTER.md (The Specification)
```

**Reading order:**
1. **Unified Architecture Guide** - Learn the pattern
2. **Design Principles** - Learn the evaluation criteria
3. **Technical Charter** - See the formal specification
4. Individual project docs - See concrete implementations

---

## Connection to Other Docs

### These are guides, not specs

**Architecture guides** (this directory):
- Mental models and frameworks
- Decision criteria
- "How to think about X"

**Canonical documents** (`../canonical/`):
- Formal specifications
- Definitive reference
- "What X is precisely"

**Research papers** (`../research/`):
- Why things are designed this way
- Theoretical foundations
- "Why X matters"

---

## For Different Audiences

### New Engineer/Contributor
**Read:** Unified Architecture Guide → Design Principles
**Time:** 1 hour
**Outcome:** Understand the pattern and principles

### Architect/Steward
**Read:** Both documents deeply + Technical Charter
**Time:** 2-3 hours
**Outcome:** Can design SIL-compliant systems

### Researcher
**Read:** Unified Architecture Guide + Research Papers
**Time:** Variable
**Outcome:** Understand architectural foundations and open problems

---

## Key Concepts Explained Here

**From Unified Architecture Guide:**
- Intent → IR → Execution pattern
- Adapter vs Microkernel architectures
- When to use which architectural style
- How all 11 SIL projects map to the framework

**From Design Principles:**
- What "Clarity" means in practice (4 sub-principles)
- How to evaluate "Simplicity" (3 criteria)
- When "Composability" is achieved (3 invariants)
- How to ensure "Correctness" (4 techniques)
- What makes systems "Verifiable" (3 requirements)

---

## Next Steps

After reading these guides:

**To understand the formal spec:**
→ Read [Technical Charter](../canonical/SIL_TECHNICAL_CHARTER.md)

**To see concrete implementations:**
→ Read [Project Index](../../projects/PROJECT_INDEX.md)

**To understand the research:**
→ Read [Research Papers](../research/)

**To learn operational details:**
→ Read [Guides](../guides/)

---

## Navigation

- **Parent:** [Documentation Hub](../READING_GUIDE.md)
- **Related:** [Canonical Documents](../canonical/), [Guides](../guides/)
- **Projects:** [Project Index](../../projects/PROJECT_INDEX.md)

---

**Last Updated:** 2025-11-30
**Documents:** 2
**Reading Time:** ~1 hour (both documents)
**Status:** Complete and stable
