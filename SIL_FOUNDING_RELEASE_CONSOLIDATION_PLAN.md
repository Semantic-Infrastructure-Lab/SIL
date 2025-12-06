# SIL Founding Release - Documentation Consolidation Plan

**Created:** 2025-12-05
**Status:** Ready for Execution
**Goal:** Prepare clean, coherent documentation for SIL's founding public release

---

## Executive Summary

**Current State:**
- Duplicate files scattered across `docs/` and `docs/meta/`
- Outdated project counts (README says 11, reality is 13)
- Mix of public-facing, internal, and draft content without clear separation
- Two versions of PROGRESSIVE_DISCLOSURE.md serving different purposes

**Target State:**
- Clean canonical documentation for founding release
- All internal/planning docs isolated in `internal/` (not for website)
- Consistent project count (13) across all documents
- Clear hierarchy: canonical → meta → innovations → research → architecture

---

## 1. Actual Project Count Analysis

**Reality Check:**
```bash
$ grep "^###" /home/scottsen/src/projects/SIL/projects/PROJECT_INDEX.md | wc -l
13
```

**The 13 Projects:**
1. Morphogen (Production)
2. Philbrick (Research)
3. TiaCAD (Production)
4. GenesisGraph (Production)
5. Reveal (Production)
6. RiffStack (MVP/Active)
7. Sup (Alpha/Active)
8. BrowserBridge (Alpha/Active)
9. Pantheon (Research)
10. Agent-Ether (Specification)
11. Prism (Specification)
12. Semantic-Memory (Planned)
13. SIL (Meta-project/Documentation)

**Production Count:**
- Per PROJECT_INDEX: 5 (morphogen, tiacad, genesisgraph, reveal, SIL)
- Per README badges: Says 5 ✅ (correct)
- Reality: **5 production, 13 total projects**

**Issue:** README, FAQ, QUICKSTART say "11 projects" - should be **13 projects**

---

## 2. Duplicate Files Analysis & Resolution

### 2.1 FAQ.md (IDENTICAL - Safe to Delete)

**Locations:**
- `/home/scottsen/src/projects/SIL/docs/FAQ.md`
- `/home/scottsen/src/projects/SIL/docs/meta/FAQ.md`

**Checksums:**
```
237935020cef6f82d650f42b3f96e0cc  docs/FAQ.md
237935020cef6f82d650f42b3f96e0cc  docs/meta/FAQ.md
```

**Decision:** ✅ Keep `docs/meta/FAQ.md`, delete `docs/FAQ.md`
- Reason: Meta docs belong in `docs/meta/` for organizational clarity
- Both files say "12 projects" - need to update to 13

**Action:**
```bash
git rm docs/FAQ.md
# Update docs/meta/FAQ.md: "12 projects" → "13 projects"
```

---

### 2.2 QUICKSTART.md (DIFFERENT - Top-level OUTDATED)

**Locations:**
- `/home/scottsen/src/projects/SIL/docs/QUICKSTART.md` (OUTDATED)
- `/home/scottsen/src/projects/SIL/docs/meta/QUICKSTART.md` (CORRECT)

**Differences:**
- Top-level says "**11 projects**, 5 in production"
- Meta version says "**12 projects**, 4 in production"
- Reality: **13 projects, 5 in production**

**Decision:** ✅ Keep `docs/meta/QUICKSTART.md`, delete `docs/QUICKSTART.md`
- Update meta version: "12 projects, 4 in production" → "13 projects, 5 in production"

**Action:**
```bash
git rm docs/QUICKSTART.md
# Update docs/meta/QUICKSTART.md:
#   - "12 projects" → "13 projects"
#   - "4 in production" → "5 in production"
```

---

### 2.3 READING_GUIDE.md (DIFFERENT - Top-level OUTDATED)

**Locations:**
- `/home/scottsen/src/projects/SIL/docs/READING_GUIDE.md` (OUTDATED)
- `/home/scottsen/src/projects/SIL/docs/meta/READING_GUIDE.md` (MORE RECENT)

**Differences:**
- Top-level mentions "11 projects"
- Meta version mentions "12 projects"
- Reality: **13 projects**

**Decision:** ✅ Keep `docs/meta/READING_GUIDE.md`, delete `docs/READING_GUIDE.md`
- Update meta version: "12 projects" → "13 projects"

**Action:**
```bash
git rm docs/READING_GUIDE.md
# Update docs/meta/READING_GUIDE.md: "12 projects" → "13 projects"
```

---

### 2.4 PROGRESSIVE_DISCLOSURE.md (DIFFERENT CONTENT - Keep Both)

**Locations:**
- `/home/scottsen/src/projects/SIL/docs/PROGRESSIVE_DISCLOSURE.md` (912 lines)
- `/home/scottsen/src/projects/SIL/docs/innovations/PROGRESSIVE_DISCLOSURE.md` (339 lines)

**Content Comparison:**

**docs/PROGRESSIVE_DISCLOSURE.md:**
- Comprehensive guide to Progressive Disclosure as SIL's #1 principle
- 3 levels: Orient → Navigate → Focus
- Implementation patterns across reveal, Beth, tia search
- Design patterns, anti-patterns, measurement
- 912 lines of detailed guidance

**docs/innovations/PROGRESSIVE_DISCLOSURE.md:**
- Innovation showcase format (economic impact focus)
- 86% token reduction metrics
- Production metrics (v0.16.0)
- Economic proof: $47K/year per 100 agents
- Status/adoption section

**Decision:** ✅ **Keep BOTH - they serve different purposes**

**Rename for Clarity:**
```bash
# Top-level comprehensive guide → principles documentation
git mv docs/PROGRESSIVE_DISCLOSURE.md docs/canonical/PROGRESSIVE_DISCLOSURE_GUIDE.md

# innovations/ version stays as innovation showcase
# (docs/innovations/PROGRESSIVE_DISCLOSURE.md remains unchanged)
```

**Reasoning:**
- `/docs/canonical/PROGRESSIVE_DISCLOSURE_GUIDE.md` = Design principle, implementation guide
- `/docs/innovations/PROGRESSIVE_DISCLOSURE.md` = Innovation showcase with metrics/economics
- Different audiences: architects vs. stakeholders/investors

---

## 3. Directory Classification

### 3.1 FOUNDING RELEASE (Public-Facing)

**docs/canonical/** - Core philosophy & principles
- ✅ FOUNDERS_LETTER.md
- ✅ FOUNDER_PROFILE.md
- ✅ SIL_MANIFESTO.md
- ✅ SIL_PRINCIPLES.md
- ✅ SIL_GLOSSARY.md
- ✅ SIL_TECHNICAL_CHARTER.md
- ✅ SIL_STEWARDSHIP_MANIFESTO.md
- ✅ SIL_RESEARCH_AGENDA_YEAR1.md
- ✅ PROGRESSIVE_DISCLOSURE_GUIDE.md (renamed from top-level)
- ✅ HIERARCHICAL_AGENCY_FRAMEWORK.md
- ✅ MULTI_AGENT_PROTOCOL_PRINCIPLES.md
- ✅ SEMANTIC_FEEDBACK_LOOPS.md
- ✅ SEMANTIC_OBSERVABILITY.md
- ✅ FOUNDERS_NOTE_MULTISHOT_AGENT_LEARNING.md

**docs/meta/** - Getting started docs
- ✅ FAQ.md
- ✅ QUICKSTART.md
- ✅ READING_GUIDE.md
- ✅ DEDICATION.md

**docs/innovations/** - Project showcases
- ✅ INNOVATIONS.md (index)
- ✅ MORPHOGEN.md
- ✅ GENESISGRAPH.md
- ✅ PANTHEON.md
- ✅ AGENT_ETHER.md
- ✅ PROGRESSIVE_DISCLOSURE.md (innovation showcase)

**docs/research/** - Research papers
- ✅ RAG_AS_SEMANTIC_MANIFOLD_TRANSPORT.md
- ✅ AGENT_HELP_STANDARD.md
- ✅ AI_DOCUMENTATION_STANDARDS.md
- ✅ IDENTITY_MAPPING.md

**docs/architecture/** - Architecture guides
- ✅ UNIFIED_ARCHITECTURE_GUIDE.md
- ✅ README.md

**docs/tools/** - Tool documentation
- ✅ REVEAL.md

---

### 3.2 INTERNAL ONLY (NOT for Public Release/Website)

**internal/strategy/** - Strategic planning
- 🔒 COMMUNICATION_TOOLKIT.md
- 🔒 FOUNDER_LETTERS_ROADMAP.md
- 🔒 FOUNDER_ROLE_AND_POLICY.md
- 🔒 INVESTOR_EVALUATION_FRAMEWORK.md
- 🔒 PITCH_FRAMEWORKS.md
- 🔒 REVEAL_AS_SIL_GATEWAY.md
- 🔒 RISK_MITIGATION_STRATEGY.md
- 🔒 SIL_FOUNDING_TEAM_ARCHETYPES.md
- 🔒 SIL_TWO_DIVISION_STRUCTURE.md
- 🔒 STRATEGIC_ROADMAP.md
- 🔒 TEAM_COMPOSITION_STRATEGY.md

**internal/pitch/** - Pitch materials
- 🔒 DOSSIER.md
- 🔒 LEGITIMACY.md
- 🔒 OVERVIEW.md
- 🔒 PHILOSOPHY.md
- 🔒 THE_FORK.md
- 🔒 TOOLS_CATALOG.md
- 🔒 VISION_HOPE.md
- 🔒 VISION_REALITY.md

**internal/personnel/** - Team profiles
- 🔒 eric_shoup.md
- 🔒 FOUNDER_BACKGROUND.md
- 🔒 founder_profile.md
- 🔒 marion_groh_marquardt.md
- 🔒 mike_abbott.md
- 🔒 patrick_collison.md
- 🔒 TIA_PROFILE.md

**internal/operations/**
- 🔒 DEPLOYMENT_STANDARDS.md

**internal/planning/**
- 🔒 DOC_CONSOLIDATION_PLAN.md (this plan's predecessor)
- 🔒 CONSOLIDATION_SUMMARY.md
- 🔒 SIL_PHYSICAL_LAB_DESIGN.md

---

### 3.3 STAGING (Draft/WIP - NOT for Public Release)

**staging/canonical/**
- 📝 SIL_CIVILIZATIONAL_SYSTEMS_ENGINEERING.md
- 📝 SIL_MORPHOGEN_PROJECT.md
- 📝 SIL_RESEARCH_AGENDA_YEAR1.md (duplicate)
- 📝 SIL_TECHNICAL_CHARTER.md (duplicate)

**staging/architecture/**
- 📝 DESIGN_PRINCIPLES.md

**staging/guides/**
- 📝 OPTIMIZATION_IN_SIL.md
- 📝 SIL_ECOSYSTEM_PROJECT_LAYOUT.md

**staging/semantic-os/**
- 📝 layer-0 through layer-5 (7 files)
- 📝 README.md

**staging/vision/**
- 📝 REVEAL_SIL_INTEGRATION.md
- 📝 SIL_VISION_COMPLETE.md

**Decision:** ⚠️ Staging stays as-is (drafts not ready for release)

---

### 3.4 Files in Wrong Location

**docs/SIL_DESIGN_PRINCIPLES.md**
- Location: `/docs/SIL_DESIGN_PRINCIPLES.md` (top-level)
- Should be: `docs/canonical/` or `docs/architecture/`
- **Action:** Move to `docs/architecture/DESIGN_PRINCIPLES.md` (if different from canonical/SIL_PRINCIPLES.md)

**docs/SIL_SAFETY_THRESHOLDS.md**
- Location: `/docs/SIL_SAFETY_THRESHOLDS.md` (top-level)
- Should be: `docs/canonical/` or `internal/strategy/`
- **Action:** Review content, move to appropriate location

---

## 4. Project Count Updates Required

**Files to update (11 projects → 13 projects):**

1. **README.md** (line 5, 38, 77, 132, 300)
   - Badge: `[![Projects](https://img.shields.io/badge/Projects-11-blue)]()`
   - Links: "See all 11 projects"
   - Text: "Total Projects: 11"
   - Change to: **13**

2. **docs/meta/FAQ.md**
   - Current: "12 projects, 4 in production"
   - Change to: "**13 projects, 5 in production**"

3. **docs/meta/QUICKSTART.md**
   - Current: "12 projects, 4 in production"
   - Change to: "**13 projects, 5 in production**"
   - Multiple references throughout

4. **docs/meta/READING_GUIDE.md**
   - Current: "12 projects"
   - Change to: "**13 projects**"

5. **docs/innovations/INNOVATIONS.md**
   - Current: "12 projects across 7 layers"
   - Change to: "**13 projects across 7 layers**"

6. **docs/canonical/SIL_MANIFESTO.md**
   - Current: "12 projects"
   - Change to: "**13 projects**"

7. **docs/canonical/FOUNDERS_LETTER.md**
   - Check for project count references

8. **docs/canonical/FOUNDER_PROFILE.md**
   - Check for project count references

---

## 5. Execution Plan

### Phase 1: Remove Duplicates (2 minutes)

```bash
cd /home/scottsen/src/projects/SIL

# Remove outdated duplicates
git rm docs/FAQ.md
git rm docs/QUICKSTART.md
git rm docs/READING_GUIDE.md
```

**Validates:**
- Checksums confirmed identical/outdated
- Meta versions are source of truth

---

### Phase 2: Reorganize PROGRESSIVE_DISCLOSURE (1 minute)

```bash
# Move comprehensive guide to canonical
git mv docs/PROGRESSIVE_DISCLOSURE.md docs/canonical/PROGRESSIVE_DISCLOSURE_GUIDE.md

# innovations/PROGRESSIVE_DISCLOSURE.md stays (innovation showcase)
```

**Validates:**
- Two different purposes, both valuable
- Clear naming distinguishes them

---

### Phase 3: Move Misplaced Files (2 minutes)

```bash
# Move design principles to architecture
git mv docs/SIL_DESIGN_PRINCIPLES.md docs/architecture/DESIGN_PRINCIPLES.md

# Review and categorize safety thresholds
# (Decision needed: canonical vs internal/strategy?)
```

---

### Phase 4: Update Project Counts (15 minutes)

**Search and replace across 8 files:**

```bash
# Find all references
grep -rn "11 projects\|12 projects" docs/ README.md

# Update manually (careful with context):
# - 11 projects → 13 projects
# - 12 projects → 13 projects
# - 5 in production (already correct)
```

**Files to edit:**
1. README.md (badge + multiple references)
2. docs/meta/FAQ.md
3. docs/meta/QUICKSTART.md
4. docs/meta/READING_GUIDE.md
5. docs/innovations/INNOVATIONS.md
6. docs/canonical/SIL_MANIFESTO.md
7. docs/canonical/FOUNDERS_LETTER.md (check)
8. docs/canonical/FOUNDER_PROFILE.md (check)

---

### Phase 5: Verify Internal Separation (5 minutes)

**Ensure no internal/ docs leak to public:**

```bash
# Check website navigation files (if they exist)
grep -r "internal/" docs/ *.md *.yaml *.yml 2>/dev/null

# Verify .gitignore excludes internal/ from public repos
cat .gitignore | grep internal
```

**If internal/ not in .gitignore:**
```bash
echo "internal/" >> .gitignore
```

---

### Phase 6: Update Navigation & Links (10 minutes)

**Files likely needing link updates:**

1. **README.md**
   - Check all doc links work
   - Remove references to deleted duplicates

2. **docs/meta/READING_GUIDE.md**
   - Update paths to PROGRESSIVE_DISCLOSURE_GUIDE
   - Verify canonical docs list

3. **docs/canonical/*.md**
   - Search for broken cross-references

```bash
# Find all markdown links
grep -rn "\[.*\](.*\.md)" docs/ README.md | grep -E "FAQ.md|QUICKSTART.md|READING_GUIDE.md|PROGRESSIVE_DISCLOSURE.md"

# Update paths as needed
```

---

### Phase 7: Document Review Checklist

**Manual verification:**

- [ ] README.md: Badge shows 13 projects
- [ ] README.md: Quick Start table correct
- [ ] README.md: All doc links work
- [ ] docs/meta/FAQ.md: 13 projects, 5 production
- [ ] docs/meta/QUICKSTART.md: 13 projects, 5 production
- [ ] docs/meta/READING_GUIDE.md: 13 projects
- [ ] docs/innovations/INNOVATIONS.md: 13 projects
- [ ] docs/canonical/SIL_MANIFESTO.md: 13 projects
- [ ] No broken links in canonical docs
- [ ] PROGRESSIVE_DISCLOSURE_GUIDE vs PROGRESSIVE_DISCLOSURE clear distinction

---

### Phase 8: Commit & Document (5 minutes)

```bash
git add -A
git commit -m "$(cat <<'EOF'
docs: consolidate for founding release - 13 projects, remove duplicates

Documentation cleanup for SIL founding public release:

**Removed Duplicates:**
- docs/FAQ.md (identical to meta version)
- docs/QUICKSTART.md (outdated, meta version correct)
- docs/READING_GUIDE.md (outdated, meta version correct)

**Reorganized:**
- docs/PROGRESSIVE_DISCLOSURE.md → docs/canonical/PROGRESSIVE_DISCLOSURE_GUIDE.md
  (Comprehensive design guide, 912 lines)
- docs/innovations/PROGRESSIVE_DISCLOSURE.md remains
  (Innovation showcase with metrics, 339 lines)
- docs/SIL_DESIGN_PRINCIPLES.md → docs/architecture/DESIGN_PRINCIPLES.md

**Updated Project Counts:**
- 11 projects → 13 projects (8 files updated)
- Reflects actual count: 5 production + 8 others = 13 total
- Production count: 5 (morphogen, tiacad, genesisgraph, reveal, SIL)

**Structure:**
- docs/canonical/ - Core philosophy (14 docs)
- docs/meta/ - Getting started (4 docs)
- docs/innovations/ - Project showcases (6 docs)
- docs/research/ - Research papers (4 docs)
- docs/architecture/ - Architecture guides (3 docs)
- docs/tools/ - Tool docs (1 doc)
- internal/ - NOT for public release (strategy, pitch, personnel)
- staging/ - Drafts not ready for release

**Founding Release Readiness:**
All public-facing docs now consistent, duplicates removed, clear
separation between public (docs/) and internal (internal/, staging/).

Closes: Project count inconsistency (#tracked elsewhere)
Related: SIL founding release preparation
EOF
)"
```

---

## 6. Website Publication Strategy

### 6.1 Founding Release Content (INCLUDE)

**Must-have for launch:**

**Core:**
- docs/canonical/FOUNDERS_LETTER.md
- docs/canonical/SIL_MANIFESTO.md
- docs/canonical/SIL_PRINCIPLES.md
- docs/canonical/SIL_GLOSSARY.md

**Getting Started:**
- docs/meta/QUICKSTART.md
- docs/meta/FAQ.md
- docs/meta/READING_GUIDE.md

**Architecture:**
- docs/architecture/UNIFIED_ARCHITECTURE_GUIDE.md
- docs/canonical/SIL_TECHNICAL_CHARTER.md
- docs/canonical/SIL_STEWARDSHIP_MANIFESTO.md

**Innovations:**
- docs/innovations/INNOVATIONS.md (index)
- docs/innovations/MORPHOGEN.md
- docs/innovations/GENESISGRAPH.md
- docs/innovations/PANTHEON.md
- docs/innovations/AGENT_ETHER.md
- docs/innovations/PROGRESSIVE_DISCLOSURE.md

**Research:**
- docs/research/RAG_AS_SEMANTIC_MANIFOLD_TRANSPORT.md
- docs/research/AGENT_HELP_STANDARD.md

**Total:** 20 documents for founding release

---

### 6.2 Internal Content (EXCLUDE)

**Never publish:**
- internal/strategy/* (all 11 files)
- internal/pitch/* (all 8 files)
- internal/personnel/* (all 7 files)
- internal/operations/* (all 1 file)
- internal/planning/* (all planning docs)
- staging/* (all draft content)

**Reasoning:**
- Strategy docs reveal competitive positioning
- Pitch materials are for investor conversations
- Personnel docs contain private background info
- Staging content not ready for public consumption

---

### 6.3 Optional/Future Content (CONSIDER)

**Could add later:**
- docs/canonical/PROGRESSIVE_DISCLOSURE_GUIDE.md (912 lines - maybe excerpt?)
- docs/canonical/HIERARCHICAL_AGENCY_FRAMEWORK.md
- docs/canonical/SEMANTIC_FEEDBACK_LOOPS.md
- docs/research/AI_DOCUMENTATION_STANDARDS.md
- docs/research/IDENTITY_MAPPING.md

**Reasoning:** Good content, but maybe overwhelming for founding release

---

## 7. Success Criteria

**Founding release ready when:**

- [ ] ✅ No duplicate files in docs/
- [ ] ✅ All files show "13 projects, 5 in production"
- [ ] ✅ No broken links in public-facing docs
- [ ] ✅ Clear separation: docs/ (public) vs internal/ (private)
- [ ] ✅ PROGRESSIVE_DISCLOSURE_GUIDE vs PROGRESSIVE_DISCLOSURE distinction clear
- [ ] ✅ Website nav only references docs/ (not internal/)
- [ ] ✅ README.md badge shows 13 projects
- [ ] ✅ All canonical docs in docs/canonical/
- [ ] ✅ All meta docs in docs/meta/
- [ ] ✅ Git history clean (good commit message)

---

## 8. Risk Assessment

**Low Risk:**
- File moves preserve git history
- No content deleted, only reorganized
- Can revert commits if issues found
- Most docs not yet externally referenced

**Medium Risk:**
- Project count update (11→13) requires careful search/replace
- Link updates must be thorough
- Website navigation needs to be checked

**Mitigation:**
1. Test all links before commit
2. Review diff carefully
3. Keep staging branch for rollback
4. Document all changes clearly

---

## 9. Post-Consolidation Validation

**Run after Phase 8:**

```bash
# Check for broken internal links
find docs/ -name "*.md" -exec grep -l "](.*\.md)" {} \; | \
  xargs -I {} python3 -c "
import re, sys
with open('{}') as f:
    content = f.read()
    links = re.findall(r'\]\((.*?\.md.*?)\)', content)
    for link in links:
        if not link.startswith('http'):
            print('{}: {}'.format('{}', link))
"

# Verify project count consistency
grep -rn "projects" docs/ README.md | grep -E "[0-9]+ projects" | \
  grep -v "13 projects" | grep -v "Projects-13"

# Check for accidental internal/ references
grep -rn "internal/" docs/ README.md 2>/dev/null | grep -v "^#"
```

---

## 10. Timeline Estimate

**Total Time:** ~45 minutes

- Phase 1 (Remove duplicates): 2 min
- Phase 2 (Reorganize PROGRESSIVE_DISCLOSURE): 1 min
- Phase 3 (Move misplaced files): 2 min
- Phase 4 (Update project counts): 15 min
- Phase 5 (Verify internal separation): 5 min
- Phase 6 (Update navigation & links): 10 min
- Phase 7 (Document review checklist): 5 min
- Phase 8 (Commit & document): 5 min

**Ready to execute immediately.**

---

## Appendix A: Project Count Breakdown

**13 Total Projects:**

**Production (5):**
1. Morphogen - Universal computation (v0.11, 900+ tests)
2. TiaCAD - Declarative CAD (v3.1.1, 1027 tests)
3. GenesisGraph - Verifiable provenance (v0.3.0, 363 tests)
4. Reveal - Code exploration (v0.16.0, PyPI published)
5. SIL - Meta-project/documentation

**Active Development (3):**
6. RiffStack - Musical MLIR (MVP)
7. Sup - Semantic UI (Alpha)
8. BrowserBridge - Browser control (Alpha)

**Research (2):**
9. Pantheon - Universal Semantic IR
10. Philbrick - Analog/digital hybrid computing

**Specification (2):**
11. Agent-Ether - Multi-agent protocols
12. Prism - Microkernel query engine

**Planned (1):**
13. Semantic-Memory - Persistent semantic graph

**Test Coverage:** 3,250+ tests total (production projects)

---

## Appendix B: File Inventory Summary

**Current State:**
- docs/ (top-level): 4 files (3 outdated duplicates + 1 misplaced)
- docs/canonical/: 14 files
- docs/meta/: 5 files (includes 3 correct versions)
- docs/innovations/: 6 files
- docs/research/: 5 files
- docs/architecture/: 2 files
- docs/tools/: 2 files
- internal/: 27 files (strategy, pitch, personnel, operations)
- staging/: 15+ files (drafts)

**After Consolidation:**
- docs/ (top-level): 0 files (clean root)
- docs/canonical/: 15 files (+ PROGRESSIVE_DISCLOSURE_GUIDE)
- docs/meta/: 4 files (duplicates removed)
- docs/innovations/: 6 files (unchanged)
- docs/research/: 5 files (unchanged)
- docs/architecture/: 3 files (+ DESIGN_PRINCIPLES)
- docs/tools/: 2 files (unchanged)
- internal/: 27 files (unchanged, not for release)
- staging/: 15+ files (unchanged, not for release)

**Total public docs:** ~35 files
**Founding release docs:** ~20 files (curated subset)

---

**STATUS:** Ready for immediate execution
**OWNER:** TIA + Scott review
**TIMELINE:** 45 minutes
**RISK:** Low (reversible, preserves history)

---

END OF PLAN
