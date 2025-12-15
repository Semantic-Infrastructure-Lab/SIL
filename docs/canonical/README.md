# Canonical Documents

**The foundational documents of the Semantic Infrastructure Lab**

> **Navigation:** This is the index of canonical documents.
> - [Start Here](START_HERE.md) — New to SIL? Begin there

These are the authoritative, definitive documents that define SIL's mission, principles, technical architecture, and governance.

**Document Count:** 21 canonical documents across 5 categories

---

## Core Foundation

### [Manifesto](SIL_MANIFESTO.md) ⭐ Start Here
**15 minutes** | Why semantic infrastructure matters

The problem with contemporary AI systems: they lack explicit meaning, stable memory, inspectable reasoning, and provenance. SIL exists to build this missing layer.

---

### [Principles](SIL_PRINCIPLES.md)
**10 minutes** | The 14 principles that guide all SIL work

- 5 core principles (Clarity, Simplicity, Composability, Correctness, Verifiability)
- 9 operational principles (Structure before heuristics, Meaning must be explicit, Provenance everywhere, etc.)

---

### [Glossary](SIL_GLOSSARY.md)
**Reference** | Canonical vocabulary (108+ terms)

Precise definitions for all core concepts including trust, governance, and agency primitives. **Keep this open while reading other documents.**

---

### [Semantic OS Architecture](SIL_SEMANTIC_OS_ARCHITECTURE.md)
**30 minutes** | Six-layer technical architecture

The 6-layer Semantic Operating System: Semantic Memory → USIR → Domain Modules → Agent Orchestration → Deterministic Engines → Semantic Interfaces

---

### [Stewardship Manifesto](SIL_STEWARDSHIP_MANIFESTO.md)
**20 minutes** | Values and governance

How SIL approaches stewardship, decision-making, and long-term sustainability.

---

### [Founder's Letter](FOUNDERS_LETTER.md)
**10 minutes** | Personal perspective

Why SIL exists, what gap it fills in modern AI infrastructure, and the commitment to explicit meaning and provenance.

---

## Trust & Governance

These documents define how trust, authorization, and agency work in semantic systems. This is the "governance kernel" — the primitives that enable legally-defensible, auditable agent behavior.

### [Trust Assertion Protocol (TAP)](TRUST_ASSERTION_PROTOCOL.md)
**25 minutes** | Trust claims and verification

How agents and humans make verifiable trust claims. Covers 7 claim types, proof mechanisms (signatures, ZK-SNARKs, SD-JWT), and the Semantic Passport.

**Key insight:** TAP proves "X can do Y" (capability). It does NOT prove "X may do Y" (authorization).

---

### [Authorization Protocol](AUTHORIZATION_PROTOCOL.md)
**20 minutes** | Permission and authority

The primitive that grants permission to act. Distinct from TAP — authorization is explicit permission with scope, constraints, and expiration.

**Key insight:** TAP + Authorization = Complete trust model. TAP without Authorization is capability without permission.

---

### [Hierarchical Agency Framework](HIERARCHICAL_AGENCY_FRAMEWORK.md)
**35 minutes** | Agency levels and delegation

How agency is calibrated by level (Strategic → Operational → Tactical → Execution). Covers delegation depth limits, authority validation, and the "why" hierarchy.

**Key insight:** Agency shrinks down the chain. Lower levels get narrower scope and less context.

---

### [Safety Thresholds](SIL_SAFETY_THRESHOLDS.md)
**15 minutes** | Safety boundaries and escalation

When agents must escalate vs act autonomously. Defines blast radius limits, reversibility requirements, and human-in-loop triggers.

---

## Research Contributions

Theoretical frameworks and research papers emerging from SIL work.

### [Semantic Feedback Loops](SEMANTIC_FEEDBACK_LOOPS.md)
**40 minutes** | Closed-loop control for semantic systems

How semantic systems achieve precision through reflection-measurement-correction loops.

---

### [Semantic Observability](SEMANTIC_OBSERVABILITY.md)
**45 minutes** | Intent-execution alignment detection

Framework for measuring semantic system health through vector embeddings and multi-dimensional fitness metrics.

---

### [Multi-Agent Protocol Principles](MULTI_AGENT_PROTOCOL_PRINCIPLES.md)
**30 minutes** | Agent coordination patterns

Principles for designing multi-agent communication protocols that enable composable, transparent collaboration.

---

### [Founder's Note: Multi-Shot Agent Learning](FOUNDERS_NOTE_MULTISHOT_AGENT_LEARNING.md)
**35 minutes** | Learning across interactions

How agents learn and improve through multiple interaction cycles, building institutional memory.

---

## Technical Deep Dives

Detailed technical specifications and implementation guidance.

### [Design Principles](SIL_DESIGN_PRINCIPLES.md)
**20 minutes** | Engineering principles

Detailed engineering principles for building semantic infrastructure.

---

### [Technical Charter](SIL_TECHNICAL_CHARTER.md)
**25 minutes** | Technical governance

Charter defining technical decision-making, standards, and architectural authority.

---

### [Progressive Disclosure Guide](PROGRESSIVE_DISCLOSURE_GUIDE.md)
**20 minutes** | Token-efficient information delivery

How to structure information for progressive disclosure — the pattern that achieves 25x token reduction.

---

### [Reveal + Beth Progressive Knowledge](REVEAL_BETH_PROGRESSIVE_KNOWLEDGE_SYSTEM.md)
**15 minutes** | Discovery tools

How Reveal and Beth work together for progressive codebase exploration.

---

### [Tool Quality Monitoring](SIL_TOOL_QUALITY_MONITORING.md)
**15 minutes** | Tool health metrics

How to monitor and measure tool quality in production semantic systems.

---

### [Year 1 Research Agenda](SIL_RESEARCH_AGENDA_YEAR1.md)
**20 minutes** | Research priorities

Research priorities and deliverables for SIL's first year.

---

## Reading Paths

### Path 1: Quick Start (30 minutes)
**Perfect for:** First-time visitors, curious developers

1. Manifesto (15 min) ← Why SIL exists
2. Glossary (quick reference) ← Key terms
3. Founder's Letter (10 min) ← Personal context

**Output:** You understand what SIL is and why it matters.

---

### Path 2: Technical Understanding (1.5 hours)
**Perfect for:** Engineers, architects who want depth

1. Manifesto (15 min)
2. Principles (10 min)
3. Semantic OS Architecture (30 min)
4. Stewardship Manifesto (20 min)
5. Glossary (ongoing reference)

**Output:** You understand SIL's technical architecture and governance model.

---

### Path 3: Trust & Governance Deep Dive (1.5 hours)
**Perfect for:** Those building agent systems, legal/compliance folks

1. Trust Assertion Protocol (25 min) ← How trust claims work
2. Authorization Protocol (20 min) ← How permission works
3. Hierarchical Agency Framework (35 min) ← How agency is structured
4. Safety Thresholds (15 min) ← When to escalate

**Output:** You understand how SIL enables legally-defensible, auditable agent behavior.

---

### Path 4: Complete Mastery (4+ hours)
**Perfect for:** Core contributors, stewards

Read all documents in order by section, then explore:
- [Tools](../tools/) - See SIL principles in production code
- [Research](../research/) - Formal research papers
- [Architecture](../architecture/) - The universal pattern

**Output:** Complete understanding of SIL's foundation.

---

## Document Organization

| Category | Count | Reading Time |
|----------|-------|--------------|
| Core Foundation | 6 | ~1.5 hours |
| Trust & Governance | 4 | ~1.5 hours |
| Research Contributions | 4 | ~2.5 hours |
| Technical Deep Dives | 6 | ~2 hours |
| **Total** | **21** | **~7.5 hours** |

---

## Quick Reference by Topic

**Trust:** TAP → Authorization → Safety Thresholds
**Agency:** Hierarchical Agency → Multi-Agent Protocols → Safety Thresholds
**Architecture:** Semantic OS → Design Principles → Technical Charter
**Learning:** Multi-Shot Learning → Semantic Feedback Loops → Semantic Observability
**Tools:** Progressive Disclosure → Reveal+Beth → Tool Quality Monitoring

---

## Navigation

- **New here?** [Start Here](START_HERE.md)
- **Production tools:** [Tools](../tools/)
- **Research papers:** [Research](../research/)
- **Architecture patterns:** [Architecture](../architecture/)

---

**Last Updated:** 2025-12-14
**Total Documents:** 21 canonical docs across 5 categories
**Estimated Total Reading Time:** ~7.5 hours
