# GitHub Pages Setup

This repository uses GitHub Pages to render HTML mockups as interactive web pages.

## Enable GitHub Pages

**One-time setup** (repository owner only):

1. Go to repository settings:
   https://github.com/diarized/KrakowBites_changelog/settings/pages

2. Under **"Source"**:
   - Select: **Deploy from a branch**

3. Under **"Branch"**:
   - Select: **master**
   - Directory: **/ (root)**

4. Click **Save**

5. Wait ~1 minute for deployment

## Verify Setup

Once enabled, HTML files are accessible at:

```
https://diarized.github.io/KrakowBites_changelog/<file-path>
```

**Example**:
- Repository URL (source): https://github.com/diarized/KrakowBites_changelog/blob/master/docs/brand-alternatives/urban-cartography-mockup.html
- GitHub Pages URL (rendered): https://diarized.github.io/KrakowBites_changelog/docs/brand-alternatives/urban-cartography-mockup.html

## File Types

| File Type | Where to View | URL Type |
|-----------|---------------|----------|
| `.md` (Markdown) | GitHub repository | Auto-rendered on github.com |
| `.html` (HTML mockups) | GitHub Pages | Rendered as web page on github.io |
| Other files | GitHub repository | Source view on github.com |

## Status

Check GitHub Pages deployment status:
https://github.com/diarized/KrakowBites_changelog/deployments

## Troubleshooting

**HTML file shows source code instead of rendering:**
- GitHub Pages not enabled → Follow setup instructions above
- Deployment pending → Wait 1-2 minutes, refresh page
- Wrong URL → Ensure using `github.io` not `github.com`

**404 Not Found:**
- File doesn't exist in repository
- Typo in URL path
- GitHub Pages deployment failed (check deployments page)

## Notes

- Deployment is automatic after pushing to master branch
- Changes appear within ~1 minute
- All files in repository are publicly accessible
- SSL/HTTPS enabled by default
- No build step required (static files served directly)
