# Guides

**Practical guides for using and understanding SIL**

This directory contains how-to guides, tutorials, and practical reference material for working with SIL infrastructure, projects, and ecosystem.

---

## Available Guides

### [Optimization in SIL](OPTIMIZATION_IN_SIL.md)
**15 minutes** | The "free lunch" value proposition

What optimizations you get automatically when you build on SIL's semantic infrastructure:
- Cross-domain optimization (audio + physics)
- Semantic query optimization
- Provenance-guided caching
- Structure-aware compilation

**Why this matters:** Understanding what SIL gives you for free helps you design better systems.

---

### [SIL Ecosystem Project Layout](SIL_ECOSYSTEM_PROJECT_LAYOUT.md)
**20 minutes** | Complete map of project locations and git repos

Where code lives, how projects are organized, and the relationship between:
- Production repos (`~/src/projects/`)
- TIA workspace (`~/src/tia/projects/`)
- The two-tier structure
- Three organizational patterns (Full TIA Workspace, Lightweight Tracking, In-Development)

**Critical for:**
- Understanding where to commit code
- Knowing which projects are where
- Navigating the ecosystem
- Avoiding confusion about multiple directories

**Quick reference matrix:** Shows all 11 projects, their locations, git status, and maturity.

---

## Future Guides (Planned)

### Getting Started with SIL
Step-by-step tutorial for new users:
1. Try a production project (reveal, morphogen, tiacad)
2. Understand the architecture
3. Build your first domain module

### Contributing to SIL
How to contribute code, documentation, research:
- Code style and standards
- Testing requirements
- Documentation requirements
- PR workflow

### Building a Domain Module
Tutorial for creating new Layer 2 domain modules:
- Define your domain semantics
- Create adapter to Pantheon IR
- Implement domain-specific operations
- Write tests and docs

### Integrating with Semantic Memory
How to use Layer 0 for persistent knowledge:
- Store semantic graphs
- Track provenance
- Query with geometric constraints
- Version knowledge over time

### Using GenesisGraph for Provenance
Practical guide to verifiable process provenance:
- Define your process graph
- Attach attestations
- Selective disclosure strategies
- Verification workflows

---

## Guide Philosophy

SIL guides follow these principles:

**Practical, not theoretical:**
- Show working code examples
- Include common pitfalls
- Provide copy-paste templates

**Focused on one task:**
- Each guide answers a specific "How do I...?" question
- Clear scope and prerequisites
- Defined outcome

**Connected to architecture:**
- Link to relevant architecture docs
- Explain why, not just how
- Show where this fits in the larger system

---

## How Guides Differ from Other Docs

**Guides** (this directory):
- "How do I accomplish X?"
- Step-by-step instructions
- Practical examples and code

**Architecture** (`../architecture/`):
- "How should I think about X?"
- Mental models and patterns
- Design frameworks

**Canonical** (`../canonical/`):
- "What is X precisely?"
- Formal definitions
- Authoritative specifications

**Research** (`../research/`):
- "Why is X designed this way?"
- Theoretical foundations
- Open problems

---

## Navigation

- **Parent:** [Documentation Hub](../READING_GUIDE.md)
- **Related:** [Architecture](../architecture/), [Canonical](../canonical/)
- **Projects:** [Project Index](../../projects/PROJECT_INDEX.md)

---

## Contributing Guides

Have a useful workflow or tutorial? Guidelines for guide contributions:

1. **Clear scope** - One specific task
2. **Prerequisites** - What reader needs to know first
3. **Working examples** - Code that actually runs
4. **Common issues** - Troubleshooting section
5. **Next steps** - What to read/do after this

See main SIL repository for contribution process.

---

**Last Updated:** 2025-11-30
**Current Guides:** 2
**Planned Guides:** 5+
**Status:** Growing as ecosystem matures
