# Server Analysis: trzeci.stonith.pl

**Date:** 2026-01-01
**Purpose:** Assess existing infrastructure for hosting KrakowBites alongside blog.stonith.pl
**Server:** trzeci.stonith.pl (OVH VPS)

---

## Current Setup Summary

### Server Specifications

| Component | Status | Details |
|-----------|--------|---------|
| **OS** | ✅ Debian 13 | Kernel 6.12.48, x86_64 |
| **CPU** | ✅ Available | Cloud VPS (QEMU) |
| **RAM** | ✅ 3.7 GB | 3.3 GB available, 469 MB used |
| **Disk** | ✅ 79 GB | 72 GB free (5% used) |
| **Swap** | ❌ None | Should add for PostgreSQL |
| **Node.js** | ✅ v24.12.0 | Latest LTS, perfect for Astro |
| **npm** | ✅ v11.6.2 | Latest version |
| **Apache** | ✅ Running | Version not checked, serving blog.stonith.pl |
| **PostgreSQL** | ❌ Not installed | **REQUIRED for KrakowBites** |

---

## Existing Blog Setup (blog.stonith.pl)

### Architecture

**Type:** Static site (Astro SSG - Static Site Generation)
- **Source:** `/home/debian/astro-blog/`
- **Build output:** `/home/debian/astro-blog/dist/`
- **Apache DocumentRoot:** `/var/www/blog.stonith.pl/`
- **Deployment:** Manual `rsync` or `cp` after build

### Astro Configuration

```javascript
// ~/astro-blog/astro.config.mjs
export default defineConfig({
	site: 'https://blog.stonith.pl',
	integrations: [mdx(), sitemap()],
	// Note: No output: 'server', so this is STATIC build
});
```

**Dependencies:**
- `astro@5.16.6` (latest)
- `@astrojs/mdx` - Markdown support
- `@astrojs/sitemap` - SEO
- `@astrojs/rss` - RSS feed
- `sharp` - Image optimization

### Apache VirtualHost Configuration

```apache
# /etc/apache2/sites-enabled/blog.stonith.pl-le-ssl.conf
<VirtualHost *:443>
    ServerName blog.stonith.pl
    DocumentRoot /var/www/blog.stonith.pl

    # Static files only (no reverse proxy)
    <Directory /var/www/blog.stonith.pl>
        Options -Indexes +FollowSymLinks
        DirectoryIndex index.html
    </Directory>

    # SSL via Let's Encrypt
    SSLCertificateFile /etc/letsencrypt/live/blog.stonith.pl/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/blog.stonith.pl/privkey.pem

    # Security headers (comprehensive)
    Header always set Strict-Transport-Security "max-age=31536000"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "DENY"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"
    Header always set Content-Security-Policy "..."
    Header always set Permissions-Policy "geolocation=(), microphone=(), camera=()"

    # Performance (GZIP + caching)
    AddOutputFilterByType DEFLATE text/html text/css application/javascript
    ExpiresActive On
    ExpiresByType text/css "access plus 1 year"
</VirtualHost>
```

**Key observations:**
- ✅ Excellent security headers template
- ✅ GZIP compression enabled
- ✅ Browser caching configured
- ✅ SSL properly configured
- ❌ No proxy modules enabled (needed for KrakowBites)

---

## Apache Modules Status

### Currently Enabled
```
headers_module   ✅ (needed for security headers)
rewrite_module   ✅ (useful for URL rewriting)
ssl_module       ✅ (for HTTPS)
```

### Required but NOT Enabled (for KrakowBites SSR)
```
proxy_module     ❌ MUST ENABLE
proxy_http       ❌ MUST ENABLE
http2            ⚠️ Optional but recommended
```

### Available (ready to enable)
```
/etc/apache2/mods-available/proxy.load          ✅
/etc/apache2/mods-available/proxy_http.load     ✅
/etc/apache2/mods-available/http2.load          ✅
/etc/apache2/mods-available/proxy_http2.load    ✅
```

**Enable with:**
```bash
sudo a2enmod proxy proxy_http http2
sudo systemctl reload apache2
```

---

## Network & Ports

### Listening Ports
```
122   - SSH (non-standard port, good security practice)
80    - HTTP (Apache)
443   - HTTPS (Apache)
53    - DNS (systemd-resolved, local only)
5355  - mDNS (systemd-resolved)
```

### Available for KrakowBites
```
3000  ✅ FREE - Perfect for Node.js Astro SSR app
5432  ✅ FREE - Will be used by PostgreSQL
```

### Firewall
**Status:** ❌ No firewall configured (iptables policy ACCEPT)
**Recommendation:** Consider enabling `ufw` to restrict access to essential ports only

---

## What's Missing for KrakowBites

### 1. PostgreSQL Database ❌ CRITICAL

**Status:** Not installed

**Installation needed:**
```bash
sudo apt-get update
sudo apt-get install postgresql-16 postgresql-client-16
```

**Configuration needed:**
- Create `krakowbites` database
- Create `krakowbites_user` with password
- Configure PostgreSQL for local connections
- Run database migrations
- Setup backup strategy (pg_dump cron)

**Resources required:**
- ~100-200 MB RAM (PostgreSQL)
- ~50-100 MB disk initially (will grow with bookings)

---

### 2. Apache Proxy Modules ❌ REQUIRED

**Status:** Available but not enabled

**Action:**
```bash
sudo a2enmod proxy proxy_http http2
sudo systemctl reload apache2
```

**Purpose:** Reverse proxy traffic to Node.js (port 3000)

---

### 3. systemd Service ❌ REQUIRED

**Status:** Not configured

**Need to create:**
- `/etc/systemd/system/krakowbites.service`
- Environment file with secrets
- Auto-restart configuration
- Logging to journald

**Purpose:** Keep Node.js app running, restart on failure

---

### 4. systemd Timers for Cron Jobs ❌ REQUIRED

**Status:** Not configured

**Need to create:**
1. **Exchange rate updates** - Daily at 2:00 AM
2. **Review request emails** - Daily check for tours completed 24h ago
3. **Database backups** - Daily at 3:00 AM
4. **Tour reminders** - Daily check for tours in 48h

---

### 5. Swap Space ⚠️ RECOMMENDED

**Status:** 0 bytes swap

**Recommendation:** Add 1-2GB swap file for PostgreSQL stability

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

### 6. Additional Packages ⚠️ OPTIONAL

**May be needed:**
```bash
# Image processing (Astro already has sharp)
# But might need system libs for sharp to compile
sudo apt-get install libvips-dev

# Build tools (likely already installed)
sudo apt-get install build-essential

# Git (for deployment, if not installed)
sudo apt-get install git
```

---

## Deployment Strategy Comparison

### Blog (Static) vs KrakowBites (SSR)

| Aspect | Blog (Static) | KrakowBites (SSR) |
|--------|---------------|-------------------|
| **Build output** | HTML/CSS/JS files | Node.js server bundle |
| **Deployment** | `rsync dist/ → /var/www/` | `git pull + npm build + systemd restart` |
| **Apache role** | Serve static files | Reverse proxy to Node.js |
| **Process** | None (static files) | Long-running Node.js (systemd) |
| **Updates** | Copy files, instant | Restart service, ~2-5s downtime |
| **Database** | None | PostgreSQL required |
| **Resources** | Minimal (just disk) | RAM for Node.js + PostgreSQL |

---

## Proposed Directory Structure

```
/home/debian/
├── astro-blog/              # Existing blog (static)
│   ├── dist/                # Build output → /var/www/blog.stonith.pl/
│   └── astro.config.mjs     # Static build
│
└── krakowbites/             # NEW: KrakowBites (SSR)
    ├── dist/
    │   ├── server/          # Node.js SSR server
    │   └── client/          # Static assets (served by Apache)
    ├── uploads/             # Tour images (served by Apache)
    ├── .env.production      # Environment secrets
    └── astro.config.mjs     # SSR build (output: 'server')

/var/www/
├── blog.stonith.pl/         # Existing: blog static files
│   ├── index.html
│   └── _astro/
│
└── krakowbites.com/         # NEW: Static assets only
    ├── _astro/              # Hashed CSS/JS (served by Apache)
    └── uploads/             # Tour images (served by Apache)
    # Note: HTML is rendered by Node.js, not served here

/etc/apache2/sites-available/
├── blog.stonith.pl-le-ssl.conf       # Existing
└── krakowbites.com-le-ssl.conf       # NEW: Reverse proxy config

/etc/systemd/system/
├── krakowbites.service                    # NEW: Node.js app
├── krakowbites-exchange-rates.timer       # NEW: Cron jobs
├── krakowbites-exchange-rates.service
├── krakowbites-backup.timer
└── krakowbites-backup.service
```

---

## Apache Configuration Plan for KrakowBites

### New VirtualHost: krakowbites.com

```apache
<VirtualHost *:443>
    ServerName krakowbites.com
    ServerAlias www.krakowbites.com

    # SSL Configuration (Let's Encrypt)
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/krakowbites.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/krakowbites.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf

    # HTTP/2 for performance
    Protocols h2 http/1.1

    # Security headers (copy from blog config, excellent template)
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"

    # Static assets served directly by Apache (faster)
    Alias /uploads /home/debian/krakowbites/uploads
    Alias /_astro /home/debian/krakowbites/dist/client/_astro

    <Directory /home/debian/krakowbites/uploads>
        Require all granted
        Options -Indexes
        # Cache images for 1 year
        <FilesMatch "\.(jpg|jpeg|png|gif|webp|svg)$">
            Header set Cache-Control "max-age=31536000, public, immutable"
        </FilesMatch>
    </Directory>

    <Directory /home/debian/krakowbites/dist/client/_astro>
        Require all granted
        # Cache hashed assets forever (immutable)
        Header set Cache-Control "max-age=31536000, public, immutable"
    </Directory>

    # Reverse proxy to Node.js for everything else
    ProxyPreserveHost On
    ProxyPass /uploads !
    ProxyPass /_astro !
    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/

    # Compression (for dynamic content from Node.js)
    <IfModule mod_deflate.c>
        AddOutputFilterByType DEFLATE text/html text/plain text/css application/javascript application/json
    </IfModule>

    # Logs
    ErrorLog ${APACHE_LOG_DIR}/krakowbites-error.log
    CustomLog ${APACHE_LOG_DIR}/krakowbites-access.log combined
</VirtualHost>

# HTTP to HTTPS redirect
<VirtualHost *:80>
    ServerName krakowbites.com
    ServerAlias www.krakowbites.com
    Redirect permanent / https://krakowbites.com/
</VirtualHost>
```

---

## SSL Certificate Setup

### For krakowbites.com (when domain is ready)

```bash
# Install certbot if not already installed
sudo apt-get install certbot python3-certbot-apache

# Obtain certificate (Apache plugin, automatic)
sudo certbot --apache -d krakowbites.com -d www.krakowbites.com

# Certbot will:
# 1. Verify domain ownership
# 2. Obtain certificate
# 3. Modify Apache config
# 4. Setup auto-renewal cron

# Verify auto-renewal
sudo certbot renew --dry-run
```

**Note:** Domain must point to trzeci server IP before running certbot

---

## Resource Usage Estimates

### Current (Blog only)
- RAM: ~470 MB used
- Disk: 3.7 GB used (mostly OS + Node.js)
- CPU: Minimal (static files)

### Projected (Blog + KrakowBites)
- RAM: ~1.2 GB total
  - OS/Apache: 400 MB
  - Node.js (Astro SSR): 200-300 MB
  - PostgreSQL: 200-300 MB
  - Buffer: 300 MB
- Disk: ~4.5 GB total
  - OS/packages: 3.7 GB
  - PostgreSQL: 100-200 MB (grows with data)
  - KrakowBites app: 100 MB
  - Tour images: 200-500 MB (grows with tours)
  - Backups: 100 MB (grows, should rotate)
- CPU: Low load (solo guide, <100 bookings/day)

**Server capacity:** ✅ More than sufficient
- 3.7 GB RAM available (3x projected usage)
- 72 GB disk available (16x projected usage)

---

## Implementation Checklist

### Phase 1: Prerequisites (30-45 min)

- [ ] **Install PostgreSQL 16**
  ```bash
  sudo apt-get update
  sudo apt-get install postgresql-16 postgresql-client-16
  ```

- [ ] **Enable Apache proxy modules**
  ```bash
  sudo a2enmod proxy proxy_http http2
  sudo systemctl reload apache2
  ```

- [ ] **Add swap space (optional but recommended)**
  ```bash
  sudo fallocate -l 2G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
  ```

- [ ] **Create PostgreSQL database and user**
  ```bash
  sudo -u postgres psql
  CREATE DATABASE krakowbites;
  CREATE USER krakowbites_user WITH PASSWORD 'strong_password_here';
  GRANT ALL PRIVILEGES ON DATABASE krakowbites TO krakowbites_user;
  \q
  ```

---

### Phase 2: KrakowBites Application (2-3 hours)

- [ ] **Clone/create project**
  ```bash
  cd /home/debian
  git clone <repo> krakowbites
  # OR: Copy local project to server
  ```

- [ ] **Install dependencies**
  ```bash
  cd ~/krakowbites
  npm ci --production
  ```

- [ ] **Configure environment**
  ```bash
  cp .env.example .env.production
  nano .env.production
  # Fill in: DATABASE_URL, PAYU secrets, SMTP config
  ```

- [ ] **Run database migrations**
  ```bash
  npm run db:migrate
  # Creates tours, bookings, reviews tables
  ```

- [ ] **Build application**
  ```bash
  npm run build
  # Outputs to dist/server/ and dist/client/
  ```

- [ ] **Test locally**
  ```bash
  NODE_ENV=production PORT=3000 node dist/server/entry.mjs
  # Visit http://trzeci:3000 (if tunneled) or curl localhost:3000
  ```

---

### Phase 3: systemd Service (30 min)

- [ ] **Create systemd service file**
  ```bash
  sudo nano /etc/systemd/system/krakowbites.service
  # (Use template from technical-architecture.md)
  ```

- [ ] **Set permissions on environment file**
  ```bash
  sudo chmod 640 /home/debian/krakowbites/.env.production
  sudo chown debian:www-data /home/debian/krakowbites/.env.production
  ```

- [ ] **Enable and start service**
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable krakowbites
  sudo systemctl start krakowbites
  sudo systemctl status krakowbites
  ```

- [ ] **Check logs**
  ```bash
  sudo journalctl -u krakowbites -f
  ```

---

### Phase 4: Apache Configuration (30 min)

- [ ] **Create VirtualHost config**
  ```bash
  sudo nano /etc/apache2/sites-available/krakowbites.com.conf
  # (Use template from above)
  ```

- [ ] **Create static asset directories**
  ```bash
  sudo mkdir -p /home/debian/krakowbites/uploads/tours
  sudo chown -R debian:www-data /home/debian/krakowbites/uploads
  ```

- [ ] **Enable site**
  ```bash
  sudo a2ensite krakowbites.com
  sudo apache2ctl configtest
  sudo systemctl reload apache2
  ```

- [ ] **Setup SSL (when domain points to server)**
  ```bash
  sudo certbot --apache -d krakowbites.com -d www.krakowbites.com
  ```

---

### Phase 5: Cron Jobs (systemd timers) (30 min)

- [ ] **Exchange rate updates timer**
  ```bash
  sudo nano /etc/systemd/system/krakowbites-exchange-rates.timer
  sudo nano /etc/systemd/system/krakowbites-exchange-rates.service
  sudo systemctl enable krakowbites-exchange-rates.timer
  sudo systemctl start krakowbites-exchange-rates.timer
  ```

- [ ] **Database backup timer**
  ```bash
  sudo nano /etc/systemd/system/krakowbites-backup.timer
  sudo nano /etc/systemd/system/krakowbites-backup.service
  sudo nano /usr/local/bin/krakowbites-backup.sh
  sudo chmod +x /usr/local/bin/krakowbites-backup.sh
  sudo systemctl enable krakowbites-backup.timer
  sudo systemctl start krakowbites-backup.timer
  ```

- [ ] **Verify timers**
  ```bash
  sudo systemctl list-timers
  ```

---

## Deployment Workflow (After Initial Setup)

### For Blog (Unchanged)
```bash
ssh trzeci
cd ~/astro-blog
# Edit content
npm run build
sudo rsync -av --delete dist/ /var/www/blog.stonith.pl/
sudo chown -R www-data:www-data /var/www/blog.stonith.pl/
```

### For KrakowBites
```bash
ssh trzeci
cd ~/krakowbites

# Pull latest code
git pull origin main

# Install dependencies (if package.json changed)
npm ci --production

# Run migrations (if schema changed)
npm run db:migrate

# Build
npm run build

# Restart service
sudo systemctl restart krakowbites

# Check status
sudo systemctl status krakowbites
sudo journalctl -u krakowbites -n 50

# Verify site is up
curl -I https://krakowbites.com/
```

**Future improvement:** Create a deployment script or git post-receive hook to automate this.

---

## Security Considerations

### Current State
- ✅ SSH on non-standard port 122 (good)
- ✅ SSL properly configured for blog
- ✅ Comprehensive security headers
- ❌ No firewall configured
- ⚠️ Root login via sudo (acceptable for single admin)

### Recommendations for KrakowBites

1. **Enable firewall (ufw)**
   ```bash
   sudo apt-get install ufw
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   sudo ufw allow 122/tcp   # SSH
   sudo ufw allow 80/tcp    # HTTP
   sudo ufw allow 443/tcp   # HTTPS
   sudo ufw enable
   ```

2. **Restrict PostgreSQL**
   - Keep PostgreSQL listening only on localhost (default)
   - No external database access needed

3. **Environment secrets**
   - `.env.production` should be mode 640, owned by debian:www-data
   - Never commit secrets to git
   - Use strong passwords for database

4. **Rate limiting** (optional, future)
   - Apache mod_evasive for DDoS protection
   - Or implement in Node.js app

5. **Fail2ban** (optional)
   - Monitor SSH login attempts
   - Auto-ban repeated failures

---

## Monitoring Strategy

### Application Health
```bash
# Check Node.js process
sudo systemctl status krakowbites

# Check logs
sudo journalctl -u krakowbites -f

# Check database connections
sudo -u postgres psql -c "SELECT count(*) FROM pg_stat_activity WHERE datname='krakowbites';"

# Check disk space
df -h /

# Check memory
free -h
```

### Performance Monitoring
```bash
# Apache access logs
sudo tail -f /var/log/apache2/krakowbites-access.log

# Apache error logs
sudo tail -f /var/log/apache2/krakowbites-error.log

# Node.js uptime
sudo systemctl show krakowbites --property=ActiveState,SubState,MainPID
ps -p <PID> -o etime=
```

### Self-hosted Uptime Monitor (optional)
- Install Uptime Kuma in Docker
- Monitor `/api/health` endpoint

---

## Estimated Timeline

| Phase | Duration | Can Start |
|-------|----------|-----------|
| Install PostgreSQL + enable modules | 30 min | Immediately |
| Add swap space | 10 min | Immediately |
| Initialize KrakowBites project | 1 hour | After PostgreSQL |
| Setup systemd service | 30 min | After build |
| Configure Apache VirtualHost | 30 min | Anytime (parallel) |
| Setup SSL with certbot | 15 min | After domain DNS configured |
| Create systemd timers | 30 min | After app running |
| Testing & verification | 1 hour | Final step |
| **Total** | **~4-5 hours** | **Over 1-2 days** |

---

## Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| **PostgreSQL resource usage** | High RAM consumption | 3.7GB RAM available, PostgreSQL should use <300MB |
| **Port 3000 conflict** | App won't start | Port currently free, verified |
| **systemd service crashes** | Site downtime | Restart=on-failure configured, monitor logs |
| **Disk space (backups)** | Backups fill disk | Rotate backups (keep 30 days), 72GB available |
| **Domain not pointing to server** | Can't get SSL | Test with IP first, add domain later |
| **Apache proxy misconfiguration** | 502 errors | Test config before enabling, use curl for debugging |

---

## Compatibility with Existing Blog

### No Conflicts Expected ✅

| Aspect | Blog | KrakowBites | Conflict? |
|--------|------|-------------|-----------|
| **Ports** | None (static) | 3000 (Node.js) | ✅ No |
| **Apache config** | blog.stonith.pl | krakowbites.com | ✅ Different vhosts |
| **Database** | None | PostgreSQL 5432 | ✅ New service |
| **Disk** | /var/www/blog.stonith.pl | /home/debian/krakowbites | ✅ Separate paths |
| **systemd** | None | krakowbites.service | ✅ New service |
| **Deployment** | rsync static files | git pull + restart | ✅ Different workflows |

**Conclusion:** Blog and KrakowBites can coexist without any interference.

---

## Next Steps

1. **Confirm domain registration** - Is krakowbites.com registered and ready?
2. **Point DNS to trzeci server** - What's the server's public IP?
3. **Install PostgreSQL** - Can start immediately
4. **Enable proxy modules** - Can start immediately
5. **Initialize project** - After PostgreSQL ready
6. **Test deployment** - After domain DNS propagated

**Ready to proceed?** Start with PostgreSQL installation and proxy module enablement while domain DNS propagates.

---

## Questions to Answer

- [ ] What's the trzeci server's public IP address?
- [ ] Is krakowbites.com domain registered?
- [ ] Should we use subdomain (e.g., tours.stonith.pl) instead for testing?
- [ ] Any specific PostgreSQL version preference, or 16 is fine?
- [ ] Should we enable firewall (ufw) or leave open for now?
- [ ] Do you want automated deployment (git hooks) from the start?

---

**Document Status:** Server analysis complete, ready for implementation
**Blocking Issues:** None - server is ready for KrakowBites
**Estimated Effort:** 4-5 hours over 1-2 days
