# Content Manifest System - Maintenance Guide

**Version**: 1.0
**Created**: 2025-12-16 (charcoal-jewel-1216)
**Maintainer**: SIL Operations
**Visibility**: internal

---

## Overview

The Content Manifest System provides **glass-box transparency** for documentation visibility, solving the problem of internal docs being accidentally exposed on public websites.

### Key Principles

1. **Explicit over implicit** - Every file categorized with rationale
2. **Allowlist approach** - Must opt-in to publication (safe by default)
3. **Glass-box transparency** - Visible YAML, documented decisions
4. **Flat structure** - 2 levels max, simple to maintain

### Files in System

- **Manifest**: `/projects/SIL/docs/CONTENT_MANIFEST.yaml` (493 lines)
- **Sync Script**: `/projects/sil-website/scripts/sync-docs.py` (487 lines)
- **Wrapper**: `/projects/sil-website/scripts/sync-docs.sh` (backward compatibility)

---

## Quick Reference

### Check Manifest Status
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --validate
```

### Preview Changes Before Sync
```bash
./scripts/sync-docs.py --dry-run
```

### Sync Public Docs to Website
```bash
./scripts/sync-docs.py
```

### Remove Internal Files from Website
```bash
./scripts/sync-docs.py --clean
```

---

## Manifest Structure

### File Format (Flat YAML)

```yaml
files:
  - path: docs/meta/FAQ.md
    visibility: public
    purpose: Frequently asked questions about SIL
    lines: 245

  - path: docs/meta/DEDICATION.md
    visibility: internal
    flagged_for_removal: true
    purpose: Original Turing dedication
    reason: Grandiose tone contradicts calibration work
    decision_date: 2025-12-16
    decision_session: chrome-hue-1216
    recommendation: Remove entirely

  - path: docs/architecture/README.md
    visibility: public
    purpose: Architecture documentation index
    under_review: true
    note: May contain internal implementation details
```

### Key Fields

| Field | Required | Values | Purpose |
|-------|----------|--------|---------|
| `path` | Yes | `docs/path/to/file.md` | File path relative to SIL repo root |
| `visibility` | Yes | `public`, `internal` | Controls sync behavior |
| `purpose` | Recommended | String | What this file is for |
| `reason` | For internal | String | Why this is internal |
| `flagged_for_removal` | Optional | boolean | File should be deleted |
| `under_review` | Optional | boolean | Visibility decision pending |
| `decision_date` | For flagged | YYYY-MM-DD | When decision was made |
| `decision_session` | For flagged | session-id | Which session flagged it |
| `recommendation` | For flagged | String | What to do with file |

### Metadata Section

```yaml
stats:
  total_files: 70
  public_files: 57
  internal_files: 13
  flagged_for_removal: 1
  under_review: 10

last_audit:
  date: 2025-12-16
  session: chrome-hue-1216
  auditor: TIA
  findings: "Discovered 10 internal docs being synced to public website"

next_audit:
  date: 2025-01-16
  frequency: monthly
```

---

## Common Tasks

### Adding a New Documentation File

**1. Create the file in SIL repo**
```bash
cd /home/scottsen/src/projects/SIL
vim docs/research/NEW_RESEARCH_PAPER.md
```

**2. Add to manifest**
```bash
vim docs/CONTENT_MANIFEST.yaml
```

Add entry in the appropriate section:
```yaml
  - path: docs/research/NEW_RESEARCH_PAPER.md
    visibility: internal  # Start internal, promote to public after review
    purpose: Research on [topic]
    decision_date: 2025-12-16
    recommendation: Review for publication after peer feedback
```

**3. Update stats**
```yaml
stats:
  total_files: 71  # Increment
  internal_files: 14  # Increment if internal
```

**4. Commit**
```bash
git add docs/research/NEW_RESEARCH_PAPER.md docs/CONTENT_MANIFEST.yaml
git commit -m "docs: Add new research paper (internal for review)"
```

**5. If public, sync to website**
```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
```

### Making Internal Doc Public

**1. Update manifest visibility**
```bash
cd /home/scottsen/src/projects/SIL
vim docs/CONTENT_MANIFEST.yaml
```

Change:
```yaml
  - path: docs/meta/SOME_DOC.md
    visibility: public  # Changed from internal
    purpose: [existing purpose]
    made_public: 2025-12-16
    reason: Reviewed and approved for publication
```

**2. Update stats**
```yaml
stats:
  public_files: 58  # Increment
  internal_files: 12  # Decrement
```

**3. Commit and sync**
```bash
git commit -am "docs: Make SOME_DOC public after review"

cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py
git add docs/meta/SOME_DOC.md
git commit -m "docs: Add SOME_DOC (approved for publication)"
```

### Removing a Flagged File

**1. Verify it's flagged in manifest**
```yaml
  - path: docs/meta/DEDICATION.md
    visibility: internal
    flagged_for_removal: true
    recommendation: Remove entirely
```

**2. Delete from SIL repo**
```bash
cd /home/scottsen/src/projects/SIL
git rm docs/meta/DEDICATION.md
```

**3. Remove from manifest**
```bash
vim docs/CONTENT_MANIFEST.yaml
# Delete the entire entry for DEDICATION.md
```

**4. Update stats**
```yaml
stats:
  total_files: 69  # Decrement
  internal_files: 12  # Decrement if was internal
  flagged_for_removal: 0  # Decrement
```

**5. Commit**
```bash
git commit -m "docs: Remove DEDICATION.md (grandiose tone, flagged in chrome-hue-1216)"
```

### Updating Existing Doc Content

**For public docs:**

```bash
# 1. Edit in SIL repo (source of truth)
cd /home/scottsen/src/projects/SIL
vim docs/foundations/SIL_GLOSSARY.md

# 2. Commit
git commit -am "docs: Update glossary definitions"

# 3. Sync to website
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py

# 4. Commit synced changes
git add docs/foundations/SIL_GLOSSARY.md
git commit -m "docs: Sync glossary updates"

# 5. Deploy
./deploy/deploy-container.sh staging
```

**For internal docs:**
```bash
# Just edit and commit - won't be synced
cd /home/scottsen/src/projects/SIL
vim docs/meta/SIL_RESEARCH_AGENDA_YEAR1.md
git commit -am "docs: Update internal research agenda"
```

---

## Validation and Troubleshooting

### Validate Manifest Integrity

```bash
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --validate
```

**Expected output:**
```
✓ Found CONTENT_MANIFEST.yaml
✓ No internal files found in website repo
✓ Validation passed
```

### Check for Internal Files in Website

```bash
./scripts/sync-docs.py --validate
```

If violations found:
```
✗ Found internal file: meta/MARKDOWN_STYLE_GUIDE.md
⚠ Found 1 internal files in website repo

To remove these files:
  rm /path/to/file

Or run: ./scripts/sync-docs.py --clean
```

**Fix:**
```bash
./scripts/sync-docs.py --clean
```

### Reconcile File Counts

If you see:
```
WARNING: More files in website than expected
Expected public files: 57
Actual files in website: 66
```

**Explanation:** Website has ~9 website-specific files (pages/, guides/, projects/) that aren't in manifest. This is normal.

**If count is significantly higher:** Some files may not be tracked in manifest.

**Find untracked files:**
```bash
cd /home/scottsen/src/projects/sil-website
comm -13 \
  <(grep "path: docs/" ../SIL/docs/CONTENT_MANIFEST.yaml | \
    sed 's/.*path: //' | sed 's/docs\///' | sort) \
  <(find docs -name "*.md" | sed 's|docs/||' | sort)
```

### YAML Syntax Errors

**Symptom:**
```
ERROR: Failed to parse manifest: ...
```

**Common causes:**
1. Unquoted strings with colons (`:`)
2. Inconsistent indentation
3. Missing spaces after `-` in lists

**Fix:**
```bash
# Validate YAML syntax
python3 -c "import yaml; yaml.safe_load(open('docs/CONTENT_MANIFEST.yaml'))"

# If error, check line number and fix indentation/quoting
```

**Tips:**
- Use 2 spaces for indentation
- Quote strings containing `:` or special chars
- Keep structure flat (max 2 levels)

---

## Monthly Audit Checklist

**Schedule:** First Monday of each month
**Owner:** SIL Operations
**Duration:** ~30 minutes

### Pre-Audit

- [ ] Pull latest from SIL repo: `git pull`
- [ ] Check for new files: `find docs -name "*.md" -type f`
- [ ] Review recent commits: `git log --since="1 month ago" --name-only docs/`

### Audit Steps

1. **Find uncategorized files**
   ```bash
   # List all .md files
   find docs -name "*.md" | sort > /tmp/all_docs.txt

   # List files in manifest
   grep "path: docs/" docs/CONTENT_MANIFEST.yaml | \
     sed 's/.*path: //' | sort > /tmp/manifest_docs.txt

   # Find difference
   comm -13 /tmp/manifest_docs.txt /tmp/all_docs.txt
   ```

2. **Review flagged files**
   ```bash
   grep -A5 "flagged_for_removal: true" docs/CONTENT_MANIFEST.yaml
   ```
   - Decide: Delete or keep?
   - Update manifest accordingly

3. **Review under_review files**
   ```bash
   grep -A5 "under_review: true" docs/CONTENT_MANIFEST.yaml
   ```
   - Decide: Public or internal?
   - Update visibility and remove `under_review` flag

4. **Validate counts**
   ```bash
   cd /home/scottsen/src/projects/sil-website
   ./scripts/sync-docs.py --validate
   ```

5. **Check for internal files in website**
   ```bash
   ./scripts/sync-docs.py --validate
   # Should show: ✓ No internal files found
   ```

### Post-Audit

- [ ] Update `last_audit` section in manifest
- [ ] Update `next_audit` date (+1 month)
- [ ] Commit changes: `git commit -am "docs: Monthly manifest audit"`
- [ ] If changes made, re-sync: `cd sil-website && ./scripts/sync-docs.py`

---

## Decision Framework

### When to Make a File Public

**Criteria for `visibility: public`:**
- ✅ Content is polished and production-ready
- ✅ Tone is calibrated (humble, not grandiose)
- ✅ No internal operational details exposed
- ✅ Reflects well on SIL's professionalism
- ✅ Would be useful to external audience
- ✅ No competitive sensitivity

**Examples:**
- Research papers (after peer review)
- System documentation (Reveal, Beth, TIA)
- Glossaries and guides
- Founder background (appropriate tone)

### When to Keep Internal

**Criteria for `visibility: internal`:**
- 🔒 Work-in-progress / draft state
- 🔒 Internal procedures and operations
- 🔒 Strategic planning documents
- 🔒 Quality thresholds and monitoring
- 🔒 Grandiose or inappropriate tone
- 🔒 Implementation details exposing vulnerabilities

**Examples:**
- SIL_RESEARCH_AGENDA_YEAR1.md (strategy)
- SIL_TOOL_QUALITY_MONITORING.md (operations)
- MARKDOWN_STYLE_GUIDE.md (internal standards)
- Notes and WIP documents

### When to Flag for Removal

**Criteria for `flagged_for_removal: true`:**
- ⚠️ Grandiose tone that can't be salvaged
- ⚠️ Duplicate content (better version exists)
- ⚠️ Obsolete / superseded by newer docs
- ⚠️ Content that shouldn't exist publicly

**Process:**
1. Flag in manifest with reason
2. Discuss in next audit
3. Either delete or rehabilitate
4. Update manifest accordingly

---

## Integration with Deployment

### Development Workflow

```bash
# 1. Edit docs in SIL repo
cd /home/scottsen/src/projects/SIL
vim docs/systems/reveal.md

# 2. Commit to SIL repo
git commit -am "docs: Update Reveal documentation"

# 3. Sync to website (if public)
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py

# 4. Verify sync
git status  # Should show updated docs
git diff docs/systems/reveal.md

# 5. Commit synced docs
git commit -am "docs: Sync Reveal updates from SIL repo"

# 6. Deploy to staging
./deploy/deploy-container.sh staging

# 7. Verify staging
curl https://sil-staging.mytia.net/systems/reveal
```

### Pre-Deployment Checklist

Before deploying to production:

- [ ] Run validation: `./scripts/sync-docs.py --validate`
- [ ] Verify no internal files: Should show `✓ No internal files found`
- [ ] Check file counts: Should be ~57 public + ~9 website-specific
- [ ] Test staging site: All public links work
- [ ] Test internal blocks: Internal doc URLs return 404

---

## FAQ

### Q: Can I edit docs directly in sil-website repo?

**A: NO.** Always edit in `/projects/SIL/docs/` then sync.

**Why?** SIL repo is source of truth. Changes in website repo will be overwritten on next sync.

**Exception:** Website-specific files (pages/, guides/) that aren't synced from SIL repo.

### Q: What if I accidentally committed internal doc to public?

**A: Fix immediately:**
```bash
cd /home/scottsen/src/projects/sil-website

# 1. Remove file
git rm docs/meta/INTERNAL_DOC.md

# 2. Commit removal
git commit -m "fix: Remove internal doc accidentally synced"

# 3. Deploy fix
./deploy/deploy-container.sh production

# 4. Update manifest to ensure it's marked internal
cd /home/scottsen/src/projects/SIL
vim docs/CONTENT_MANIFEST.yaml
# Verify visibility: internal

# 5. Run clean to catch any others
cd /home/scottsen/src/projects/sil-website
./scripts/sync-docs.py --clean
```

### Q: How do I add a completely new category?

**Example:** Adding `docs/tutorials/`

```bash
# 1. Create directory in SIL repo
cd /home/scottsen/src/projects/SIL
mkdir docs/tutorials

# 2. Add files
vim docs/tutorials/README.md

# 3. Add all files to manifest
vim docs/CONTENT_MANIFEST.yaml
```

Add entries:
```yaml
  # Tutorials (3 files)
  - path: docs/tutorials/README.md
    visibility: public
    purpose: Tutorials index

  - path: docs/tutorials/getting-started.md
    visibility: public
    purpose: Getting started tutorial
```

### Q: The sync script shows warnings about missing categories?

**Example:**
```
WARNING: category_path_not_found: canonical, tools, innovations
```

**A: This is normal.** The website content loader looks for certain standard categories. If they don't exist, it logs a warning but continues. Not an error.

**To suppress:** Either create the directories or ignore the warnings.

### Q: What's the difference between manifest and .gitignore?

| Feature | Manifest | .gitignore |
|---------|----------|------------|
| **Purpose** | Controls sync (SIL → website) | Controls git tracking |
| **Approach** | Allowlist (opt-in) | Denylist (opt-out) |
| **Visibility** | Visible, documented | Hidden dotfile |
| **Rationale** | Explicit with reasons | Implicit patterns |
| **Scope** | SIL repo files only | Any file in repo |

**Use both:** Manifest controls sync, .gitignore controls git.

### Q: How do I know what files are currently public?

```bash
cd /home/scottsen/src/projects/SIL

# List all public files
python3 << 'EOF'
import yaml
with open('docs/CONTENT_MANIFEST.yaml') as f:
    m = yaml.safe_load(f)
    public = [f['path'] for f in m['files'] if f.get('visibility') == 'public']
    for p in sorted(public):
        print(p)
EOF
```

Or check the website:
```bash
cd /home/scottsen/src/projects/sil-website
find docs -name "*.md" | grep -v pages/ | grep -v guides/ | sort
```

---

## Version History

| Version | Date | Session | Changes |
|---------|------|---------|---------|
| 1.0 | 2025-12-16 | charcoal-jewel-1216 | Initial comprehensive guide |

---

## See Also

- **CONTENT_MANIFEST.yaml** - The manifest itself
- **WEBSITE_DEPLOYMENT_GUIDE.md** - Full deployment workflow
- **WEBSITE_DEPLOYMENT_AUDIT.md** - Audit findings and history
- **README_SYNCED_CONTENT.md** - Website repo sync documentation
