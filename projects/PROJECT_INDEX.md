# SIL Project Index

**The Complete Map of Semantic Infrastructure Lab Projects**

**Last Updated:** 2025-11-28
**Total Projects:** 11
**Production Ready:** 5
**Git Initialized:** 11 (4 private development repos)

---

## 🗺️ Overview

This index maps all SIL projects to the **6-Layer Semantic OS Architecture**. Each project embodies SIL principles (Clarity, Simplicity, Composability, Correctness, Verifiability) and contributes to building the semantic substrate for intelligent systems.

**Architecture Reference:** [Unified Architecture Guide](../docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md)

### 🔒 Repository Status

All 11 SIL projects are now git-initialized:
- **7 Public Repos:** morphogen, tiacad, genesisgraph, reveal, SIL, riffstack, browserbridge
- **4 Private Repos:** pantheon, agent-ether, sup, prism (marked with 🔒)

Private repos are in active development and will be made public when ready for broader collaboration.

---

## 📊 Projects by Layer

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 5: Human Interfaces / SIM                            │
│  Progressive disclosure, exploration, visualization          │
│  • reveal (✅ Production v0.9.0)                             │
│  • browserbridge (🚧 Alpha)                                  │
└─────────────────────────────────────────────────────────────┘
                            ▲
                            │
┌─────────────────────────────────────────────────────────────┐
│  Layer 4: Deterministic Engines                             │
│  MLIR compilation, reproducible execution                   │
│  • morphogen (✅ Production v0.11)                           │
│  • riffstack (🚧 MVP)                                        │
└─────────────────────────────────────────────────────────────┘
                            ▲
                            │
┌─────────────────────────────────────────────────────────────┐
│  Layer 3: Multi-Agent Orchestration                         │
│  Agent protocols, coordination                              │
│  • agent-ether (📋 Specification)                           │
└─────────────────────────────────────────────────────────────┘
                            ▲
                            │
┌─────────────────────────────────────────────────────────────┐
│  Layer 2: Domain Modules                                    │
│  Audio, CAD, UI, musical synthesis                          │
│  • morphogen (✅ Production - audio/physics)                 │
│  • tiacad (✅ Production v3.1.1 - CAD)                       │
│  • riffstack (🚧 MVP - musical MLIR)                         │
│  • sup (🚧 Alpha - semantic UI)                              │
└─────────────────────────────────────────────────────────────┘
                            ▲
                            │
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: USIR (Universal Semantic IR)                      │
│  Universal semantic representation                          │
│  • pantheon (🔬 Research)                                    │
└─────────────────────────────────────────────────────────────┘
                            ▲
                            │
┌─────────────────────────────────────────────────────────────┐
│  Layer 0: Semantic Memory                                   │
│  Persistent provenance-complete semantic graph              │
│  • semantic-memory (📋 Planned)                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  Cross-Cutting Infrastructure                               │
│  • genesisgraph (✅ Production v0.3.0 - provenance)          │
│  • prism (📋 Specification - microkernel query)             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Production Systems (Ready to Use)

### [Morphogen](https://github.com/semantic-infrastructure-lab/morphogen) - Universal Deterministic Computation
**Status:** ✅ **Production v0.11** | **Tests:** 900+ | **Coverage:** 85%

**Layers:** 2 (Domain Module) + 4 (Deterministic Engine)

**What it does:** Universal, deterministic computation platform unifying audio synthesis, physics simulation, circuit design, geometry, and optimization in one type system, scheduler, and language.

**Key innovations:**
- Cross-domain composition (audio + physics + circuits in same program)
- Deterministic execution (bitwise-identical results)
- MLIR-based compilation
- Multirate scheduling (audio @ 48kHz, physics @ 240Hz)

**Use cases:** Audio synthesis, physical modeling, multi-domain simulation, generative art

**Links:**
- Repository: `semantic-infrastructure-lab/morphogen`
- [Documentation](https://github.com/semantic-infrastructure-lab/morphogen/docs)
- [Examples](https://github.com/semantic-infrastructure-lab/morphogen/examples)

---

### [TiaCAD](https://github.com/semantic-infrastructure-lab/tiacad) - Declarative Parametric CAD
**Status:** ✅ **Production v3.1.1** | **Tests:** 1027 | **Coverage:** 92%

**Layer:** 2 (Domain Module - CAD/geometric reasoning)

**What it does:** Declarative parametric CAD system using YAML instead of code. Reference-based composition model for explicit, verifiable geometry.

**Key innovations:**
- YAML-based declarative syntax (no programming required)
- Reference-based composition (parts as peers, not hierarchy)
- Auto-generated spatial anchors
- Comprehensive schema validation

**Use cases:** Parametric 3D modeling, manufacturing, design automation, CAD workflows

**Links:**
- Repository: `semantic-infrastructure-lab/tiacad`
- [Tutorial](https://github.com/semantic-infrastructure-lab/tiacad/TUTORIAL.md)
- [Examples](https://github.com/semantic-infrastructure-lab/tiacad/examples)

---

### [GenesisGraph](https://github.com/semantic-infrastructure-lab/genesisgraph) - Universal Verifiable Provenance
**Status:** ✅ **Production v0.3.0** | **Tests:** 363 | **Coverage:** 63%

**Layer:** Cross-Cutting (Provenance infrastructure for all layers)

**What it does:** Open standard for cryptographically verifiable process provenance. Three-level selective disclosure (A/B/C) enables proving compliance without revealing trade secrets.

**Key innovations:**
- Selective disclosure (prove compliance without revealing IP)
- DID-based identity (did:web, did:ion, did:ethr)
- Zero-knowledge proof templates
- Transparency log anchoring

**Use cases:** AI pipeline verification, manufacturing compliance, scientific reproducibility, healthcare audit trails

**Links:**
- Repository: `semantic-infrastructure-lab/genesisgraph`
- [5-Minute Quickstart](https://github.com/semantic-infrastructure-lab/genesisgraph/docs/getting-started/quickstart.md)
- [Use Cases](https://github.com/semantic-infrastructure-lab/genesisgraph/docs/use-cases.md)

---

### [reveal](https://github.com/semantic-infrastructure-lab/reveal) - Progressive Code Disclosure
**Status:** ✅ **Production v0.9.0** | **Platform:** PyPI | **Downloads:** 10K+

**Layer:** 5 (Human Interfaces / SIM - progressive disclosure)

**What it does:** Semantic exploration tool for codebases. Smart, progressive disclosure of code structure without reading entire files. Optimized for AI agents and developers.

**Key innovations:**
- Progressive disclosure (structure → elements → implementation)
- Zero configuration (smart defaults)
- 18 file types supported (Python, JS, TS, Rust, Go, etc.)
- Perfect Unix integration (`filename:line` format for vim, git, grep)

**Use cases:** Codebase exploration, AI agent context optimization, rapid code understanding, token-efficient file reading

**Links:**
- Repository: `semantic-infrastructure-lab/reveal`
- [PyPI Package](https://pypi.org/project/reveal-cli/)
- [Documentation](https://github.com/semantic-infrastructure-lab/reveal/docs)

---

### [SIL](https://github.com/semantic-infrastructure-lab/sil) - The Lab Itself
**Status:** ✅ **Complete v1.0** | **Canonical Docs:** 5

**Layer:** Meta (Lab manifesto, architecture, principles)

**What it is:** The Semantic Infrastructure Lab itself - vision, principles, research agenda, and unified architecture for the entire ecosystem.

**Key documents:**
- [Manifesto](../docs/canonical/SIL_MANIFESTO.md) - Why SIL exists
- [Technical Charter](../docs/canonical/SIL_TECHNICAL_CHARTER.md) - System specification
- [Principles](../docs/canonical/SIL_PRINCIPLES.md) - The 14 principles
- [Unified Architecture Guide](../docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md) - The framework
- [Research Agenda Year 1](../docs/canonical/SIL_RESEARCH_AGENDA_YEAR1.md) - Current focus

---

## 🚧 Active Development (2-4 Weeks to Production)

### [Pantheon](https://github.com/scottsen/pantheon) - Universal Semantic IR
**Status:** 🔬 **Research** | **Maturity:** Prototype | **Repo:** 🔒 Private

**Layer:** 1 (USIR - Universal Semantic Intermediate Representation)

**What it does:** Universal semantic IR enabling cross-domain composition. Adapters translate domain languages (audio, CAD, UI) into common semantic graph for interoperability.

**Key innovations:**
- Universal graph representation
- Domain adapters (audio, CAD, UI, geometry)
- Semantic type system
- Cross-domain operators

**Needs before production:**
- README polish
- Adapter examples
- API documentation
- Integration tutorials

**Use cases:** Cross-domain composition, semantic transformation, universal representation layer

---

### [RiffStack](https://github.com/semantic-infrastructure-lab/riffstack) - Musical MLIR
**Status:** 🚧 **MVP** | **Maturity:** Alpha

**Layers:** 2 (Domain Module - musical synthesis) + 4 (MLIR engine)

**What it does:** Stack-based live looping and audio synthesis with YAML-driven patch configuration. Real-time performance environment for musical expression.

**Key innovations:**
- Stack-based composition
- Live looping
- MLIR compilation for performance
- YAML patch description

**Needs before production:**
- Architecture documentation
- Performance benchmarks
- Example patches library
- Stability improvements

**Use cases:** Live musical performance, audio patching, real-time synthesis

---

### [SUP](https://github.com/scottsen/sup) - Semantic UI Platform
**Status:** 🚧 **Alpha** | **Maturity:** Early development | **Repo:** 🔒 Private

**Layer:** 2 (Domain Module - UI/interaction)

**What it does:** Semantic UI platform translating intent into reactive UI components. Declarative UI description with multiple backend targets (React, Vue, native).

**Key innovations:**
- Intent → UI compilation
- Backend-agnostic (React, Vue, native)
- Semantic layout constraints
- Accessibility-first

**Needs before production:**
- API stabilization
- Component library
- Backend implementations
- Documentation

**Use cases:** Declarative UI construction, multi-platform UI, accessibility automation

---

### [BrowserBridge](https://github.com/semantic-infrastructure-lab/browserbridge) - Browser Automation
**Status:** 🚧 **Alpha** | **Maturity:** Early development

**Layer:** 5 (Human Interfaces - browser state extraction)

**What it does:** Event-driven browser automation for human-AI collaboration. Standards-based (CloudEvents, AsyncAPI, WebSocket), protocol-agnostic.

**Key innovations:**
- Event-driven architecture
- Standards-based protocols
- Semantic DOM extraction
- Human-AI collaboration primitives

**Needs before production:**
- API documentation
- Integration examples
- Protocol specification
- Stability testing

**Use cases:** Browser automation, web scraping, UI testing, human-AI collaboration

---

## 📋 Planned / Specification Phase

### [Prism](https://github.com/scottsen/prism) - Microkernel Query Engine
**Status:** 📋 **Specification** | **Maturity:** Design phase | **Repo:** 🔒 Private

**Layer:** Cross-Cutting (Microkernel research)

**What it is:** Formally verified microkernel query engine. Minimal trusted core with provable correctness guarantees.

**Key innovations:**
- Microkernel architecture (mechanism, not policy)
- Formal verification
- Minimal TCB (Trusted Computing Base)
- Composable query primitives

**Current work:**
- Consolidating "Set Stack" specification
- Formal semantics definition
- Verification strategy

**Timeline:** 6-12 months to prototype

---

### [Agent Ether](https://github.com/scottsen/agent-ether) - Agent Protocols
**Status:** 📋 **Specification** | **Maturity:** Planning | **Repo:** 🔒 Private

**Layer:** 3 (Multi-Agent Orchestration)

**What it does:** Deterministic protocols for multi-agent coordination. Message passing, state synchronization, and coordination primitives for intelligent agent systems.

**Key innovations:**
- Deterministic coordination
- Message-passing primitives
- State synchronization
- Provenance-complete interactions

**Current work:**
- Protocol specification
- Reference implementation design
- Integration patterns with Layer 1-2

**Timeline:** 3-6 months to specification v1.0

---

### [Semantic Memory](https://github.com/semantic-infrastructure-lab/semantic-memory) - Persistent Semantic Graph
**Status:** 📋 **Planned** | **Maturity:** Concept

**Layer:** 0 (Foundation - persistent semantic substrate)

**What it does:** Durable, queryable knowledge graphs with versioning. Persistent semantic continuity across tasks and time.

**Key innovations:**
- Versioned semantic graphs
- Provenance-complete transformations
- Efficient incremental updates
- Cross-session continuity

**Current work:**
- Architecture design
- Storage strategy
- Query language design

**Timeline:** 12-18 months to prototype

---

## 📈 Maturity Levels

| Symbol | Status | Description | Criteria |
|--------|--------|-------------|----------|
| ✅ | **Production** | Ready for use, stable API | 300+ tests, documentation complete, >80% coverage |
| 🔬 | **Research** | Working prototype, evolving | Core functionality present, API may change |
| 🚧 | **Alpha/MVP** | Early development, unstable | Basic features work, needs polish |
| 📋 | **Specification** | Design phase | Documentation only, no code yet |
| 💭 | **Planned** | Future project | Concept stage |

---

## 🎯 Research Themes

SIL projects cluster around four core research themes:

### 1. Universal Semantic Representations
**How do we create IRs that work across domains?**

**Projects:**
- **Pantheon** - Universal Semantic IR
- **Morphogen** - Cross-domain composition (audio + physics + circuits)

**Open questions:**
- What are the universal primitives?
- How do domains compose semantically?
- Can we prove correctness across domain boundaries?

---

### 2. Domain-Specific Compilers
**How do we compile semantic intent to execution?**

**Projects:**
- **Morphogen** - Audio/simulation DSL → MLIR
- **RiffStack** - Musical intent → WebAudio/GPU
- **SUP** - UI intent → React/Vue/native
- **TiaCAD** - Geometric intent → CadQuery/STEP

**Open questions:**
- What's the right compilation strategy per domain?
- How do we preserve semantics during lowering?
- Can we verify compiled output matches intent?

---

### 3. Microkernel Architectures
**How do we build formally verified systems?**

**Projects:**
- **Prism** - Microkernel query engine

**Open questions:**
- What belongs in the kernel vs userspace?
- How do we verify correctness formally?
- What are the minimal primitives?

---

### 4. Provenance & Verification
**How do we prove computational correctness?**

**Projects:**
- **GenesisGraph** - Verifiable process provenance

**Open questions:**
- How do we balance disclosure vs secrecy?
- What's the right granularity for provenance?
- Can we verify AI pipeline correctness?

---

## 🔗 Quick Links

### For Newcomers
1. Start: [SIL Manifesto](../docs/canonical/SIL_MANIFESTO.md) - Why SIL exists
2. Learn: [Unified Architecture Guide](../docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md) - The framework
3. Explore: Try a production project (morphogen, tiacad, genesisgraph, reveal)

### For Contributors
1. [Principles](../docs/canonical/SIL_PRINCIPLES.md) - The 14 principles
2. [Technical Charter](../docs/canonical/SIL_TECHNICAL_CHARTER.md) - System specification
3. [Research Agenda Year 1](../docs/canonical/SIL_RESEARCH_AGENDA_YEAR1.md) - Current priorities

### For Researchers
1. [Unified Architecture Guide](../docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md) - Canonical framework
2. [Design Principles](../docs/architecture/DESIGN_PRINCIPLES.md) - The 5 design principles
3. Individual project documentation

---

## 📊 Statistics

**Total Projects:** 11
**Production Ready:** 5 (morphogen, tiacad, genesisgraph, reveal, sil)
**Active Development:** 4 (pantheon, riffstack, sup, browserbridge)
**Specification Phase:** 2 (prism, agent-ether)
**Planned:** 1 (semantic-memory)

**Total Test Coverage:** 3,250+ tests across all projects
**Lines of Code:** ~45,000 (production projects)
**Documentation:** ~15,000 lines (canonical + guides)

---

## 🤝 Contributing

Each project has its own contribution guidelines. General SIL contribution principles:

1. **Clarity** - Code is clear, not clever
2. **Simplicity** - Minimal essential complexity
3. **Composability** - Components combine cleanly
4. **Correctness** - Invariants preserved, tested
5. **Verifiability** - Reasoning is provable

See individual project repositories for specific contribution guides.

---

## 📬 Contact

- **GitHub Organization:** https://github.com/semantic-infrastructure-lab
- **Lab Website:** https://sil-lab.org (coming soon)

---

**Last Updated:** 2025-11-27
**Document Version:** 1.0
**Maintainer:** Semantic Infrastructure Lab
