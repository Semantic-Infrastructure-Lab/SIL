# Semantic Infrastructure Lab (SIL)

**Building the semantic substrate for intelligent systems**

[![Projects](https://img.shields.io/badge/Projects-11-blue)]()
[![Production Ready](https://img.shields.io/badge/Production-5-green)]()
[![Tests](https://img.shields.io/badge/Tests-3250+-brightgreen)]()

---

## 🎯 What is SIL?

The **Semantic Infrastructure Lab** builds the missing foundation that enables intelligent systems to reason with **explicit meaning**, not just statistical patterns.

Contemporary AI systems lack:
- **Explicit meaning** - Concepts and relationships as stable, inspectable structures
- **Stable memory** - Durable semantic continuity across tasks and time
- **Inspectable reasoning** - Traceable, reproducible chains of inference
- **Tool integration** - Composable operators across domains (audio, CAD, simulation)
- **Provenance** - Clear lineage of transformations and assumptions

**SIL exists to build this missing layer.**

---

## 🚀 Quick Start - Try Our Production Systems

**Ready to use now:**

| Project | What It Does | Try It |
|---------|-------------|--------|
| [**reveal**](https://github.com/semantic-infrastructure-lab/reveal) | Progressive code exploration (PyPI) | `pip install reveal-cli` |
| [**morphogen**](https://github.com/semantic-infrastructure-lab/morphogen) | Cross-domain computation (audio+physics+circuits) | [Examples](https://github.com/semantic-infrastructure-lab/morphogen/examples) |
| [**tiacad**](https://github.com/semantic-infrastructure-lab/tiacad) | Declarative parametric CAD in YAML | [Tutorial](https://github.com/semantic-infrastructure-lab/tiacad/TUTORIAL.md) |
| [**genesisgraph**](https://github.com/semantic-infrastructure-lab/genesisgraph) | Verifiable process provenance | [5-Min Quickstart](https://github.com/semantic-infrastructure-lab/genesisgraph/docs/getting-started/quickstart.md) |

**[See all 11 projects →](projects/PROJECT_INDEX.md)**

---

## 🏗️ The Semantic OS

SIL's research centers on the **Semantic Operating System** - a 6-layer architecture:

```
Layer 5: Human Interfaces / SIM    (reveal, browserbridge)
Layer 4: Deterministic Engines      (morphogen, riffstack)
Layer 3: Multi-Agent Orchestration  (agent-ether)
Layer 2: Domain Modules             (morphogen, tiacad, riffstack, sup)
Layer 1: Universal Semantic IR      (pantheon)
Layer 0: Semantic Memory            (semantic-memory)

Cross-Cutting: Provenance           (genesisgraph, prism)
```

**[Complete Architecture Guide →](docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md)**

---

## 📚 Essential Reading

### For Newcomers
**Start here to understand SIL:**

1. **[Founder's Letter](docs/canonical/FOUNDERS_LETTER.md)** (10 min) ⭐ **NEW**
   - Why SIL exists: the architectural gap in modern AI
   - The 6-layer Semantic OS vision
   - Our commitment to explicit meaning and provenance

2. **[Manifesto](docs/canonical/SIL_MANIFESTO.md)** (15 min)
   - The problem: AI without semantic substrate
   - Why explicit meaning matters
   - What we're building

3. **[Project Index](projects/PROJECT_INDEX.md)** (10 min)
   - All 11 projects mapped to the Semantic OS
   - Production systems, active development, and research
   - What to try first

4. **[Unified Architecture Guide](docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md)** (20 min)
   - The universal pattern (Intent → IR → Execution)
   - How all projects fit together
   - Canonical vocabulary

### For Deep Dives

5. **[Technical Charter](docs/canonical/SIL_TECHNICAL_CHARTER.md)** (45 min)
   - Formal system architecture and specifications
   - USIR specification
   - Layer definitions and invariants

6. **[Glossary](docs/canonical/SIL_GLOSSARY.md)** (20 min)
   - 61 canonical terms
   - Precise definitions
   - Quick reference

7. **[Principles](docs/canonical/SIL_PRINCIPLES.md)** (10 min)
   - The 14 principles
   - Design constraints
   - How we build systems

8. **[Research Agenda Year 1](docs/canonical/SIL_RESEARCH_AGENDA_YEAR1.md)** (30 min)
   - Current research focus
   - Milestones and deliverables
   - Success criteria

---

## 🎓 Core Principles

**Every SIL system follows 5 design principles:**

1. **Clarity** - Structure is visible, not hidden
2. **Simplicity** - Minimal essential complexity
3. **Composability** - Components combine cleanly
4. **Correctness** - Invariants are preserved
5. **Verifiability** - Reasoning is provable

Plus 9 more operational principles including:
- Structure before heuristics
- Meaning must be explicit
- Provenance everywhere
- Determinism when promised

**[Read all 14 principles →](docs/canonical/SIL_PRINCIPLES.md)**

---

## 🗺️ Complete Project Ecosystem

**11 projects spanning the full Semantic OS stack:**

| Status | Count | Projects |
|--------|-------|----------|
| ✅ Production | 5 | morphogen, tiacad, genesisgraph, reveal, sil |
| 🔬 Research | 1 | pantheon |
| 🚧 Active Dev | 3 | riffstack, sup, browserbridge |
| 📋 Specification | 2 | prism, agent-ether |
| 💭 Planned | 1 | semantic-memory |

**[Full Project Index with Details →](projects/PROJECT_INDEX.md)**

---

## 🔬 Research Themes

SIL organizes around four research areas:

### 1. Universal Semantic Representations
How do we create IRs that work across domains?
- **pantheon** - Universal Semantic IR
- **morphogen** - Cross-domain composition

### 2. Domain-Specific Compilers
How do we compile semantic intent to execution?
- **morphogen** - Audio/physics → MLIR
- **riffstack** - Musical patterns → WebAudio
- **sup** - UI intent → React/Vue
- **tiacad** - Geometry → CAD engines

### 3. Microkernel Architectures
How do we build formally verified systems?
- **prism** - Microkernel query engine

### 4. Provenance & Verification
How do we prove computational correctness?
- **genesisgraph** - Verifiable provenance graphs

---

## 📖 Documentation Hub

### Canonical Documents (The Foundation)
Located in [`docs/canonical/`](docs/canonical/):

| Document | Size | Purpose |
|----------|------|---------|
| [Manifesto](docs/canonical/SIL_MANIFESTO.md) | 12K | Why SIL exists |
| [Technical Charter](docs/canonical/SIL_TECHNICAL_CHARTER.md) | 29K | System specification |
| [Glossary](docs/canonical/SIL_GLOSSARY.md) | 8K | Canonical vocabulary |
| [Principles](docs/canonical/SIL_PRINCIPLES.md) | 5K | The 14 principles |
| [Research Agenda](docs/canonical/SIL_RESEARCH_AGENDA_YEAR1.md) | 19K | Year 1 roadmap |

### Architecture Guides
Located in [`docs/architecture/`](docs/architecture/):

- [**Unified Architecture Guide**](docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md) - The Rosetta Stone for all SIL projects
- [**Design Principles**](docs/architecture/DESIGN_PRINCIPLES.md) - The 5 design principles in depth

### Research Papers
Located in [`docs/research/`](docs/research/):

- [**RAG as Semantic Manifold Transport**](docs/research/RAG_AS_SEMANTIC_MANIFOLD_TRANSPORT.md) - Formal treatment of retrieval-augmented generation as geometric meaning transport
- See [research directory](docs/research/README.md) for full catalog

### Implementation Guides
Located in [`docs/guides/`](docs/guides/):

- [**Optimization in SIL**](docs/guides/OPTIMIZATION_IN_SIL.md) - The "free lunch" value proposition: what optimizations you get automatically

### Project Resources
Located in [`projects/`](projects/):

- [**PROJECT_INDEX.md**](projects/PROJECT_INDEX.md) - Complete map of all 11 projects

### Supplementary
Located in [`docs/vision/`](docs/vision/) and [`docs/meta/`](docs/meta/):

- [Vision](docs/vision/SIL_VISION_COMPLETE.md) - Complete founding vision
- [Founder Background](docs/meta/FOUNDER_BACKGROUND.md) - Context and background

---

## 🎯 Use Cases

**What you can build with SIL infrastructure:**

### Cross-Domain Composition
Use **morphogen** to couple domains that never talked before:
```morphogen
# Couple fluid dynamics → acoustics → audio synthesis
use fluid, acoustics, audio

@state flow : FluidNetwork1D = engine_exhaust()
@state acoustic : AcousticField1D = waveguide_from_flow(flow)

flow(dt=0.1ms) {
    flow = flow.advance(engine_pulse(t))
    acoustic = acoustic.couple_from_fluid(flow)
    audio.play(acoustic.to_audio(mic_position=1.5m))
}
```

### Declarative CAD
Use **tiacad** for parametric 3D modeling in YAML:
```yaml
parts:
  - name: bracket
    type: box
    size: [50, 30, 10]
    operations:
      - type: fillet
        edges: all
        radius: 2
```

### Verifiable Provenance
Use **genesisgraph** to prove how things were made:
```yaml
process:
  inputs:
    - raw_data.csv
  transformations:
    - clean_data
    - train_model
  outputs:
    - model.pkl
  attestations:
    - type: compliance
      standard: "ISO-27001"
```

---

## 🏛️ Repository Structure

```
SIL/
├── docs/
│   ├── canonical/          # Core documents (manifesto, charter, principles)
│   ├── architecture/       # Architecture guides (unified guide, design principles)
│   ├── vision/             # Vision documents
│   └── meta/               # Background and context
├── projects/
│   └── PROJECT_INDEX.md    # Complete project map
├── archive/                # Historical materials
├── LICENSE                 # MIT License
└── README.md               # This file
```

---

## 🤝 Contributing

SIL welcomes contributions! Each project has specific contribution guidelines in its repository.

**General principles:**
- Follow the 5 design principles (Clarity, Simplicity, Composability, Correctness, Verifiability)
- Write tests for all functionality
- Document design decisions
- Preserve semantic invariants

See individual project repositories for specific guidelines.

---

## 📊 Project Statistics

- **Total Projects:** 11
- **Production Systems:** 5 (ready to use)
- **Test Coverage:** 3,250+ tests across all projects
- **Code:** ~45,000 lines (production projects)
- **Documentation:** ~15,000 lines (canonical + guides)

---

## 🌐 Community

**Website:** https://semanticinfrastructurelab.org
**GitHub Organization:** https://github.com/semantic-infrastructure-lab

**Production Projects on GitHub:**
- [morphogen](https://github.com/semantic-infrastructure-lab/morphogen) - Universal computation
- [tiacad](https://github.com/semantic-infrastructure-lab/tiacad) - Declarative CAD
- [genesisgraph](https://github.com/semantic-infrastructure-lab/genesisgraph) - Verifiable provenance
- [reveal](https://github.com/semantic-infrastructure-lab/reveal) - Code exploration (also on [PyPI](https://pypi.org/project/reveal-cli/))

---

## 📬 Contact

For inquiries about collaboration, research partnerships, or contributions:

- **GitHub Issues:** Preferred for technical questions
- **Email:** *(contact information coming soon)*

---

## 📜 License

**Code:** MIT License (see [LICENSE](LICENSE))
**Documentation:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

---

## ⚡ Next Steps

**New to SIL?**
1. Read the [Founder's Letter](docs/canonical/FOUNDERS_LETTER.md) (10 min) ⭐
2. Read the [Manifesto](docs/canonical/SIL_MANIFESTO.md) (15 min)
3. Browse the [Project Index](projects/PROJECT_INDEX.md) (10 min)
4. Try a production project (reveal, morphogen, tiacad, genesisgraph)

**Want to understand the architecture?**
1. Read the [Unified Architecture Guide](docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md) (20 min)
2. Read the [Technical Charter](docs/canonical/SIL_TECHNICAL_CHARTER.md) (45 min)

**Want to contribute?**
1. Read the [Principles](docs/canonical/SIL_PRINCIPLES.md) (10 min)
2. Pick a project from the [Project Index](projects/PROJECT_INDEX.md)
3. Check the project's contribution guide

---

**Semantic Infrastructure Lab**
*Building the semantic substrate for intelligent systems*

**Website:** https://semanticinfrastructurelab.org
**Last Updated:** 2025-11-28
**Document Version:** 2.1
