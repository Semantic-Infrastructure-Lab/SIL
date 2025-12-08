# SIL Project Index

**The Complete Map of Semantic Infrastructure Lab Projects**

**Last Updated:** 2025-12-05
**Total Projects:** 12
**Production Ready:** 4
**Git Initialized:** 12 (all in SIL GitHub org, 7 private)

**See also:** For filesystem locations and git URLs, see `/home/scottsen/src/tia/projects/SIL/SIL_ECOSYSTEM_PROJECT_LAYOUT.md`

---

## 🗺️ Overview

This index maps all SIL projects to the **6-Layer Semantic OS Architecture**. Each project embodies SIL principles (Clarity, Simplicity, Composability, Correctness, Verifiability) and contributes to building the semantic substrate for intelligent systems.

**Architecture Reference:** [Unified Architecture Guide](../docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md)

### 🔒 Repository Status

All 12 SIL projects are now in the Semantic-Infrastructure-Lab GitHub organization:
- **7 Public Repos:** SIL, reveal, morphogen, tiacad, genesisgraph, riffstack, philbrick
- **5 Private Repos:** pantheon, browserbridge, sup, prism, agent-ether (marked with 🔒)

Private repos are in active development and will be made public when ready for broader collaboration.

---

## 📊 Projects by Layer

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 5: Human Interfaces / SIM                            │
│  Progressive disclosure, exploration, visualization          │
│  • reveal (✅ Production v0.16.0)                            │
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
**Status:** ✅ **Production v0.11** | **Tests:** 1,600+ | **Coverage:** 85%

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

### [Philbrick](https://github.com/semantic-infrastructure-lab/philbrick) - Analog/Digital Hybrid Computing
**Status:** 🔬 **Research** | **Maturity:** Design phase | **Repo:** Public

**Layer:** 4 (Deterministic Engine - hardware implementation)

**What it does:** Sister project to Morphogen implementing the same deep architecture in analog/digital hybrid hardware. Four primitive operations (sum, integrate, nonlinearity, events) bridge software and physical computing.

**Key innovations:**
- Hardware realization of universal computation primitives
- Analog/digital hybrid architecture
- Direct correspondence to Morphogen software model
- Event-driven synchronization between domains

**Use cases:** Physical computing, analog computation, hardware/software co-design, neuromorphic systems

**Relationship:** Demonstrates that Morphogen's architecture isn't software-specific - same primitives work in silicon/analog domain

**Links:**
- Repository: `Semantic-Infrastructure-Lab/philbrick` (private)

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

### [reveal](https://github.com/semantic-infrastructure-lab/reveal) - Universal Resource Explorer
**Status:** ✅ **Production v0.16.0** | **Platform:** PyPI | **Downloads:** 100+/day

**Layer:** 5 (Human Interfaces / SIM - progressive disclosure)

**What it does:** Universal resource explorer with semantic understanding. Progressive disclosure pattern applies to ANY structured resource: code, environment variables, databases (planned), APIs (planned), containers (planned).

**Key innovations:**
- **Progressive disclosure** - Structure → Elements → Implementation (universal pattern)
- **Pattern detection** - Code quality checking (bugs, security, complexity)
- **AI agent-first design** - Built-in `--agent-help` following llms.txt pattern
- **URI adapters** - Explore env://, postgres:// (planned), docker:// (planned), https:// (planned)
- **Zero configuration** - Smart defaults, auto-discovery, 18 file types
- **Perfect composability** - `filename:line` format integrates with vim, git, grep, etc.

**Use cases:** Codebase exploration, AI agent context optimization, code quality checking, infrastructure inspection, token-efficient file reading (10x savings), Unix workflows

**Demonstrates SIL Principles:**
- ✅ **Structure Before Heuristics** - Shows structure first, then content
- ✅ **Meaning Must Be Explicit** - Explicit code structure, not statistical inference
- ✅ **Provenance Everywhere** - `filename:line` format is lightweight provenance
- ✅ **Composability** - Seamless Unix tool integration (doesn't replace, augments)
- ✅ **Simplicity** - Zero configuration, smart defaults enabled by semantic types

**Links:**
- Repository: `semantic-infrastructure-lab/reveal`
- [PyPI Package](https://pypi.org/project/reveal-cli/)
- [AI Agent Guide](https://github.com/semantic-infrastructure-lab/reveal#ai-agent-support)
- [Pattern Detection](https://github.com/semantic-infrastructure-lab/reveal#pattern-detection)

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

### [Pantheon](https://github.com/semantic-infrastructure-lab/pantheon) - Universal Semantic IR
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

### [SUP](https://github.com/semantic-infrastructure-lab/sup) - Semantic UI Platform
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

### [TIA Browser Reveal](https://github.com/semantic-infrastructure-lab/tia-browser-reveal) - Browser Extension
**Status:** ✅ **Production-Ready** | **Maturity:** v0.1.0 | **Repo:** 🔒 Private

**Layer:** 5 (Human Interfaces - browser integration)

**What it does:** Browser extension for extracting semantic content from web pages. Native messaging integration with TIA command-line tools. Validates BrowserBridge architectural concepts in production.

**Key innovations:**
- Native messaging architecture (browser ↔ command line)
- Site-specific extraction presets (ChatGPT, generic pages)
- DOM query capabilities
- Full automation and testing (8/8 tests passing)

**Relationship to BrowserBridge:** Proof-of-concept that validates browser extension + command-line integration works in practice. Foundation for BrowserBridge extension package.

**Use cases:** Web content extraction, ChatGPT conversation capture, page structure analysis, browser-CLI integration

**Links:**
- Repository: `Semantic-Infrastructure-Lab/tia-browser-reveal` (private)

---

## 📋 Planned / Specification Phase

### [Prism](https://github.com/semantic-infrastructure-lab/prism) - Microkernel Query Engine
**Status:** 📋 **Specification Complete** | **Maturity:** Architecture design complete, implementation pending | **Repo:** 🌍 Public

**Layer:** Cross-Cutting (Microkernel research) + Layer 1 Frontend (Analytical/Relational domain)

**Domain:** Analytical and relational computation (queries, data processing, set operations)

**Position in SIL:** Domain-specific frontend to Pantheon, sister project to Morphogen (audio domain)

**What it is:** Microkernel-based query execution system for analytical computation. Minimal kernel (3 primitives) with competing service bundles that demonstrate policy flexibility.

**Architecture:**
- **Prism Kernel** - 3 primitives (operators, buffers, channels), 14 syscalls, ~200 line C interface
- **Set Stack Service** - Explainable analytics with Cascades optimizer, MLIR scheduler, domain operators (TimeOps, UnitOps)
- **SEM Service** - GPU-optimized mesh topology scheduler, learned optimizer, heterogeneous hardware focus
- Services compete on benchmarks; users choose based on workload

**Key architectural insight:**
Resolved "Set Stack vs SEM" question by recognizing they're not competing architectures to merge, but competing service bundles (policy) running atop the same microkernel (mechanism). Direct application of Jochen Liedtke's minimality criterion.

**Key innovations:**
- Microkernel architecture (mechanism, not policy) - kernel provides primitives, services provide optimization
- Capability-based buffer isolation (prevents leaks, enables zero-copy, formal verification)
- Pluggable optimizer services (Cascades cost-based, learned ML-guided, greedy heuristic)
- Explainable physical strategies (optimizer justifies decisions with cost models)
- Message-passing concurrency (race-free by construction, async channels)
- Hardware introspection for accurate cost estimation
- Competing service bundles prove microkernel flexibility

**Integration with Pantheon:**
```
SetLang/SQL → Prism IR (logical operators) → Pantheon IR → MLIR → Hardware
```
Domain constraints (TimeOps, UnitOps) map to Pantheon metadata. Prism trace extends Pantheon provenance model.

**Specifications complete:**
- Microkernel architecture (500 lines) - kernel interface, service model, design rationale
- Optimizer service design (747 lines) - pattern transformations, cost models, strategies
- Set Stack vs SEM resolution (436 lines) - architectural decision, microkernel insight
- SEM specification (complete 5-layer mesh topology alternative)
- Kernel interface (C header with 14 syscalls)
- Original Set Stack 8-layer design (for comparison)

**Current work:**
- Implementation planning
- Service interface prototyping
- Integration design with Pantheon IR

**Timeline:** 6-12 months to working prototype (kernel + one service)

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


## 📊 Statistics

**Total Projects:** 12
**Production Ready:** 4 (morphogen, tiacad, genesisgraph, reveal)
**Active Development:** 5 (pantheon, philbrick, riffstack, sup, browserbridge)
**Specification Phase:** 2 (prism, agent-ether)
**Planned:** 1 (semantic-memory)

**Total Test Coverage:** 3,100+ tests across all projects
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

**Last Updated:** 2025-12-05
**Document Version:** 1.1
**Maintainer:** Semantic Infrastructure Lab
