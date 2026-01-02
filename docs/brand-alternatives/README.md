# Brand Mockup Hosting

## Live URL

The Urban Cartography brand mockup is live at:

**https://krakowbites.stonith.pl/brand/urban-cartography-mockup.html**

Share this URL with clients to showcase the brand identity concept.

## How It Works

**Architecture**:
- Apache serves `/brand/*` paths directly from `/var/www/krakowbites-static/brand/`
- Static files bypass the Node.js/Astro proxy for fast delivery
- No rebuild required - instant updates

**Configuration**:
- Apache Alias directive in `/etc/apache2/sites-available/krakowbites.stonith.pl-le-ssl.conf`
- `ProxyPass /brand !` excludes `/brand/*` from proxy
- Files served with proper MIME types and caching headers

## Uploading New Mockups

**Method 1: Helper Script**
```bash
./scripts/upload-brand-mockup.sh docs/brand-alternatives/your-mockup.html
```

The script will output the live URL automatically.

**Method 2: Manual Upload**
```bash
# Copy file to server
scp your-mockup.html trzeci:/tmp/

# Move to static directory with correct ownership
ssh trzeci "sudo mv /tmp/your-mockup.html /var/www/krakowbites-static/brand/ && \
            sudo chown www-data:www-data /var/www/krakowbites-static/brand/your-mockup.html && \
            sudo chmod 644 /var/www/krakowbites-static/brand/your-mockup.html"
```

**Accessible at**: `https://krakowbites.stonith.pl/brand/your-mockup.html`

## Directory Structure

```
Server: trzeci.stonith.pl
├─ /var/www/krakowbites/              # Astro application
│  ├─ src/                             # Astro source files
│  └─ dist/                            # Astro build output
│
└─ /var/www/krakowbites-static/       # Static files (served directly by Apache)
   └─ brand/                           # Brand mockups and assets
      └─ urban-cartography-mockup.html
```

## Apache Configuration

**File**: `/etc/apache2/sites-available/krakowbites.stonith.pl-le-ssl.conf`

```apache
# Serve static mockups directly from Apache
Alias /brand /var/www/krakowbites-static/brand
<Directory /var/www/krakowbites-static/brand>
    Require all granted
    Options -Indexes +FollowSymLinks
    AllowOverride None
</Directory>

# Proxy everything else to Node.js/Astro
ProxyPreserveHost On
ProxyPass /brand !
ProxyPass / http://127.0.0.1:3000/
ProxyPassReverse / http://127.0.0.1:3000/
```

**Key Points**:
- `Alias /brand` maps URL path to filesystem directory
- `ProxyPass /brand !` excludes `/brand/*` from proxy (exclamation mark = exception)
- Order matters: Alias must come before ProxyPass

## Use Cases

**Client Presentations**:
- Share live URL instead of PDF attachments
- Client can view on any device with browser
- No email size limits or download requirements

**Design Iterations**:
- Upload new versions instantly
- No Astro rebuild needed
- Keep version history (e.g., `mockup-v1.html`, `mockup-v2.html`)

**A/B Testing**:
- Upload multiple variants
- Share different URLs for comparison
- Client can switch between options easily

## File Permissions

**Requirements**:
- Owner: `www-data:www-data`
- Permissions: `644` (read for all, write for owner)
- Directory: `755` (executable for traversal)

**Verify permissions**:
```bash
ssh trzeci "ls -la /var/www/krakowbites-static/brand/"
```

## Troubleshooting

**404 Not Found**:
- Check file exists: `ssh trzeci "ls /var/www/krakowbites-static/brand/"`
- Check permissions: `ssh trzeci "ls -l /var/www/krakowbites-static/brand/your-file.html"`
- Verify Apache config: `ssh trzeci "sudo apache2ctl configtest"`

**403 Forbidden**:
- Fix permissions: `ssh trzeci "sudo chmod 644 /var/www/krakowbites-static/brand/your-file.html"`
- Fix ownership: `ssh trzeci "sudo chown www-data:www-data /var/www/krakowbites-static/brand/your-file.html"`

**Wrong MIME type**:
- Apache auto-detects `.html` files as `text/html`
- For other types, check `/etc/mime.types`

**Cache issues**:
- Force refresh: Ctrl+Shift+R (Chrome/Firefox)
- Clear browser cache
- Check Last-Modified header: `curl -I https://krakowbites.stonith.pl/brand/your-file.html`

## Security

**Considerations**:
- No directory listing (Options -Indexes)
- No .htaccess overrides (AllowOverride None)
- Read-only access for world (644 permissions)
- SSL/TLS encryption (HTTPS only)

**Best Practices**:
- Don't upload files with sensitive data
- Use descriptive filenames (not `secret-mockup.html`)
- Remove old mockups when no longer needed
- Keep this directory for temporary client presentations only

## Cleanup

**Remove old mockups**:
```bash
ssh trzeci "sudo rm /var/www/krakowbites-static/brand/old-mockup.html"
```

**List all uploaded files**:
```bash
ssh trzeci "ls -lh /var/www/krakowbites-static/brand/"
```

---

**Created**: 2026-01-01
**Purpose**: Host brand identity mockups for client presentations
**Maintainer**: Infrastructure team
