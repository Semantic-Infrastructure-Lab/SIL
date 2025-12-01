# SIL Ecosystem - Project Layout & Repository Guide

**Last Updated:** 2025-11-27
**Purpose:** Canonical reference for all SIL-related project locations, git repos, and directory structure

---

## Overview

The SIL (Semantic Infrastructure Lab) ecosystem spans multiple projects across different locations. This document maps the complete landscape to prevent confusion about where code lives vs. where planning/research happens.

---

## The Two-Tier Structure

### Tier 1: Production Code Repositories (`~/src/projects/`)

**Location:** `/home/scottsen/src/projects/`
**Purpose:** Clean production code, official documentation, publishable artifacts
**Rules:**
- NO TIA artifacts (Beth indexes, session links, etc.)
- NO research/analysis documents
- Clean git history suitable for public/GitHub publication
- Official README, LICENSE, production docs only

### Tier 2: TIA Workspace (`~/src/tia/projects/`)

**Location:** `/home/scottsen/src/tia/projects/`
**Purpose:** Research notes, analysis, project metadata, planning documents
**Contents:**
- `project.yaml` - TIA project metadata
- Research documents and analysis
- Beth integration metadata
- Session links and relationship tracking
- Experimental/planning documents

**Critical Rule:** These two tiers MUST NOT be mixed. The production repos stay clean.

---

## SIL Ecosystem Projects

### Core Infrastructure

#### 1. SIL (Semantic Infrastructure Lab)
**Official Repo:** `/home/scottsen/src/projects/SIL/`
- **Git:** https://github.com/scottsen/SIL.git
- **Status:** Published GitHub repo ✅
- **Contents:** Core SIL manifesto, technical charter, principles
- **Structure:**
  ```
  SIL/
  ├── docs/           # Official SIL documentation
  ├── archive/        # Historical documents
  ├── README.md       # Main overview
  ├── LICENSE
  └── .git/
  ```

**TIA Workspace:** `/home/scottsen/src/tia/projects/SIL/`
- **Contents:** Research notes, analysis, project.yaml
- **Key Docs:**
  - `project.yaml` - TIA metadata with explicit repo separation rules
  - `SIL_PREPARATION_INDEX.md` - Research index
  - `SIL_ECOSYSTEM_PROJECT_LAYOUT.md` - This document

#### 2. Pantheon (Universal Semantic IR)
**Official Repo:** `/home/scottsen/src/projects/pantheon/`
- **Git:** ❌ Not yet a git repository
- **Status:** Local development
- **Purpose:** Universal Semantic Intermediate Representation - the core IR layer
- **Structure:**
  ```
  pantheon/
  ├── pantheon-core/  # Core IR implementation
  ├── adapters/       # Domain adapters (Morphogen, etc.)
  ├── docs/           # Architecture documentation
  ├── examples/       # Usage examples
  ├── tests/          # Test suite
  └── tools/          # Development tools
  ```

**TIA Workspace:** `/home/scottsen/src/tia/projects/pantheon/`
- **Contents:** Research notes, project.yaml
- **Key Docs:** Planning and analysis documents

**Next Steps:**
- Initialize as git repo
- Publish to GitHub when ready

### Domain-Specific Frontends

#### 3. Morphogen (Audio Synthesis)
**Official Repo:** `/home/scottsen/src/projects/morphogen/`
- **Git:** https://github.com/scottsen/morphogen.git
- **Status:** Published GitHub repo ✅
- **Purpose:** Audio synthesis domain - first validated Pantheon frontend
- **Position:** Production-ready, demonstrates Pantheon integration pattern

**TIA Workspace:** `/home/scottsen/src/tia/projects/Morphogen/`
- **Contents:** Research notes, project.yaml

#### 4. Set Stack / Prism (Analytical Queries)
**Official Repo:** ❌ Not yet created
- **Status:** Specification phase
- **Purpose:** Relational/analytical query frontend to Pantheon
- **Note:** Rename from "Set Stack" to "Prism" pending (see violet-dawn-1126)

**TIA Workspace:** `/home/scottsen/src/tia/projects/Set Stack/`
- **Contents:** Complete v1.0 specification, project.yaml
- **Key Docs:**
  - `SET_STACK_SPECIFICATION_v1.0.md` - Full 8-layer architecture (needs update to 3-layer)
  - `SIL_ECOSYSTEM_RELATIONSHIPS.md` - Ecosystem positioning

**Next Steps:**
- Simplify architecture (8 layers → 3 layers)
- Rename to "Prism"
- Create official repo when implementation begins

#### 5. TiaCAD (Parametric CAD)
**Official Repo:** `/home/scottsen/src/projects/tiacad/`
- **Git:** https://github.com/scottsen/tiacad.git
- **Status:** ✅ Published GitHub repo, **Production-ready v3.1.1**
- **Purpose:** Declarative parametric CAD in YAML
- **Maturity:** 1080+ tests, 92% coverage, active development
- **TIA Integration:** Tracked in `.tia/projects/tiacad.yaml` (metadata only, no workspace)

**Note:** TiaCAD is a mature standalone project that predates the SIL ecosystem. Pantheon adapter directory exists (`pantheon/adapters/tiacad/`) but is currently empty.

**Next Steps:** Implement Pantheon adapter for cross-domain CAD integration

### Supporting Infrastructure

#### 6. GenesisGraph (Provenance & Lineage)
**Official Repo:** `/home/scottsen/src/projects/genesisgraph/`
- **Git:** ✅ Has `.git` directory (needs remote setup)
- **Status:** v0.1 Public Working Draft
- **Purpose:** Universal Verifiable Process Provenance standard
- **Features:** Three-level selective disclosure (A/B/C), compliance certification without revealing trade secrets
- **TIA Integration:** Tracked in `.tia/projects/genesisgraph.yaml` (metadata only, no workspace)

**Note:** GenesisGraph is a mature standalone open standard with core validation library complete. Ready for v0.1 Working Draft.

**Next Steps:** Set up GitHub remote, publish v0.1 Working Draft, integrate with Pantheon provenance layer

#### 7. Riffstack (Audio Infrastructure / MLIR)
**Official Repo:** `/home/scottsen/src/projects/riffstack/`
- **Git:** ❌ Not yet a git repository
- **Status:** MVP Complete (v0.1)
- **Purpose:** Multi-level IR (MLIR) for audio synthesis - composable audio DSL
- **Architecture:** 6-layer creative compiler (Composition → Theory → Note → Audio → DSP → Machine)
- **Position:** Domain-specific IR following Pantheon's multi-level pattern

**Next Steps:** MLIR dialect implementation, GPU backend (SPIR-V, WebGPU)

#### 8. SUP (Semantic UI Platform)
**Official Repo:** `/home/scottsen/src/projects/tia-projects/sup/`
- **Git:** ❌ Not yet a git repository
- **Status:** Alpha (v0.1.0)
- **Purpose:** **USIR for UI domain** - semantic-first UI infrastructure
- **Architecture:** SCM (Semantic Component Model) → Multi-framework compiler → React/Vue/Svelte/HTML
- **Position:** Perfect example of domain-specific USIR implementation

**TIA Workspace:** `/home/scottsen/src/projects/tia-projects/sup/` (embedded in tia-projects)

**Key Features:**
- Zero-runtime compile-time transformations
- AI-native (structured APIs for LLMs)
- Multi-framework compilation (one semantic source → many frameworks)
- Token engine, behavior engine, accessibility validation

**Next Steps:** Initialize git repo when architecture stabilizes, publish to GitHub

#### 9. Reveal (Progressive Disclosure / Semantic Code Explorer)
**Official Repo:** `/home/scottsen/src/projects/reveal/`
- **Git:** ✅ GitHub (published to PyPI as `reveal-cli`)
- **Status:** Production (v0.9.0)
- **Purpose:** Semantic code exploration with progressive disclosure
- **Architecture:** Tree → Structure → Element extraction
- **Position:** USIR browsing for code - 10x token efficiency

**Key Features:**
- 18+ file types (Python, JS, TS, Rust, Go, GDScript, Bash, Jupyter, Markdown, YAML, JSON, etc)
- URI adapters (`env://`, future: `https://`, `git://`, `docker://`)
- Universal `filename:line` format (vim/git/grep compatible)
- AI-optimized (structure before detail)

**Next Steps:** Expand URI adapter ecosystem, integrate with Pantheon IR browsing

#### 10. BrowserBridge (Event-Driven Browser Automation)
**Official Repo:** `/home/scottsen/src/projects/browserbridge/`
- **Git:** ✅ Local git repository
- **Status:** In Development (Phase 1: Foundation)
- **Purpose:** Semantic browser state extraction + human-AI collaboration
- **Architecture:** CloudEvents + AsyncAPI + WebSocket + JSON-RPC
- **Position:** Event-driven semantic integration layer for browser

**Key Features:**
- Real-time browser event streaming (CloudEvents standard)
- Protocol-agnostic adapters (MCP, LangChain, REST)
- Workflow recording and replay
- Human-AI shared context (not just AI control)

**Next Steps:** 10-week buildout (browser extension → server → workflows → adapters → launch)

**Related:** tia-browser-reveal (production-ready validation)

#### 11. tia-browser-reveal (Browser Extension)
**Official Repo:** `/home/scottsen/src/projects/tia-browser-reveal/`
- **Git:** ❌ Not yet a git repository
- **Status:** Production-ready (694 lines, 8/8 tests passing)
- **Purpose:** Minimal browser extension validating BrowserBridge concepts
- **Architecture:** Firefox/Chrome extension → native messaging → TIA commands
- **Position:** **Validates** semantic browser extraction pattern

**Key Features:**
- Extract ChatGPT conversations and page structure
- Simple workflow: click → save → reveal
- `tia browser reveal` command integration
- Real working code proving browser + CLI integration works

**Relationship to BrowserBridge:**
- Browser-Reveal = "curl for browsers" (quick extraction, ships now)
- BrowserBridge = "Playwright for AI agents" (comprehensive platform, 10-week timeline)

**Next Steps:** Publish when BrowserBridge reaches maturity, or release independently as lightweight tool

---

## The Meta-System: TIA

**TIA Repository:** `/home/scottsen/src/tia/`
- **Git:** `scottsen@aegir.whatbox.ca:git/tia.git` (private Whatbox repo)
- **Purpose:** The meta-system that manages all projects, research, and sessions
- **Structure:**
  ```
  tia/
  ├── bin/            # TIA executables (tia, tia-boot, tia-save, etc.)
  ├── commands/       # 44 command domains
  ├── lib/            # Core libraries (search, beth, semantic, ai)
  ├── docs/           # TIA documentation (~70 subdirs)
  ├── sessions/       # Session archives (~1900 sessions)
  ├── projects/       # TIA project workspaces (Tier 2)
  ├── config/         # System configuration
  └── templates/      # Templates (CLAUDE.md, etc.)
  ```

**Key Point:** TIA is the orchestration layer that connects all projects but is NOT part of the SIL ecosystem itself.

---

## Project Lifecycle: Planning → Production

### Three TIA Tracking Patterns

**Note:** All spirit-aligned projects (semantic-first, provenance, progressive disclosure, deterministic, verifiable) are part of the SIL GitHub organization. The patterns below describe how TIA tracks them internally, not whether they're "in" SIL.

#### Pattern A: Full TIA Workspace
**Examples:** SIL, Pantheon, Morphogen, Set Stack/Prism

Projects with active research and planning happening in TIA:
- **Production code:** `/home/scottsen/src/projects/<name>/` (clean, user-facing docs only)
- **Research workspace:** `/home/scottsen/src/tia/projects/<name>/` (session notes, analysis, planning)
- **Clear separation:** `.gitignore` prevents session artifacts from entering public repo
- **Full lifecycle tracking:** From conception through production

**Stages:**
1. **Planning:** TIA workspace only (Set Stack/Prism)
2. **Development:** Both tiers, local git (Pantheon)
3. **Published:** Both tiers, public GitHub (SIL, Morphogen)

**Use when:** Active research, architectural exploration, or multi-session planning needed

#### Pattern B: Lightweight TIA Tracking
**Examples:** TiaCAD, GenesisGraph, Reveal

Mature, stable projects needing minimal TIA oversight:
- **Production code:** `/home/scottsen/src/projects/<name>/` (complete, self-documenting)
- **TIA tracking:** `.tia/projects/<name>.yaml` (metadata only - no full workspace)
- **Minimal workspace:** Only created when active work sessions occur
- **Still part of SIL org:** Published to `semantic-infrastructure-lab/` GitHub

**Characteristics:**
- Production-ready with comprehensive docs (TiaCAD v3.1.1, Reveal v0.9.0, GenesisGraph v0.3.0)
- Infrequent major changes
- Self-contained (can be used independently of other SIL projects)
- TIA tracks metadata but doesn't need ongoing research workspace

**Use when:** Project is mature and stable, changes are incremental

#### Pattern C: In-Development (No Git Yet)
**Examples:** Riffstack, SUP, BrowserBridge

Projects under active development, not yet published:
- **Code location:** `/home/scottsen/src/projects/<name>/`
- **Status:** Local development, no git repo yet
- **Future path:** Will become Pattern A or B once published

**Use when:** Early development, architecture still evolving

---

## Critical Rules & Best Practices

### 1. **Never Mix Tiers**
```bash
# ❌ WRONG - TIA artifacts in production repo
/home/scottsen/src/projects/SIL/.beth/
/home/scottsen/src/projects/SIL/analysis/

# ✅ CORRECT - TIA artifacts in workspace
/home/scottsen/src/tia/projects/SIL/analysis/
```

### 2. **Production Repo .gitignore**
Every production SIL repo should ignore TIA session artifacts:
```gitignore
# TIA Session Artifacts (belong in ~/src/tia/projects/<name>/)
*_SUMMARY.md
*_COMPLETE.md
*_ANALYSIS.md
*_ASSESSMENT_*.md
*_PLAN.md
*_PROGRESS.md
*_REPORT.md
*_STATUS_REPORT.md
*_IMPLEMENTATION_*.md
NEXT_STEPS.md
STATUS.md
project.yaml

# TIA directories
.tia/
.beth/
analysis/
research/
sessions/
internal/
```

**Reference:** See `/home/scottsen/src/tia/sessions/mojare-1127/SIL_STANDARD_GITIGNORE.txt` for complete template

### 3. **Cross-Referencing**
When documenting in TIA workspace, reference official repo:
```yaml
# In project.yaml
paths:
  root: "/home/scottsen/src/tia/projects/SIL"  # TIA workspace
  official_repo: "/home/scottsen/src/projects/SIL"  # Production code
```

### 4. **Documentation Homes**
- **Architecture docs:** Production repo (`/home/scottsen/src/projects/<name>/docs/`)
- **Analysis docs:** TIA workspace (`/home/scottsen/src/tia/projects/<name>/`)
- **This layout guide:** TIA workspace (canonical reference)

---

## Quick Reference Matrix

| Project | Type | Official Repo | Git Status | TIA Workspace | Status |
|---------|------|--------------|------------|---------------|--------|
| **SIL** | Infrastructure | `/src/projects/SIL` | ✅ GitHub | `/tia/projects/SIL` | Published |
| **Pantheon** | IR Core | `/src/projects/pantheon` | ❌ Local | `/tia/projects/pantheon` | Development |
| **Morphogen** | Audio DSL | `/src/projects/morphogen` | ✅ GitHub | `/tia/projects/Morphogen` | Published |
| **Set Stack/Prism** | Query DSL | N/A | ❌ Planning | `/tia/projects/Set Stack` | Specification |
| **TiaCAD** | CAD DSL | `/src/projects/tiacad` | ✅ GitHub (v3.1.1) | Metadata only | **Production** |
| **GenesisGraph** | Provenance | `/src/projects/genesisgraph` | ✅ Local git (no remote) | Metadata only | v0.1 Draft |
| **Riffstack** | Audio MLIR | `/src/projects/riffstack` | ❌ Local | N/A | MVP (v0.1) |
| **SUP** | UI IR | `/src/tia-projects/sup` | ❌ Local | Embedded | Alpha (v0.1.0) |
| **Reveal** | Code Explorer | `/src/projects/reveal` | ✅ GitHub + PyPI | N/A | **Production** (v0.9.0) |
| **BrowserBridge** | Browser IR | `/src/projects/browserbridge` | ✅ Local git | N/A | Development |
| **tia-browser-reveal** | Browser Ext | `/src/projects/tia-browser-reveal` | ❌ Local | N/A | Production-ready |
| **TIA** | Meta-System | `/src/tia` | ✅ Whatbox | N/A | Production |

---

## Navigation Commands

### Explore Project Locations
```bash
# List all projects
tia project list

# See official repos
ls -1d ~/src/projects/*/

# See TIA workspaces
ls -1d ~/src/tia/projects/*/

# Check git status of official repos
cd ~/src/projects && for d in */; do
  echo "=== $d ==="
  cd "$d" && git remote -v 2>/dev/null || echo "Not a git repo"
  cd ..
done
```

### Find SIL Documentation
```bash
# Search across all SIL-related docs
tia beth explore "SIL ecosystem"
tia search all "pantheon architecture"

# Read official docs
tia read ~/src/projects/SIL/README.md
tia read ~/src/projects/pantheon/docs/UNIFIED_ARCHITECTURE.md

# Read TIA workspace research
tia read ~/src/tia/projects/SIL/SIL_PREPARATION_INDEX.md
```

---

## Common Confusion Points Resolved

### Q: Why are there two Morphogen directories?
**A:** `/src/projects/morphogen` is the production code (GitHub repo), `/src/tia/projects/Morphogen` is research notes and project metadata. They serve different purposes. This is **Pattern A** (Two-Tier).

### Q: Why doesn't TiaCAD have a TIA workspace directory?
**A:** TiaCAD is **Pattern B** (Lightweight TIA Tracking). It's a mature, stable project (v3.1.1, 1027 tests) that doesn't need ongoing research sessions. TIA tracks it with lightweight metadata in `.tia/projects/tiacad.yaml`. Full workspace only created when active work sessions occur.

### Q: Where should I commit new code?
**A:**
- **Pattern A projects (SIL, Pantheon, Morphogen):** Production code → `/src/projects/<name>/` (official repo), Research/analysis → `/src/tia/projects/<name>/` (TIA workspace)
- **Pattern B projects (TiaCAD, GenesisGraph):** All code → `/src/projects/<name>/` (no separate TIA workspace)

### Q: Why isn't Pantheon a git repo yet?
**A:** Still in active development. Will become a git repo and publish to GitHub when architecture stabilizes.

### Q: Where does Set Stack/Prism code go?
**A:** Currently specification-only in TIA workspace (Pattern A, Stage 1). When implementation begins, create `/src/projects/prism/` and initialize git repo.

### Q: What makes a project part of SIL?
**A:** Spirit-aligned projects that embody SIL principles:
- Semantic-first design (explicit meaning representations)
- Provenance-complete (traceable transformations)
- Progressive disclosure (structure before detail)
- Deterministic/verifiable (reproducible, testable)
- Supporting intelligent systems infrastructure

**All spirit-aligned projects go in `semantic-infrastructure-lab/` GitHub org**, regardless of when they were created or their maturity level.

### Q: What's the relationship between TIA and SIL?
**A:** TIA is the meta-system that manages research and development. SIL is one ecosystem being developed USING TIA. TIA ≠ SIL.

### Q: How do I know if a project should be Pattern A or Pattern B?
**A:**
- **Pattern A (Full TIA Workspace):** Active research/planning happening, architectural exploration, multi-session work
- **Pattern B (Lightweight Tracking):** Mature, stable project with infrequent changes, comprehensive self-documentation
- When in doubt, use Pattern A for new SIL work

**Note:** Both patterns are part of SIL org! The difference is TIA tracking overhead, not SIL membership.

---

## Integration with This Document

**Canonical Location:** `/home/scottsen/src/tia/projects/SIL/SIL_ECOSYSTEM_PROJECT_LAYOUT.md`

**Update Strategy:**
- Update this document when projects change status (local → git → GitHub)
- Update when new SIL projects are created
- Update when directory structures change
- Reference this from all SIL project README files

**References:**
- SIL project.yaml explicitly documents this separation (lines 21-34)
- See session violet-dawn-1126 for Set Stack/Prism integration analysis
- See session ancient-void-1123 for Pantheon architecture

---

## Version History

- **2025-11-27 (v1.0):** Initial creation - mapped complete SIL ecosystem layout
- **2025-11-27 (v1.1):** Corrected TiaCAD and GenesisGraph status
  - TiaCAD: Updated to show GitHub published (v3.1.1), production-ready
  - GenesisGraph: Updated to show v0.1 Working Draft status, has local git
  - Added "Three Organizational Patterns" section (Pattern A/B/C)
  - Updated Quick Reference Matrix with accurate git/workspace status
  - Expanded Common Confusion Points with Pattern explanations
- **2025-11-27 (v1.2):** Added 4 cross-project discoveries
  - **SUP** (Semantic UI Platform) - USIR for UI domain
  - **Reveal** (Progressive Disclosure) - Semantic code exploration (production, on PyPI)
  - **BrowserBridge** - Event-driven browser automation
  - **tia-browser-reveal** - Browser extension (production-ready validation)
  - Updated Quick Reference Matrix: 7 projects → 11 projects
  - Added detailed architecture and positioning for each new project

---

**Last Updated:** 2025-11-27 (v1.2)
**Maintained By:** TIA System
**Session:** warlike-vertex-1127
