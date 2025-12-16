# Content Manifest Quick Reference

**One-page cheat sheet for common manifest operations**

---

## Daily Operations

### Check What's Public
```bash
grep "visibility: public" docs/CONTENT_MANIFEST.yaml | grep "path:"
```

### Validate System Health
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --validate
# ✓ No internal files found in website repo
```

### Sync Public Docs to Website
```bash
./scripts/sync-docs.py
```

### Preview Changes (Dry Run)
```bash
./scripts/sync-docs.py --dry-run
```

---

## Adding Files

### New Internal Doc (WIP)
```yaml
# Add to CONTENT_MANIFEST.yaml
  - path: docs/research/WIP_PAPER.md
    visibility: internal
    purpose: Research on [topic]
    decision_date: 2025-12-XX
    recommendation: Review before publication
```

### New Public Doc
```yaml
# Add to CONTENT_MANIFEST.yaml
  - path: docs/systems/NEW_TOOL.md
    visibility: public
    purpose: Documentation for new tool
```

Then sync:
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
```

---

## Changing Visibility

### Make Internal → Public
```yaml
# Change in CONTENT_MANIFEST.yaml:
  - path: docs/meta/SOME_DOC.md
    visibility: public  # Changed from internal
    made_public: 2025-12-XX
```

Update stats:
```yaml
stats:
  public_files: 58  # +1
  internal_files: 12  # -1
```

Sync:
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
```

### Make Public → Internal
```yaml
# Change in CONTENT_MANIFEST.yaml:
  - path: docs/meta/SOME_DOC.md
    visibility: internal  # Changed from public
    made_internal: 2025-12-XX
    reason: Contains sensitive information
```

Update stats, then clean from website:
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --clean
```

---

## Emergency Procedures

### Internal Doc Accidentally Published
```bash
# 1. Mark internal in manifest
cd /home/scottsen/src/projects/SIL
vim docs/CONTENT_MANIFEST.yaml
# Set visibility: internal

# 2. Remove from website
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --clean

# 3. Verify removed
./scripts/sync-docs.py --validate

# 4. Deploy fix immediately
./deploy/deploy-container.sh production
```

### Sync Script Broken
```bash
# Use backward-compatible wrapper
./scripts/sync-docs.sh

# Or manual rsync (emergency only)
rsync -av --delete \
  ../SIL/docs/foundations/ \
  docs/foundations/
```

---

## File Operations

### Edit Public Doc
```bash
# 1. Edit in SIL repo
cd /home/scottsen/src/projects/SIL
vim docs/systems/reveal.md
git commit -am "docs: Update Reveal docs"

# 2. Sync to website
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
git add docs/systems/reveal.md
git commit -m "docs: Sync Reveal updates"
```

### Edit Internal Doc
```bash
# Just edit in SIL repo - won't sync
cd /home/scottsen/src/projects/SIL
vim docs/meta/SIL_RESEARCH_AGENDA_YEAR1.md
git commit -am "docs: Update research agenda"
```

### Delete Doc
```bash
# 1. Remove file
git rm docs/path/to/file.md

# 2. Remove from manifest
vim docs/CONTENT_MANIFEST.yaml

# 3. Update stats
# Decrement total_files and public_files or internal_files

# 4. Commit
git commit -m "docs: Remove obsolete file"
```

---

## Validation

### Find Files Not in Manifest
```bash
comm -13 \
  <(grep "path: docs/" docs/CONTENT_MANIFEST.yaml | \
    sed 's/.*path: //' | sort) \
  <(find docs -name "*.md" | sort)
```

### Check Internal Files in Website
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --validate
```

### Verify YAML Syntax
```bash
python3 -c "import yaml; yaml.safe_load(open('docs/CONTENT_MANIFEST.yaml'))"
```

---

## Stats

### Count Public Files
```bash
grep "visibility: public" docs/CONTENT_MANIFEST.yaml | wc -l
```

### Count Internal Files
```bash
grep "visibility: internal" docs/CONTENT_MANIFEST.yaml | wc -l
```

### List Flagged Files
```bash
grep -B2 "flagged_for_removal: true" docs/CONTENT_MANIFEST.yaml
```

### List Under Review
```bash
grep -B2 "under_review: true" docs/CONTENT_MANIFEST.yaml
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Internal file in website | `./scripts/sync-docs.py --clean` |
| Sync does nothing | Check parser reads `files[].visibility` |
| YAML syntax error | Check indentation, quote colons |
| File not syncing | Check `visibility: public` in manifest |
| Too many files warning | Normal if ~9 website-specific files |

---

## File Locations

| File | Location |
|------|----------|
| Manifest | `/projects/SIL/docs/CONTENT_MANIFEST.yaml` |
| Sync script | `/projects/sil-website/scripts/sync-docs.py` |
| Full guide | `/projects/SIL/docs/operations/CONTENT_MANIFEST_GUIDE.md` |
| Deployment guide | `/projects/SIL/docs/operations/WEBSITE_DEPLOYMENT_GUIDE.md` |
| Website README | `/projects/sil-website/docs/README_SYNCED_CONTENT.md` |

---

## Key Commands

```bash
# Validate
./scripts/sync-docs.py --validate

# Dry run
./scripts/sync-docs.py --dry-run

# Sync
./scripts/sync-docs.py

# Clean
./scripts/sync-docs.py --clean
```

---

**Full documentation:** CONTENT_MANIFEST_GUIDE.md
