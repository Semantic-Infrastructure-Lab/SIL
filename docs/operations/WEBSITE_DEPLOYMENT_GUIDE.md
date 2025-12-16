# Website Deployment Guide - Complete Workflow

**Version**: 1.0
**Date**: 2025-12-12
**Maintainer**: SIL Infrastructure Team

---

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Source of Truth](#source-of-truth)
3. [Content Update Workflow](#content-update-workflow)
4. [Deployment Workflow](#deployment-workflow)
5. [Common Operations](#common-operations)
6. [Troubleshooting](#troubleshooting)
7. [Validation Checklists](#validation-checklists)

---

## Architecture Overview

### Repositories

```
/projects/SIL/              ← SOURCE OF TRUTH (git tracked)
  └── docs/                 ← Edit here for SIL website content

/projects/sil-website/      ← SIL Website (FastAPI app)
  ├── docs/                 ← SYNCED (gitignored, do not edit)
  ├── static/llms-full.txt  ← GENERATED (git tracked)
  └── scripts/sync-docs.sh  ← Sync from SIL repo

/projects/sif-website/      ← SIF Website (FastAPI app)
  ├── docs/                 ← SYNCED (gitignored, do not edit)
  │   └── pages/            ← SIF-specific pages (see note below)
  └── static/llms-full.txt  ← GENERATED (git tracked)
```

### Infrastructure

- **Staging Server**: tia-staging (10.108.0.8)
  - SIL: Port 8080 → https://sil-staging.mytia.net
  - SIF: Port 8082 → https://sif-staging.mytia.net

- **Production Server**: tia-apps
  - SIL: Port 8010 → https://semanticinfrastructurelab.org
  - SIF: Port 8011 → https://semanticinfrastructurefoundation.org

- **Registry**: registry.mytia.net

---

## Source of Truth

### ⚠️ CRITICAL: Where to Edit Files

| Content | Source Location | Synced To | Method |
|---------|----------------|-----------|--------|
| SIL canonical docs | `/projects/SIL/docs/canonical/` | Both websites | sync-docs.sh |
| SIL architecture | `/projects/SIL/docs/architecture/` | Both websites | sync-docs.sh |
| SIL research | `/projects/SIL/docs/research/` | Both websites | sync-docs.sh |
| SIL meta (founder bio) | `/projects/SIL/docs/meta/` | SIL website | sync-docs.sh |
| SIF pages (about, funding) | `/projects/sif-website/docs/pages/` | N/A | Manual ⚠️ |
| llms-full.txt | **GENERATED** | N/A | generate-llms-full.sh |

### 🔴 DO NOT EDIT

- `/projects/{sil,sif}-website/docs/` ← Will be overwritten by sync
- `/projects/{sil,sif}-website/static/llms-full.txt` ← Will be overwritten by generate script

### 🟡 EXCEPTION: SIF Pages

**Current Issue**: SIF-specific pages (`docs/pages/*.md`) are:
- Not synced from SIL repo
- Not git tracked (in gitignored `docs/` directory)
- Unclear source of truth

**Temporary Workflow**:
1. Edit `/projects/sif-website/docs/pages/about.md` directly
2. Manually backup to `/projects/SIL/foundation/website-content/` (suggested)
3. Commit backup to SIL repo for version control

**TODO**: Decide permanent solution (see WEBSITE_DEPLOYMENT_AUDIT.md)

---

## Content Update Workflow

### ⚠️ IMPORTANT: Content Manifest System

**As of 2025-12-16**, all documentation is controlled by **CONTENT_MANIFEST.yaml**.

- **Public docs** (`visibility: public`) are synced to website automatically
- **Internal docs** (`visibility: internal`) are NOT synced, kept in SIL repo only
- See **CONTENT_MANIFEST_GUIDE.md** for full details

### Scenario 1: Update Existing Public Documentation

**Example**: Fix founder bio, update manifesto, add to research paper

```bash
# 1. Edit source files in SIL repo
cd /home/scottsen/src/projects/SIL
vim docs/meta/FOUNDER_BACKGROUND.md

# 2. Verify it's marked public in manifest
grep -A3 "FOUNDER_BACKGROUND.md" docs/CONTENT_MANIFEST.yaml
# Should show: visibility: public

# 3. Commit to source repo
git add docs/meta/FOUNDER_BACKGROUND.md
git commit -m "fix: Correct founder bio facts"
git push

# 4. Sync to SIL website (manifest-aware)
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
# Or use backward-compatible wrapper: ./scripts/sync-docs.sh

# 5. Verify sync worked
git status
git diff docs/meta/FOUNDER_BACKGROUND.md

# 6. Commit website changes
git add docs/
git commit -m "docs: Sync founder bio updates from SIL repo"
git push

# 7. Deploy (see Deployment Workflow section)
./deploy/deploy-container.sh staging
```

**Time**: ~10 minutes (edit to deployed)

### Scenario 1b: Add New Documentation File

**Example**: Write new research paper

```bash
# 1. Create file in SIL repo
cd /home/scottsen/src/projects/SIL
vim docs/research/NEW_PAPER.md

# 2. Add to CONTENT_MANIFEST.yaml
vim docs/CONTENT_MANIFEST.yaml
```

Add entry:
```yaml
  - path: docs/research/NEW_PAPER.md
    visibility: internal  # Start internal, promote after review
    purpose: Research on [topic]
    decision_date: 2025-12-XX
    recommendation: Review for publication after peer feedback
```

Update stats:
```yaml
stats:
  total_files: 71  # Increment
  internal_files: 14  # Increment
```

```bash
# 3. Commit both files
git add docs/research/NEW_PAPER.md docs/CONTENT_MANIFEST.yaml
git commit -m "docs: Add new research paper (internal for review)"

# 4. When ready to publish, change visibility to public
vim docs/CONTENT_MANIFEST.yaml
# Change: visibility: public

# 5. Update stats and commit
git commit -am "docs: Make NEW_PAPER public after review"

# 6. Sync to website
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
git add docs/research/NEW_PAPER.md
git commit -m "docs: Add NEW_PAPER (approved for publication)"
```

**See**: CONTENT_MANIFEST_GUIDE.md for detailed workflow

---

### Scenario 2: Update SIF-Specific Pages

**Example**: Update foundation about page, modify funding model

```bash
# 1. Edit SIF pages directly (temporary workflow)
cd /home/scottsen/src/projects/sif-website
vim docs/pages/about.md

# 2. Backup to SIL repo for version control
mkdir -p /home/scottsen/src/projects/SIL/foundation/website-content
cp docs/pages/about.md /home/scottsen/src/projects/SIL/foundation/website-content/
cd /home/scottsen/src/projects/SIL
git add foundation/website-content/about.md
git commit -m "backup: SIF about page content"
git push

# 3. Regenerate llms-full.txt (if about.md included in script)
cd /home/scottsen/src/projects/sif-website
./scripts/generate-llms-full.sh

# 4. Deploy
./deploy/deploy-container.sh staging --fresh
```

**⚠️ Note**: This workflow is suboptimal - see audit for permanent solution

---

### Scenario 3: Update Both Websites

**Example**: Update shared docs + SIF-specific pages

```bash
# 1. Update SIL source repo (shared content)
cd /home/scottsen/src/projects/SIL
vim docs/canonical/SIL_MANIFESTO.md
git commit -am "docs: Update manifesto section 3"
git push

# 2. Sync to BOTH websites
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.sh
./scripts/generate-llms-full.sh
git commit -am "sync: Update manifesto from SIL repo"
git push

cd /home/scottsen/src/projects/sif-website
./scripts/sync-docs.sh
# Also update SIF-specific pages if needed
vim docs/pages/funding.md
./scripts/generate-llms-full.sh
git commit -am "sync: Update manifesto + funding page"
git push

# 3. Deploy both (can run in parallel)
cd /home/scottsen/src/projects/sil-website
./deploy/deploy-container.sh staging --fresh &

cd /home/scottsen/src/projects/sif-website
./deploy/deploy-container.sh staging --fresh &

wait  # Wait for both to complete
```

---

## Deployment Workflow

### Pre-Deployment Checklist

```bash
# 1. Verify source changes committed
cd /home/scottsen/src/projects/SIL
git status  # Should be clean

# 2. Verify sync completed
cd /home/scottsen/src/projects/sil-website
diff -r docs/meta/ ../SIL/docs/meta/  # Should match

# 3. Verify llms-full.txt regenerated
ls -lh static/llms-full.txt
# Check timestamp is recent

# 4. Verify website changes committed
git status  # Should show clean or ready to commit

# 5. Check infrastructure health
tia infrastructure status
# Should show all services healthy
```

---

### Deploy to Staging

```bash
cd /home/scottsen/src/projects/sil-website

# Build and deploy with --fresh flag (bypasses cache)
./deploy/deploy-container.sh staging --fresh

# What happens:
# 1. Builds container image from current directory
# 2. Tags as registry.mytia.net/sil-website:staging
# 3. Pushes to registry
# 4. SSHs to tia-staging server
# 5. Pulls image and restarts container
# 6. Verifies health endpoint

# Expected output:
# 🚀 SIL Website Container Deployment
# ====================================
# Environment: staging
# ...
# ✅ Deployment successful
# 🌐 Staging: https://sil-staging.mytia.net
```

**Time**: ~5-7 minutes

---

### Verify Staging

```bash
# Manual verification
curl -I https://sil-staging.mytia.net/health
# Expected: HTTP/2 200

# Check specific content
curl -s https://sil-staging.mytia.net/docs/meta/founder-background | grep "Cal Poly"
# Should show updated content

# Visual verification
# Open in browser: https://sil-staging.mytia.net
# Navigate to founder bio, verify facts correct
```

---

### Deploy to Production

```bash
# ⚠️ Only after staging verified!

cd /home/scottsen/src/projects/sil-website

# Deploy to production
./deploy/deploy-container.sh production --fresh

# What happens:
# 1. Same as staging but targets tia-apps server
# 2. Different port (8010 vs 8080)
# 3. Production domain

# Expected output:
# 🚀 SIL Website Container Deployment
# ====================================
# Environment: production
# ...
# ✅ Deployment successful
# 🌐 Production: https://semanticinfrastructurelab.org
```

**Time**: ~5-7 minutes

---

### Verify Production

```bash
# Health check
curl -I https://semanticinfrastructurelab.org/health

# Content verification
curl -s https://semanticinfrastructurelab.org/docs/meta/founder-background | \
  grep "California Polytechnic"

# Full site check
# Browser: https://semanticinfrastructurelab.org
# Verify:
# - Founder bio shows correct facts
# - No broken links
# - llms-full.txt accessible: /llms-full.txt
```

---

## Common Operations

### Operation: Emergency Rollback

**Scenario**: Deployed bad content, need to rollback immediately

```bash
# Option 1: Redeploy previous container (if still in registry)
ssh tia-apps
podman ps -a | grep sil-website  # Find previous container ID
podman start <previous-container-id>
podman stop sil-website
podman rename sil-website sil-website-bad
podman rename <previous-container-id> sil-website

# Option 2: Revert git commits and redeploy
cd /home/scottsen/src/projects/sil-website
git log --oneline -5  # Find last good commit
git revert <bad-commit-hash>
./deploy/deploy-container.sh production --fresh

# Option 3: Restore from source
cd /home/scottsen/src/projects/SIL
git log docs/ --oneline -5  # Find last good version
git checkout <good-commit> -- docs/
git commit -m "revert: Restore docs to working state"
# Then re-sync and deploy
```

**Time**: 10-15 minutes

---

### Operation: Update Single Document

```bash
# Fast path for minor fixes
cd /home/scottsen/src/projects/SIL
vim docs/meta/FAQ.md
git commit -am "docs: Fix typo in FAQ"
git push

cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.sh
./scripts/generate-llms-full.sh
git commit -am "sync: Update FAQ"
./deploy/deploy-container.sh production --fresh
```

---

### Operation: Bulk Document Update

```bash
# Multiple documents changed
cd /home/scottsen/src/projects/SIL
# ... edit multiple files ...
git add docs/
git commit -m "docs: Major update to research papers and architecture"
git push

cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.sh
./scripts/generate-llms-full.sh

# Review changes before committing
git diff static/llms-full.txt | head -100
git status

git commit -am "sync: Bulk documentation update"
./deploy/deploy-container.sh staging --fresh

# Test staging thoroughly
# ... verification ...

./deploy/deploy-container.sh production --fresh
```

---

### Operation: Regenerate llms-full.txt Only

```bash
# If you suspect llms-full.txt is stale
cd /home/scottsen/src/projects/sil-website
./scripts/generate-llms-full.sh

# Review
ls -lh static/llms-full.txt
tail -50 static/llms-full.txt  # Check generation timestamp

# Commit and deploy if needed
git diff static/llms-full.txt
git commit -am "regenerate: Update llms-full.txt"
./deploy/deploy-container.sh staging --fresh
```

---

## Troubleshooting

### Problem: Deployed site shows old content

**Symptoms**:
- Made edits, deployed, but site shows old version
- curl shows old content

**Diagnosis**:
```bash
# Check container timestamp
ssh tia-apps
podman inspect sil-website | grep Created
# Should be recent (within last hour if you just deployed)

# Check container file contents
podman exec sil-website cat /app/docs/meta/FOUNDER_BACKGROUND.md | head -20
# Should show new content
```

**Causes**:
1. **Forgot --fresh flag**: Container built from cache
2. **Forgot to sync**: docs/ not synced from SIL repo
3. **Forgot to regenerate**: llms-full.txt not regenerated
4. **Wrong source**: Edited website copy instead of SIL repo

**Fix**:
```bash
# Verify source
cd /home/scottsen/src/projects/SIL
git log -1 docs/meta/FOUNDER_BACKGROUND.md
# Should show recent commit with your changes

# Re-sync
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.sh
./scripts/generate-llms-full.sh

# Rebuild with --fresh (critical!)
./deploy/deploy-container.sh production --fresh
```

---

### Problem: llms-full.txt has old facts

**Symptoms**:
- Main site correct, but /llms-full.txt shows old content
- grep finds incorrect information

**Diagnosis**:
```bash
cd /home/scottsen/src/projects/sil-website
grep "Colorado Boulder" static/llms-full.txt
# Should return nothing if corrected

grep "Cal Poly" static/llms-full.txt
# Should find correct education
```

**Causes**:
1. Forgot to run `generate-llms-full.sh`
2. Generated before sync
3. Manually edited llms-full.txt (wrong!)

**Fix**:
```bash
# Regenerate from current docs
./scripts/generate-llms-full.sh

# Verify
grep -c "Cal Poly" static/llms-full.txt  # Should be > 0
grep -c "Colorado Boulder" static/llms-full.txt  # Should be 0

# Commit and deploy
git commit -am "regenerate: Update llms-full.txt with correct facts"
./deploy/deploy-container.sh production --fresh
```

---

### Problem: SIF pages not updating

**Symptoms**:
- SIF about page shows old content
- Edited docs/pages/about.md but deploy didn't change it

**Diagnosis**:
```bash
cd /home/scottsen/src/projects/sif-website

# Check if about.md in container
ssh tia-apps
podman exec sif-website cat /app/docs/pages/about.md | head -20
# Should show new content

# Check Dockerfile
grep "COPY.*docs" Dockerfile
# Should include: COPY --chown=appuser:appuser docs/ docs/
```

**Causes**:
1. Forgot --fresh flag (cache issue)
2. docs/pages/ not included in COPY
3. Application not reading from correct path

**Fix**:
```bash
# Rebuild with --fresh
./deploy/deploy-container.sh staging --fresh

# If still fails, check Dockerfile includes pages
grep "docs/" Dockerfile
```

---

### Problem: "File not found" errors in container

**Symptoms**:
- Container starts but 404 errors
- Missing docs or static files

**Diagnosis**:
```bash
ssh tia-apps
podman exec sil-website ls -la /app/docs/
podman exec sil-website ls -la /app/static/
```

**Causes**:
1. Forgot to sync before build
2. Paths wrong in Dockerfile COPY
3. .dockerignore excluding needed files

**Fix**:
```bash
# Verify local files exist
cd /home/scottsen/src/projects/sil-website
ls -la docs/ static/

# Check .dockerignore
cat .dockerignore

# Rebuild
./deploy/deploy-container.sh staging --fresh
```

---

## Validation Checklists

### Pre-Deployment Checklist

**Before deploying to staging or production**:

- [ ] Source repo changes committed and pushed
- [ ] Sync script executed successfully
- [ ] llms-full.txt regenerated (check timestamp)
- [ ] Git status clean (no uncommitted changes)
- [ ] Infrastructure healthy (`tia infrastructure status`)
- [ ] No pending hotfixes or rollbacks needed

---

### Post-Deployment Checklist (Staging)

**After deploying to staging**:

- [ ] Health endpoint responds: `curl https://sil-staging.mytia.net/health`
- [ ] Updated content visible in browser
- [ ] No 404 errors on navigation
- [ ] llms-full.txt accessible: `/llms-full.txt`
- [ ] Search for known corrections (e.g., "Cal Poly" present)
- [ ] No error logs: `ssh tia-staging; podman logs sil-website-staging`

---

### Post-Deployment Checklist (Production)

**After deploying to production**:

- [ ] Health endpoint responds: `curl https://semanticinfrastructurelab.org/health`
- [ ] Updated content visible
- [ ] All links working
- [ ] llms-full.txt accessible and correct
- [ ] No error logs: `ssh tia-apps; podman logs sil-website`
- [ ] Analytics tracking (if enabled)
- [ ] Notify team of deployment

---

## Quick Reference Commands

```bash
# Full workflow (SIL website)
cd /projects/SIL && vim docs/meta/FILE.md && git commit -am "docs: update" && git push
cd /projects/sil-website && ./scripts/sync-docs.sh && ./scripts/generate-llms-full.sh
git commit -am "sync: update docs" && ./deploy/deploy-container.sh staging --fresh

# Check what's deployed
ssh tia-apps "podman inspect sil-website | grep Created"
curl -I https://semanticinfrastructurelab.org/health

# Emergency rollback
cd /projects/sil-website && git revert HEAD && ./deploy/deploy-container.sh production --fresh

# Verify source vs synced
diff -r /projects/SIL/docs/meta /projects/sil-website/docs/meta

# Infrastructure status
tia infrastructure status
```

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-12 | Initial comprehensive guide (sunny-cyclone-1212) |

---

**Feedback**: Report issues or suggest improvements via session notes or project tracker
