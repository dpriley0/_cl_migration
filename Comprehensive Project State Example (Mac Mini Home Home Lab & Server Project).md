# Complete Session Summary: Mac Mini Home Server Project

---

## 1. The Big Picture - Your Vision

**End Goal:** Build a comprehensive DIY personal cloud infrastructure centered around your 2018 Mac Mini server to replace commercial cloud services, prioritizing privacy and preventing companies from profiting from your personal data.

**Core Philosophy:**

- Self-hosted solutions for maximum privacy and control
- Long-term efficiency over immediate ease of use
- Understanding fundamental systems rather than relying on abstractions
- Mobile-first approach (systems must work on iPhone since that's where you actually use them)

**Why This Matters:**

- Working at NASA with strict IT restrictions (web-only cloud access)
- Self-hosted solutions provide full access from anywhere
- Learning opportunity to understand infrastructure deeply
- Cost savings (eliminate AWS, cloud storage subscriptions, etc.)

---

## 2. All Apps/Tools/Services You've Shown Interest In

### Currently Deployed ✅

- **volkyra.com** - Personal website (migrated from AWS to Cloudflare Pages)
- **test.toomanytabs.space** - Test page via Cloudflare Tunnel
- **Cloudflare Tunnel** - Secure remote access (running in Docker)
- **Docker** - Container platform for services
- **Nginx** - Web server (for test page)

### High Priority - Mentioned Multiple Times

- **Paperless-ngx** - Document management with OCR, tag-based organization
- **Plex** - Media server for streaming your movie/TV collection
- **Nextcloud** - Self-hosted file sync/cloud storage (Dropbox replacement)
- **Immich** - Photo management (self-hosted Google Photos alternative) - PERFECT for your ~1,000 architectural inspiration photos
- **Logseq** - Knowledge management / PKM, want self-hosted sync server

### Tools & Workflows

- **Supernote A6 X2** - E-ink tablet for analog note-taking

- Recently purchased

- Planning OCR automation to flow handwritten notes into Logseq

- Following 7-day learning plan

- Testing WebDAV sync, OCR quality, export formats

- **jj (Jujutsu)** - Version control (preference over Git)

- **Mercurial** - Also using, want to host repositories

- **Nushell** - Primary shell

### Future Infrastructure

- **DIY NAS** - RAID-Z1, BTRFS/ZFS filesystems, SMS/email alerts

- **Backups** - Local + encrypted offsite (Backblaze B2 with restic)

- **Cloudflare Access** - Authentication using WebAuthn/passkeys

- Face ID for you (iPhone)

- Fingerprint for your wife (Android)

- **Version control hosting** - For both jj and Mercurial repositories

### Web Projects (Mentioned)

1. Dashboard/admin interface for managing services
2. Home lab documentation site
3. Photo gallery frontend (with Immich backend)
4. Timecard app for work
5. **volkyra.com dynamic features** (near future):
- Resume mailer / contact form
- Personal book library database with interactive functions

### Other Services Mentioned

- **Syncthing** - File sync alternative to Nextcloud (you considered both)

### Tools/Platforms Used

- **Proton VPN** - Premium account (caused network issues with WireGuard extension)
- **Proton Mail Bridge** - Email integration
- **Proton Pass** - Password manager
- **Proton Drive** - File storage
- **Claude Code** - AI-assisted coding (installed via Homebrew, OAuth authenticated)
- **Homebrew** - Package manager
- **Spaceship.com** - Domain registrar (toomanytabs.space, volkyra.com)
- **Cloudflare** - DNS, CDN, Pages, Tunnel, Workers (free tier)
- **GitHub** - Repository hosting (web_lmr for volkyra.com)

---

## 3. How Integral Is AstralJunction to Your System?

**CRITICAL - It's the Foundation of Everything**

### What AstralJunction Provides:

**Core Infrastructure:**

- Central hub for all self-hosted services
- Eliminates dependency on commercial cloud providers
- Provides NASA firewall-friendly access (everything via HTTPS)
- Enables privacy-first approach (your data never leaves your control)

**Specific Integrations:**

1. **Document Management** (Paperless-ngx on Mac Mini)
- OCR scanned documents

- Organize by tags (not folders)

- Full-text search

- Accessible from anywhere via tunnel
3. **Knowledge Management** (Logseq sync on Mac Mini)
- Central sync server for notes across devices

- Supernote → OCR → Logseq workflow automation

- Works on iPhone, MacBook, work computer (web access)
5. **Photo Management** (Immich on Mac Mini)
- Nearly 1,000 architectural inspiration photos

- AI-powered search and organization

- Mobile-first (upload from iPhone while browsing social media)

- Better than desktop-only solutions
7. **Media Streaming** (Plex on Mac Mini)
- Access your media library anywhere

- Share with family

- No cloud storage needed
9. **File Sync** (Nextcloud on Mac Mini)
- Self-hosted Dropbox replacement

- Works with NASA restrictions (web access)

- Calendar, contacts integration
11. **Development Workflow:**
- Version control hosting (jj, Mercurial, Git)
- Python automation scripts
- Custom web applications
- Test environments

**Why Mac Mini Specifically:**

- Always-on server (headless operation)
- Low power consumption
- Powerful enough (i7-8700B, 32GB RAM)
- Quiet, reliable
- Already owned hardware (no ongoing cloud costs)

**The Alternative (Without AstralJunction):**

- AWS/cloud costs: ~$50-200/month for equivalent services
- Multiple vendor dependencies (Dropbox, Google Photos, etc.)
- Limited control over data
- Vendor lock-in
- Services can change/shut down
- Data mining by companies

**With AstralJunction:**

- One-time hardware cost
- Electricity: ~$5-10/month
- Complete control
- Privacy guaranteed
- Learn valuable infrastructure skills
- No vendor lock-in

---

## 4. Current State of AstralJunction

### Hardware

- **Model:** 2018 Mac Mini (Intel)
- **CPU:** Intel Core i7-8700B (6-core)
- **RAM:** 32GB
- **Storage:** 128GB SSD (planning to add DIY NAS for expanded storage)
- **Network:** Gigabit Ethernet (connected, not WiFi)
- **Static IP:** 192.168.70.70 (via DHCP reservation in TP-Link Deco mesh router)
- **MAC Address (Ethernet):** 14:9D:99:81:E9:F2

### Network Configuration

- **Router:** TP-Link Deco mesh system
- **Subnet:** 192.168.68.x/22 (255.255.252.0)
- **Router IP:** 192.168.68.1
- **Bonjour Hostname:** AstralJunction.local
- **Remote Access:** Screen Sharing (VNC) at vnc://192.168.70.70

### macOS Configuration

- **Computer Name:** AstralJunction
- **Username:** dpriley
- **Hostname:** dan@AstralJunction
- **Remote Login:** Enabled (SSH daemon running)
- **Auto-login:** Enabled (for automatic recovery after power outages)
- **Sleep:** Disabled (display and system stay awake 24/7)
- **Peripherals:** Disconnected (headless operation)

### Software Installed

- **macOS:** Headless operation, accessed via Screen Sharing from MacBook
- **Homebrew:** v5.0.4 (Intel Mac location: /usr/local/bin/brew)
- **Docker Desktop:** v29.0.1 (auto-start on login enabled)
- **Cloudflared:** v2025.11.1 (Cloudflare Tunnel client)
- **Shell:** Nushell v0.109.1 (primary shell)
- **Version Control:** jj (Jujutsu), Mercurial, Git available

### Active Services (Docker Containers)

1. **cloudflared**
- Status: Running, auto-restart enabled

- Config: `/Users/dpriley/.cloudflared/config.yml`

- Credentials: `/Users/dpriley/.cloudflared/93694f44-5a63-49ef-bbd8-ace63963fb46.json`

- Tunnel Name: astraljunction

- Tunnel ID: 93694f44-5a63-49ef-bbd8-ace63963fb46

- Routes: test.toomanytabs.space → [http://host.docker.internal:8080](http://host.docker.internal:8080)
3. **test-webserver** (nginx:alpine)
- Status: BROKEN
- Port: 8080
- Volume: ~/test-site mounted to /usr/share/nginx/html
- Serves: Purple gradient "AstralJunction Online!" test page

### Cloudflare Configuration

- **Account:** [Danril3y@gmail.com](mailto:Danril3y@gmail.com "mailto:Danril3y@gmail.com")

- **Domains:**

- **toomanytabs.space** (practice/test domain)

- Nameservers: gannon.ns.cloudflare.com, mckinley.ns.cloudflare.com

- AI crawler blocking: Enabled

- DNS: test.toomanytabs.space → Cloudflare Tunnel

- **volkyra.com** (production domain)

- Nameservers: Same as above

- Cloudflare Pages deployment: web-lmr

- Custom domains: volkyra.com, www.volkyra.com (both active)

- Source: GitHub repository dpriley0/web_lmr

### Deployment Workflow

- **Git:** Using jj (Jujutsu) for version control
- **Repository:** https://github.com/dpriley0/web_lmr.git
- **Main bookmark:** main
- **Deployment:** Automatic via Cloudflare Pages (detects GitHub pushes, redeploys within 30 seconds)
- **Preview deployments:** Enabled for non-main branches

### Known Issues 🔴

**CRITICAL: SSH from MacBook Doesn't Work**

- **Issue:** macOS 26.2 (Tahoe) networking bug on MacBook

- **Error:** "No route to host" when trying to SSH from MacBook to Mac Mini

- **Affected:** Only MacBook → Mac Mini SSH

- **Working:**

- ✅ PC → Mac Mini SSH (works perfectly)

- ✅ Screen Sharing (VNC) from MacBook (works perfectly)

- ✅ Mac Mini can ping MacBook

- ✅ All other network services

- **Cause:** macOS 26.2 kernel-level networking bug (not your fault!)

- **Tested Extensively:**

- Routing tables: Correct

- ARP cache: Correct

- Firewall: Disabled

- PF (Packet Filter): Disabled

- ProtonVPN WireGuard extension: Removed

- IPv6: Also fails

- Everything configuration-wise is correct

- **Workarounds:**

- Use Screen Sharing (vnc://192.168.70.70) → Terminal on Mac Mini

- Termius (installed on iPhone and MacBook)

- Use PC for SSH

- Wait for macOS 26.3 (expected late January 2026)

- **Conclusion:** This is an Apple bug, not something you can fix. Frustrating but manageable with Screen Sharing.

**Other Notes:**

- ProtonVPN caused initial networking issues (WireGuard system extension interfering)

- Resolved by killing the extension

- ProtonVPN disabled on startup

- "Allow LAN" setting exists but didn't work properly

### Architecture Patterns Established

**Two-Tier Hosting Strategy:**

1. **Static Sites → Cloudflare Pages**
- volkyra.com, www.volkyra.com

- Professional hosting, global CDN

- No home IP exposure

- Free tier

- Git-based deployment

- Future: Add Cloudflare Workers for dynamic features
3. **Dynamic Services → Mac Mini via Tunnel**
- test.toomanytabs.space (current)
- Future: paperless.toomanytabs.space, plex.toomanytabs.space, etc.
- Full control over backend
- Docker containers
- Cloudflare Tunnel hides home IP
- Free tier

**Key Technical Decisions:**

- Use `host.docker.internal` instead of `localhost` in tunnel configs (Docker networking)
- Separate database containers per application (isolation benefits)
- Integrated solutions over complexity (Cloudflare Pages over GitHub Pages)
- Mobile-first for adoption (Immich over desktop-only tools)

### Files & Directories Structure

/Users/dpriley/├── .cloudflared/│   ├── config.yml (tunnel configuration)│   ├── 93694f44-5a63-49ef-bbd8-ace63963fb46.json (tunnel credentials)│   └── cert.pem (authentication certificate)├── test-site/│   └── index.html (purple gradient test page)└── [Future: service directories for Paperless-ngx, Plex, etc.]

### What's Working Right Now ✅

1. Mac Mini headless server operational
2. Remote access via Screen Sharing
3. Docker containers running
4. Cloudflare Tunnel active (cloudflared in Docker)
5. ~~test.toomanytabs.space~~ ⁠BROKEN
6. volkyra.com deployed on Cloudflare Pages
7. www.volkyra.com working
8. Auto-restart for all containers
9. Git workflow established with jj

### Ready to Deploy 🚀

- Paperless-ngx
- Plex
- Nextcloud
- Immich
- Logseq sync
- Any other Docker-based service

**Pattern is established:**

1. Start Docker container on a port
2. Add to tunnel config
3. Restart cloudflared
4. Access at subdomain.toomanytabs.space

---

## Additional Context

### Your Work Environment

- **Employer:** NASA
- **Job:** Rocket engine simulation software (ROCETS - ROCket Engine Transient Simulator)
- **Language:** Fortran-based (13 years experience, limited fluency)
- **IT Restrictions:** Cloud services limited to web-only access
- **This matters because:** Self-hosted solutions with web access work perfectly with NASA's firewall

### Your Technical Background

- **Aerospace Engineer**
- **Python fluency:** 3.0-3.5 out of 5 (self-assessed)
- **New-ish to OOP paradigm**
- **Learning style:** Visuals, real-life examples, and metaphors helpful
- **Preference:** Wants to understand systems deeply, not just use them

### Personal Projects

- **Custom House Design:** Queen Anne Victorian with wife
- **Architecture Photos:** ~1,000 inspiration photos needing organization
- **Supernote Integration:** New workflow for analog → digital notes

### Preferences for Responses

- TL;DR at the start of complex responses
- TL;DR for each section in multi-section responses
- Reprint instructions after clarifying questions/debugging
- Very specific about which machine to run commands on
- Appreciates clarity on SSH vs Screen Sharing
- Prefers understanding "why" not just "how"

---

## What We Accomplished Today

1. ✅ Configured Mac Mini as headless server
2. ✅ Set up static IP via DHCP reservation
3. ✅ Enabled remote access (SSH + VNC)
4. ✅ Installed Homebrew, Docker, Cloudflared
5. ✅ Migrated volkyra.com from AWS to Cloudflare Pages (saving $$)
6. ✅ Cleaned up AWS resources (S3, Route 53)
7. ✅ Created Cloudflare Tunnel (astraljunction)
8. ✅ Deployed test page accessible globally
9. ✅ Configured cloudflared to run in Docker (auto-restart)
10. ✅ Debugged DNS, networking, and configuration issues
11. ✅ Established deployment patterns for future services
12. ✅ Learned Docker basics, DNS configuration, tunnel setup
13. ✅ Documented architecture and workflows

---

## Copy This to Your New Conversation!

When you start fresh, you can reference:

- "We set up my Mac Mini (AstralJunction) as a home server"
- "Cloudflare Tunnel is running in Docker"
- "Ready to deploy services like Paperless-ngx"
- "SSH from MacBook doesn't work (macOS 26.2 bug), use Screen Sharing"

**I'll be able to search this conversation for details if needed!** 🎉

---

**Good luck with your next steps! This foundation you've built is solid - everything from here is just adding more services using the same patterns.** 🚀
