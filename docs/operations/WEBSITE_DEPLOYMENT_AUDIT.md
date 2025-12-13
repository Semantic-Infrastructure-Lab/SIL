# Website Deployment Architecture Audit

**Date**: 2025-12-12
**Session**: sunny-cyclone-1212
**Auditor**: TIA

## Executive Summary

Audited the SIL/SIF website deployment architecture to identify gaps, risks, and needed improvements following the bio update work from session vugatavo-1212.

**Key Finding**: The current architecture has ambiguous source-of-truth management that led to incorrect edits in vugatavo-1212 (editing generated/synced files instead of sources).

---

## Current Architecture

### Repository Structure

```
/home/scottsen/src/projects/
├── SIL/                          # SOURCE OF TRUTH (git tracked)
│   ├── docs/
│   │   ├── canonical/
│   │   ├── architecture/
│   │   ├── research/
│   │   ├── meta/
│   │   │   └── FOUNDER_BACKGROUND.md  ← Source for SIL bio
│   │   └── ...
│   └── ...
│
├── sil-website/                  # SIL Website (FastAPI app)
│   ├── docs/                     ← SYNCED FROM ../SIL/ (gitignored)
│   ├── static/
│   │   ├── llms-full.txt         ← GENERATED from ../SIL/docs (git tracked)
│   │   └── llms.txt              ← Manual file (git tracked)
│   ├── scripts/
│   │   ├── sync-docs.sh          ← Copies ../SIL/docs → ./docs/
│   │   └── generate-llms-full.sh ← Generates llms-full.txt from ../SIL/docs
│   └── deploy/
│       └── deploy-container.sh   ← Builds & deploys containers
│
└── sif-website/                  # SIF Website (FastAPI app)
    ├── docs/                     ← SYNCED FROM ../SIL/ (gitignored)
    │   └── pages/
    │       └── about.md          ← SIF-specific content (NOT synced)
    ├── static/
    │   └── llms-full.txt         ← GENERATED (git tracked)
    └── scripts/
        ├── sync-docs.sh
        └── generate-llms-full.sh
```

### Data Flow

```
1. EDIT SOURCE
   /projects/SIL/docs/meta/FOUNDER_BACKGROUND.md
   ↓
2. COMMIT SOURCE
   git add docs/
   git commit -m "fix: correct founder bio facts"
   ↓
3. SYNC TO WEBSITES
   cd /projects/sil-website
   ./scripts/sync-docs.sh
   (rsync ../SIL/docs/* → ./docs/)
   ↓
4. REGENERATE LLM FILES
   ./scripts/generate-llms-full.sh
   (concatenate docs/* → static/llms-full.txt)
   ↓
5. COMMIT WEBSITE
   git add docs/ static/llms-full.txt
   git commit -m "sync: update founder bio"
   ↓
6. BUILD CONTAINER
   ./deploy/deploy-container.sh staging --fresh
   (--fresh flag needed to bypass podman cache)
   ↓
7. DEPLOY TO STAGING
   (deploy-container.sh pushes to registry & deploys)
   ↓
8. TEST STAGING
   https://sil-staging.mytia.net
   ↓
9. DEPLOY TO PRODUCTION
   ./deploy/deploy-container.sh production --fresh
   ↓
10. VERIFY PRODUCTION
    https://semanticinfrastructurelab.org
```

---

## Issues Identified

### 🔴 CRITICAL: Ambiguous Source of Truth

**Problem**: Not clear which files are sources vs derived/synced

**Example**: vugatavo-1212 edited:
- `/projects/sil-website/docs/meta/FOUNDER_BACKGROUND.md` (synced/derived)
- `/projects/sif-website/docs/pages/about.md` (unclear status)
- `/projects/{sil,sif}-website/static/llms-full.txt` (generated)

**Risk**: Changes get overwritten on next sync/regeneration

**Evidence**:
```bash
$ diff /projects/SIL/docs/meta/FOUNDER_BACKGROUND.md \
       /projects/sil-website/docs/meta/FOUNDER_BACKGROUND.md
Files differ  # vugatavo-1212 updated website copy, not source
```

**Solution**: Clear documentation + enforcement

---

### 🟡 MEDIUM: Inconsistent SIF Content Management

**Problem**: SIF website has content that doesn't sync from SIL repo

**Location**: `/projects/sif-website/docs/pages/about.md`

**Questions**:
- Is this SIF-specific content or should it be in SIL repo?
- Should there be a `/projects/SIF/` source repo?
- Should pages/ be git-tracked separately?

**Current State**:
- `docs/` is gitignored (entire directory)
- `docs/pages/about.md` exists but is not version controlled
- No clear source of truth for SIF-specific pages

**Risks**:
- Content loss (not backed up if not in git)
- No history/tracking of changes
- Unclear ownership

---

### 🟡 MEDIUM: Generated Files Tracked in Git

**File**: `static/llms-full.txt` (544KB)

**Current**: Git tracked, must be committed after regeneration

**Issues**:
- Large binary-like file in git history
- Merge conflicts likely if multiple editors
- Redundant (duplicates content from docs/)
- Easy to forget regeneration step

**Options**:
1. **Keep as-is**: Simple, works for CDN/caching
2. **Gitignore + Build**: Generate during container build
3. **Dynamic**: Generate on-demand at request time
4. **CDN**: Upload to S3/CDN separately from git

**Recommendation**: Need decision based on:
- How often docs change?
- Do we want version history of full file?
- Is 544KB in git acceptable?

---

### 🟢 LOW: Manual sed Edits in vugatavo-1212

**What happened**: Used `sed` to edit `llms-full.txt` directly

**Why wrong**: File is regenerated from source, edits lost

**Fix**: Already corrected - copied edits back to source repo

**Prevention**: Documentation + validation script

---

### 🟢 LOW: Missing Validation Steps

**Current**: Manual verification after deployment

**Needed**:
- Pre-deployment checks (source vs derived files match)
- Post-deployment smoke tests
- Automated validation in deploy scripts

---

## Best Practices Assessment

### ✅ Good Practices Currently Followed

1. **Separation of concerns**: Source repo separate from web apps
2. **Containerization**: Reproducible deployments
3. **Staging environment**: Test before production
4. **Scripts**: Automated sync and generation
5. **Documentation**: DEPLOYMENT.md and DEPLOY_WORKFLOW.md exist

### ❌ Gaps / Improvements Needed

1. **Source of truth not clearly marked** in filesystem
2. **No validation** that derived files match sources
3. **No pre-commit hooks** to prevent editing derived files
4. **Incomplete documentation** (missing SIF-specific workflow)
5. **No rollback procedure** documented
6. **No health checks** after deployment
7. **Generated files in git** (llms-full.txt) - needs decision

---

## Recommendations

### Immediate (This Session)

1. ✅ **Fix vugatavo-1212 edits**: Copy corrected content to source repo
2. ⏳ **Document source of truth**: Create clear guide
3. ⏳ **Fix SIF about.md**: Determine source location
4. ⏳ **Regenerate llms-full.txt**: From corrected sources
5. ⏳ **Create deployment checklist**: Step-by-step validation

### Short-Term (Next Week)

1. **Add source markers**: Create `.SOURCE` files in source dirs
2. **Add validation script**: Check derived files match sources
3. **Update .gitignore comments**: Explain why each exclusion
4. **Document SIF workflow**: Separate guide for foundation site
5. **Add health checks**: Automated post-deployment validation

### Medium-Term (Next Month)

1. **Pre-commit hooks**: Warn if editing derived files
2. **CI/CD pipeline**: Automate sync → generate → test → deploy
3. **Rollback procedure**: Document and test
4. **Decide on llms-full.txt**: Keep in git vs dynamic generation
5. **Consider monorepo**: Single repo for SIL + websites?

---

## Proposed File Markers

To make source-of-truth clear, add README files:

### `/projects/SIL/docs/README.md`
```markdown
# SOURCE OF TRUTH

This directory contains the canonical documentation for SIL.

⚠️ EDIT HERE - These files are synced to websites

DO NOT edit files in website repos:
- /projects/sil-website/docs/ ← SYNCED (will be overwritten)
- /projects/sif-website/docs/canonical,meta,etc. ← SYNCED

Workflow:
1. Edit files here
2. Commit to git
3. Run website sync scripts
4. Regenerate llms-full.txt
5. Deploy
```

### `/projects/sil-website/docs/README.md`
```markdown
# SYNCED CONTENT - DO NOT EDIT DIRECTLY

⚠️ This directory is synced from /projects/SIL/docs/

Changes made here will be OVERWRITTEN.

To update content:
1. Edit source: /projects/SIL/docs/
2. Run: ./scripts/sync-docs.sh
3. Regenerate: ./scripts/generate-llms-full.sh
4. Deploy
```

---

## Questions for Decision

1. **SIF Pages**: Should `docs/pages/*.md` be:
   - [ ] Git tracked in sif-website repo?
   - [ ] Moved to `/projects/SIF/` source repo?
   - [ ] Synced from somewhere else?

2. **llms-full.txt**: Should we:
   - [ ] Keep in git (current)?
   - [ ] Gitignore and generate during build?
   - [ ] Generate dynamically on request?

3. **Monorepo**: Would it simplify source management to have:
   - [ ] Single repo: SIL + sil-website + sif-website?
   - [ ] Current multi-repo structure is fine?

4. **CI/CD**: Should deployment be:
   - [ ] Manual (current)?
   - [ ] Automated on git push?
   - [ ] Hybrid (automated staging, manual production)?

---

## Next Steps

See `WEBSITE_DEPLOYMENT_GUIDE.md` for comprehensive documentation incorporating these findings.
