# SIL Documentation Consolidation Plan

**Created:** 2025-11-30
**Session:** hidden-universe-1130
**Goal:** Merge overlapping docs into clear, useful, publication-ready documentation

---

## Problem Statement

SIL has **15 files in `docs/canonical/`** - too many for "canonical" status. Several overlap significantly:

- **3 architecture docs** covering the same 6-layer system
- **Operational/planning docs** mixed with foundational concepts
- **Project-specific docs** (Morphogen) in canonical directory
- **Internal planning docs** (team archetypes, lab design) marked "canonical"

**Result:** Confusion about which doc to read, redundancy, maintenance burden.

---

## Core Principle

**"Canonical" means:**
- Foundational and stable
- Referenced by other docs
- Essential for understanding SIL
- **8 docs maximum** (currently 15)

---

## Current State Analysis

### ✅ TRUE CANONICAL (Keep as-is) - 8 docs

These are genuinely foundational and non-redundant:

1. **FOUNDERS_LETTER.md** (30 lines) - Personal introduction, Tia collaboration
2. **FOUNDER_PROFILE.md** (22 lines) - Scott's background
3. **TIA_PROFILE.md** (76 lines) - Tia's role and boundaries
4. **SIL_MANIFESTO.md** (365 lines) - Why SIL exists, philosophical foundation
5. **SIL_TECHNICAL_CHARTER.md** (1,171 lines) - Formal specification
6. **SIL_GLOSSARY.md** (257 lines) - Canonical vocabulary
7. **SIL_PRINCIPLES.md** (122 lines) - The 14 principles
8. **SIL_RESEARCH_AGENDA_YEAR1.md** (571 lines) - Year 1 roadmap

**Total:** 8 files, 2,614 lines

---

### ⚠️ REDUNDANT ARCHITECTURE DOCS (Merge) - 2 docs

**Problem:** We have THREE architecture documents covering the 6-layer Semantic OS:

1. **SIL_SEMANTIC_OS_ARCHITECTURE.md** (653 lines, canonical/)
   - 6-layer architecture with different numbering (Layer 1-6)
   - Source: Claude conversation extraction
   - Status: Canonical

2. **UNIFIED_ARCHITECTURE_GUIDE.md** (architecture/)
   - The "Rosetta Stone" - canonical framework
   - Intent → IR → Execution pattern
   - Maps all projects to layers
   - Status: Definitive Reference

3. **SIL_MANIFESTO.md** Section 4 "The Semantic Operating System"
   - Also describes 6 layers (Layer 0-5)
   - Embedded in manifesto

**Conflicts:**
- **Layer numbering inconsistency:** SEMANTIC_OS uses 1-6, Manifesto uses 0-5
- **Different emphases:** SEMANTIC_OS is descriptive, UNIFIED is prescriptive
- **Duplicate information:** Same concepts, different presentations

**Decision:**
- **KEEP:** `architecture/UNIFIED_ARCHITECTURE_GUIDE.md` (the Rosetta Stone)
- **ARCHIVE:** `canonical/SIL_SEMANTIC_OS_ARCHITECTURE.md` → `archive/planning-drafts-2025-11/`
- **REASON:** UNIFIED is more comprehensive, maps projects, provides decision frameworks

---

### 🔒 INTERNAL/PLANNING DOCS (Move to internal/) - 4 docs

These are valuable but NOT canonical - they're operational/planning:

1. **SIL_FOUNDING_TEAM_ARCHETYPES.md** (514 lines)
   - Marion (Empiricist), Eric (Policy), Scott (Architect), Tia (Agent)
   - → Move to `internal/personnel/FOUNDING_TEAM_ARCHETYPES.md`

2. **SIL_PHYSICAL_LAB_DESIGN.md** (526 lines)
   - Physical space design, equipment, workshop layout
   - → Move to `internal/operations/PHYSICAL_LAB_DESIGN.md`

3. **SIL_TWO_DIVISION_STRUCTURE.md** (372 lines)
   - SIL-Core vs SIL-Ventures organizational structure
   - → Move to `internal/strategy/TWO_DIVISION_STRUCTURE.md`

4. **SIL_STEWARDSHIP_MANIFESTO.md** (536 lines)
   - Governance, decision-making, stewardship model
   - → Move to `internal/strategy/STEWARDSHIP_MANIFESTO.md`

**Reasoning:** These are important for team/operations, but mixing them with foundational concepts dilutes "canonical."

---

### 📦 PROJECT-SPECIFIC DOCS (Move to project/) - 1 doc

1. **SIL_MORPHOGEN_PROJECT.md** (705 lines)
   - Deep dive on Morphogen architecture
   - → Move to Morphogen project repository
   - OR create `docs/projects/MORPHOGEN_DEEP_DIVE.md`

**Reasoning:** Project-specific details don't belong in canonical docs.

---

### 🤔 UNCLEAR STATUS (Review) - 1 doc

1. **SIL_CIVILIZATIONAL_SYSTEMS_ENGINEERING.md** (668 lines)
   - Systems engineering at civilization scale
   - Reads like vision/philosophy
   - **Option A:** Move to `docs/vision/CIVILIZATIONAL_SYSTEMS_ENGINEERING.md`
   - **Option B:** Archive as draft
   - **Decision needed:** Is this core to SIL identity or aspirational?

---

## Proposed Actions (Prioritized)

### Phase 1: Quick Wins (5 minutes)

**Move internal docs:**
```bash
# Create target directories if needed
mkdir -p internal/personnel internal/operations

# Move files
git mv docs/canonical/SIL_FOUNDING_TEAM_ARCHETYPES.md internal/personnel/
git mv docs/canonical/SIL_PHYSICAL_LAB_DESIGN.md internal/operations/
git mv docs/canonical/SIL_TWO_DIVISION_STRUCTURE.md internal/strategy/
git mv docs/canonical/SIL_STEWARDSHIP_MANIFESTO.md internal/strategy/
```

**Result:** `docs/canonical/` drops from 15 → 11 files

---

### Phase 2: Archive Redundant Architecture Doc (2 minutes)

```bash
# Move to archive
git mv docs/canonical/SIL_SEMANTIC_OS_ARCHITECTURE.md archive/planning-drafts-2025-11/

# Update references (if any exist)
grep -r "SIL_SEMANTIC_OS_ARCHITECTURE" docs/ README.md
```

**Result:** `docs/canonical/` drops from 11 → 10 files

---

### Phase 3: Handle Morphogen Doc (5 minutes)

**Option A:** Move to morphogen repo
**Option B:** Move to `docs/projects/MORPHOGEN_DEEP_DIVE.md`

**Recommendation:** Option B (keep in SIL repo but out of canonical)

```bash
mkdir -p docs/projects
git mv docs/canonical/SIL_MORPHOGEN_PROJECT.md docs/projects/MORPHOGEN_DEEP_DIVE.md
```

**Result:** `docs/canonical/` drops from 10 → 9 files

---

### Phase 4: Handle Civilizational Systems Engineering (3 minutes)

**Recommendation:** Move to vision/

```bash
git mv docs/canonical/SIL_CIVILIZATIONAL_SYSTEMS_ENGINEERING.md docs/vision/
```

**Result:** `docs/canonical/` drops from 9 → 8 files ✅

---

### Phase 5: Update Documentation (15 minutes)

**Files to update:**

1. **README.md**
   - Check for references to moved docs
   - Update if needed

2. **docs/READING_GUIDE.md**
   - Update canonical docs list (should show only 8)
   - Add references to new locations
   - Update reading paths

3. **Other docs**
   - Search for cross-references to moved docs
   - Update paths

```bash
# Find all references
grep -r "SIL_SEMANTIC_OS_ARCHITECTURE\|FOUNDING_TEAM_ARCHETYPES\|PHYSICAL_LAB_DESIGN\|TWO_DIVISION_STRUCTURE\|STEWARDSHIP_MANIFESTO\|MORPHOGEN_PROJECT\|CIVILIZATIONAL_SYSTEMS" docs/ README.md
```

---

## After State

### `docs/canonical/` (8 files - THE CORE)

```
docs/canonical/
├── FOUNDERS_LETTER.md           # Personal introduction
├── FOUNDER_PROFILE.md           # Scott's background
├── TIA_PROFILE.md               # Tia's role
├── SIL_MANIFESTO.md             # Why SIL exists
├── SIL_TECHNICAL_CHARTER.md     # Formal specification
├── SIL_GLOSSARY.md              # Vocabulary
├── SIL_PRINCIPLES.md            # 14 principles
└── SIL_RESEARCH_AGENDA_YEAR1.md # Roadmap
```

**Clear purpose:** These 8 docs define what SIL is, why it exists, and what it's building.

---

### Other Documentation (Organized by Purpose)

```
docs/
├── architecture/
│   ├── UNIFIED_ARCHITECTURE_GUIDE.md  ⭐ THE ROSETTA STONE
│   └── DESIGN_PRINCIPLES.md
│
├── vision/
│   ├── SIL_VISION_COMPLETE.md
│   └── CIVILIZATIONAL_SYSTEMS_ENGINEERING.md  ← MOVED HERE
│
├── projects/
│   └── MORPHOGEN_DEEP_DIVE.md  ← MOVED HERE
│
├── guides/
│   ├── OPTIMIZATION_IN_SIL.md
│   └── SIL_ECOSYSTEM_PROJECT_LAYOUT.md
│
├── operations/
│   └── DEPLOYMENT_STANDARDS.md
│
└── meta/
    ├── CONSOLIDATION_SUMMARY.md
    ├── DEDICATION.md
    └── FOUNDER_BACKGROUND.md

internal/  (gitignored)
├── personnel/
│   └── FOUNDING_TEAM_ARCHETYPES.md  ← MOVED HERE
│
├── operations/
│   └── PHYSICAL_LAB_DESIGN.md  ← MOVED HERE
│
└── strategy/
    ├── TWO_DIVISION_STRUCTURE.md  ← MOVED HERE
    └── STEWARDSHIP_MANIFESTO.md   ← MOVED HERE

archive/planning-drafts-2025-11/
└── SIL_SEMANTIC_OS_ARCHITECTURE.md  ← ARCHIVED
```

---

## Benefits

1. **Clarity:** "Canonical" means what it says - 8 core docs
2. **Findability:** Docs organized by purpose (vision/ projects/ operations/)
3. **Maintainability:** Less redundancy, clearer ownership
4. **Professionalism:** Clean structure for public GitHub repo
5. **Separation:** Public/private boundary enforced (internal/ gitignored)

---

## Risk Assessment

**Low risk because:**
- No content is deleted (only moved)
- Git history preserved
- Can revert if needed
- Most moved docs aren't referenced externally yet (pre-publication)

**Validation steps:**
1. Run `grep -r` to find all references
2. Update READING_GUIDE.md
3. Update README.md
4. Test all links in documentation
5. Commit with clear message

---

## Execution Checklist

- [ ] Phase 1: Move internal docs (4 files)
- [ ] Phase 2: Archive redundant architecture doc (1 file)
- [ ] Phase 3: Move Morphogen doc to projects/ (1 file)
- [ ] Phase 4: Move Civilizational doc to vision/ (1 file)
- [ ] Phase 5: Update READING_GUIDE.md
- [ ] Phase 6: Update README.md
- [ ] Phase 7: Search for and update cross-references
- [ ] Phase 8: Test all documentation links
- [ ] Phase 9: Git commit with message
- [ ] Phase 10: Update this session's README

---

## Git Commit Message (Draft)

```
docs: consolidate canonical directory from 15 to 8 core documents

Reorganize documentation for clarity and maintainability:

Moved to internal/ (operational/planning docs):
- FOUNDING_TEAM_ARCHETYPES.md → internal/personnel/
- PHYSICAL_LAB_DESIGN.md → internal/operations/
- TWO_DIVISION_STRUCTURE.md → internal/strategy/
- STEWARDSHIP_MANIFESTO.md → internal/strategy/

Moved to specialized directories:
- SIL_MORPHOGEN_PROJECT.md → docs/projects/MORPHOGEN_DEEP_DIVE.md
- SIL_CIVILIZATIONAL_SYSTEMS_ENGINEERING.md → docs/vision/

Archived (redundant with UNIFIED_ARCHITECTURE_GUIDE):
- SIL_SEMANTIC_OS_ARCHITECTURE.md → archive/planning-drafts-2025-11/

Updated navigation:
- docs/READING_GUIDE.md reflects new structure
- README.md updated with new paths

Result: docs/canonical/ now contains exactly 8 foundational documents:
- Founder's Letter, profiles, Manifesto, Technical Charter,
  Glossary, Principles, Research Agenda

This separation clarifies what is "canonical" (foundational concepts)
vs operational (team/lab planning) vs specialized (project deep-dives).
```

---

**Next Step:** Execute Phase 1-4 (file moves), then update documentation.
