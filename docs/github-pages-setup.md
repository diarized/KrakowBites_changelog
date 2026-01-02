# GitHub Pages Setup for KrakowBites_changelog

Enable GitHub Pages to render HTML mockups as interactive web pages in the public changelog repository.

## Why GitHub Pages?

**Problem**: GitHub shows HTML files as source code by default (not rendered)

**Solution**: GitHub Pages renders HTML files as actual web pages

**Result**:
- Markdown files: Already render beautifully on github.com (no setup needed)
- HTML mockups: Render as interactive web pages on github.io (requires GitHub Pages)

## Setup Instructions

**One-time setup** (5 minutes):

1. **Navigate to repository settings**:
   ```
   https://github.com/diarized/KrakowBites_changelog/settings/pages
   ```

2. **Configure Pages source**:
   - Under "Source": Select **Deploy from a branch**
   - Under "Branch":
     - Select: **master**
     - Directory: **/ (root)**
   - Click **Save**

3. **Wait for deployment**:
   - Initial deployment takes ~1 minute
   - Check status: https://github.com/diarized/KrakowBites_changelog/deployments

4. **Verify setup**:
   - Visit: https://diarized.github.io/KrakowBites_changelog/docs/brand-alternatives/urban-cartography-mockup.html
   - Should render as interactive webpage (not source code)

## URL Structure

After GitHub Pages is enabled:

| File Type | View Location | URL Pattern |
|-----------|---------------|-------------|
| Markdown (`.md`) | GitHub repository | `https://github.com/diarized/KrakowBites_changelog/blob/master/docs/...` |
| HTML (`.html`) | GitHub Pages | `https://diarized.github.io/KrakowBites_changelog/docs/...` |

**Example - Urban Cartography Mockup**:
- Source code: https://github.com/diarized/KrakowBites_changelog/blob/master/docs/brand-alternatives/urban-cartography-mockup.html
- Rendered page: https://diarized.github.io/KrakowBites_changelog/docs/brand-alternatives/urban-cartography-mockup.html ✓

## Automatic URL Mapping

The `sync-changelog.sh` script automatically:
1. Detects HTML files by extension (`.html`, `.htm`)
2. Links HTML files to GitHub Pages URLs (rendered)
3. Links Markdown files to GitHub repository URLs (source view)
4. Adds 🌐 indicator next to HTML file links

**Example README.md entry**:
```markdown
## 2026-01-01

**Add brand identity critical review**

- [brand-identity-review.md](https://github.com/.../blob/master/docs/brand-identity-review.md)
- [urban-cartography-mockup.html](https://diarized.github.io/.../docs/brand-alternatives/urban-cartography-mockup.html) 🌐
```

## Client Workflow

**Viewing documentation**:
1. Go to: https://github.com/diarized/KrakowBites_changelog
2. Click on any Markdown file → GitHub renders it automatically ✓

**Viewing HTML mockups**:
1. Go to: https://github.com/diarized/KrakowBites_changelog
2. Click on README.md
3. Find HTML file link (marked with 🌐)
4. Click link → Opens rendered web page on github.io ✓

## Deployment Process

**Automatic deployment**:
- Triggers: Every push to master branch
- Duration: ~1 minute
- Status: https://github.com/diarized/KrakowBites_changelog/deployments

**Workflow**:
```bash
# 1. Update docs in main repo
vim docs/new-mockup.html

# 2. Commit to private repo
git commit -am "Add new mockup"

# 3. Sync to public changelog
/project:sync-changelog

# 4. GitHub Pages deploys automatically
# Wait ~1 minute, then share URL with client
```

## Troubleshooting

### HTML shows source code instead of rendering

**Cause**: GitHub Pages not enabled or deployment pending

**Solution**:
1. Check Pages enabled: https://github.com/diarized/KrakowBites_changelog/settings/pages
2. Check deployment status: https://github.com/diarized/KrakowBites_changelog/deployments
3. Wait 1-2 minutes after push
4. Hard refresh browser (Ctrl+Shift+R)

### 404 Not Found

**Cause**: File doesn't exist or wrong URL

**Solution**:
1. Verify file exists in repo: https://github.com/diarized/KrakowBites_changelog
2. Check URL spelling (case-sensitive)
3. Ensure using `github.io` not `github.com` for HTML files
4. Verify GitHub Pages deployment succeeded

### Changes not appearing

**Cause**: Browser cache or deployment delay

**Solution**:
1. Wait 1-2 minutes after push
2. Hard refresh: Ctrl+Shift+R (Chrome/Firefox)
3. Check git log to verify changes pushed: `cd /home/artur/Scripts/WWW/KrakowBites_changelog && git log -1`
4. Check deployment status for errors

## Security & Privacy

**Public repository**:
- ✅ All files in `docs/` are publicly accessible
- ✅ Source code remains private (separate repository)
- ✅ Only deliverables and documentation are published
- ✅ SSL/HTTPS enabled by default on GitHub Pages

**What's public**:
- Documentation (`.md` files)
- HTML mockups (`.html` files)
- Images and assets in `docs/`
- Git history of changelog repository

**What's private**:
- Source code (main KrakowBites repository)
- Infrastructure details (unless explicitly published to docs/)
- Development workflow
- Git history of main repository

## Verification Checklist

After setup, verify:

- [ ] GitHub Pages enabled in settings
- [ ] Deployment successful (check deployments page)
- [ ] HTML mockup renders: https://diarized.github.io/KrakowBites_changelog/docs/brand-alternatives/urban-cartography-mockup.html
- [ ] README.md accessible: https://github.com/diarized/KrakowBites_changelog
- [ ] Markdown files render on GitHub
- [ ] HTML links in README.md have 🌐 indicator
- [ ] Clicking 🌐 link opens rendered page (not source)

## References

- GitHub Pages Documentation: https://docs.github.com/en/pages
- Repository Settings: https://github.com/diarized/KrakowBites_changelog/settings/pages
- Deployment Status: https://github.com/diarized/KrakowBites_changelog/deployments
- Public Changelog: https://github.com/diarized/KrakowBites_changelog

---

**Last Updated**: 2026-01-02
**Status**: Ready for setup
**Setup Time**: ~5 minutes
**Maintenance**: Automatic (no ongoing work required)
