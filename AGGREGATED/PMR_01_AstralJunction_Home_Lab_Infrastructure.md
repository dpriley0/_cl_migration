# Project Migration Record: AstralJunction Home Lab & Server Infrastructure

---

## I. Comprehensive Project Summary

### 1. Project Vision

#### 1.1 High-Level Goals

Build a comprehensive DIY personal cloud infrastructure centered around a 2018 Mac Mini server ("AstralJunction") to replace commercial cloud services, prioritizing privacy and preventing companies from profiting from personal data.

#### 1.2 Motivations

- **Privacy First**: Take back control of personal data from companies like Google who profit from and mine user data
- **NASA Compatibility**: Working at NASA with strict IT restrictions that limit cloud services to web-only access—self-hosted solutions with HTTPS access work perfectly within these constraints
- **Learning Opportunity**: Understand infrastructure deeply rather than relying on abstractions
- **Cost Savings**: Eliminate AWS, cloud storage subscriptions, and other ongoing costs
- **Long-term Efficiency**: Prioritize understanding and control over immediate ease of use

#### 1.3 Apps, Tools, Utilities, Websites, Services of Interest

**Currently Deployed ✅**

- **volkyra.com** - Personal/wife's website (migrated from AWS to Cloudflare Pages)
- **test.toomanytabs.space** - Test page via Cloudflare Tunnel
- **Cloudflare Tunnel** - Secure remote access (running in Docker on AstralJunction)
- **Docker Desktop** - Container platform for services
- **Nginx** - Web server (for test page)

**High Priority - Planned for Deployment**

- **Paperless-ngx** - Document management with OCR, tag-based organization
- **Plex** - Media server for streaming movie/TV collection
- **Nextcloud** - Self-hosted file sync/cloud storage (Dropbox replacement)
- **Immich** - Photo management (self-hosted Google Photos alternative)
- **Logseq Sync Server** - Knowledge management / PKM sync

**Infrastructure Components**

- **DIY NAS** - RAID-Z1, ZFS filesystem, TrueNAS SCALE (separate project, see PMR #2)
- **Backblaze B2** - Offsite encrypted backups with restic
- **Cloudflare Access** - Authentication using WebAuthn/passkeys (Face ID + fingerprint)
- **Version control hosting** - For both jj and Mercurial repositories

**Web Projects Mentioned**

1. Dashboard/admin interface for managing services
2. Home lab documentation site
3. Photo gallery frontend (with Immich backend)
4. Timecard app for work
5. volkyra.com dynamic features:
   - Resume mailer / contact form
   - Personal book library database with interactive functions

**Tools/Platforms in Use**

- **Proton Services** - VPN (Premium), Mail Bridge, Pass (password manager), Drive
- **Claude Code** - AI-assisted coding (installed via Homebrew, OAuth authenticated)
- **Homebrew** - Package manager
- **Spaceship.com** - Domain registrar (toomanytabs.space, volkyra.com, plus ~8 other domains)
- **Cloudflare** - DNS, CDN, Pages, Tunnel, Workers, Access (all free tier)
- **GitHub** - Repository hosting (web_lmr for volkyra.com)
- **Termius** - SSH client (iPhone and MacBook)

#### 1.4 Strategic Objectives

1. Establish foundation infrastructure for all self-hosted services
2. Maintain zero exposure of home IP address
3. Enable secure remote access from any device (iPhone, MacBook, work computer)
4. Create deployment patterns that are repeatable for future services
5. Document everything for future reference and troubleshooting

#### 1.5 Long-term Potential / Potential Workflows

- **Mobile-first access**: All systems must work well on iPhone since that's where real usage happens
- **Cross-device synchronization**: Files, notes, photos accessible from anywhere
- **Automated workflows**: Supernote → OCR → Logseq pipeline, automatic photo backups
- **Family sharing**: Plex library sharing, Immich photo albums for wife
- **Professional use**: Works seamlessly through NASA firewall restrictions

#### 1.6 Unique Value Proposition

A fully self-hosted infrastructure that provides:

- Complete data ownership and privacy
- NASA firewall compatibility (everything via HTTPS)
- Enterprise-grade security via Cloudflare (free tier)
- Zero ongoing cloud costs beyond electricity (~$5-10/month)
- Deep learning experience with infrastructure

#### 1.7 Core Philosophies Discussed

- **"The best organizational system is the one you'll actually use"** - Mobile-first design is crucial
- **"Long-term efficiency over immediate ease of use"** - Worth investing time to understand systems deeply
- **"Understand fundamental systems rather than relying on abstractions"** - Not vendor-specific knowledge
- **Clean separation between compute and storage** - Services on AstralJunction, bulk storage on future NAS
- **Separate database containers per application** - Isolation benefits for backups, updates, security, debugging

---

### 2. Requirements Compilation

#### 2.1 Explicit Requirements

- Headless server operation (no monitor/keyboard connected)
- Always-on availability (24/7 uptime)
- Remote access from anywhere (home, work, travel)
- HTTPS encryption for all services
- Automatic recovery from power outages (auto-login on boot)
- Docker-based containerized services
- Biometric authentication support (Face ID for Dan, fingerprint for wife)

#### 2.2 Implied Requirements

- Low power consumption (Mac Mini is ideal)
- Quiet operation (home environment)
- Reliable local network connectivity
- Sufficient CPU/RAM for multiple concurrent services
- Expandable storage architecture

#### 2.3 Derived Requirements

- Static IP address on local network (192.168.70.70)
- DHCP reservation rather than manual static IP (easier router management)
- DNS configuration via Cloudflare for all domains
- SSL/TLS certificates managed automatically by Cloudflare
- Backup access methods (VNC if SSH fails)

#### 2.4 Potential Edge Cases / Expansion Areas

- Power outage recovery automation
- ISP IP address changes (Cloudflare Tunnel handles this transparently)
- Service health monitoring and alerting
- Automatic updates for Docker containers
- Disaster recovery procedures

---

### 3. Architectural Overview

#### 3.1 Current Design Decisions with Rationale

**Two-Tier Hosting Strategy:**
| Tier | Use Case | Technology | Rationale |
|------|----------|------------|-----------|
| Static Sites | volkyra.com, documentation | Cloudflare Pages | Professional hosting, global CDN, zero home IP exposure, Git-based deployment |
| Dynamic Services | Paperless-ngx, Plex, etc. | Mac Mini via Cloudflare Tunnel | Full control, Docker containers, hidden home IP |

**Why Cloudflare Tunnel over Port Forwarding:**

- No exposed home IP address
- No open ports on router (security)
- Automatic SSL certificate management
- Built-in DDoS protection
- Works through corporate firewalls (NASA)
- Free tier is sufficient for personal use

**Why Docker over Bare Metal:**

- Service isolation
- Easy deployment and updates
- Reproducible configurations
- Simpler backup/restore
- Consistent environment across services

**Docker Networking Decision:**

- Use `host.docker.internal` for service routing (initially)
- Discovered: Mac Mini's LAN IP (192.168.70.70) works more reliably
- Docker bridge network (172.17.0.1) has connectivity issues with macOS

**Ruled Out Approaches:**

- **GitHub Pages** - Ruled out in favor of Cloudflare Pages (better integration with tunnel)
- **Direct port forwarding** - Security concerns, home IP exposure
- **Self-hosted authentication (Authelia)** - "Don't want to reinvent the wheel" - Cloudflare Access is better fit
- **VPS hosting** - Ongoing costs ($10-50/month), less control, storage costs add up

#### 3.2 Technology Stack

- **Hardware**: 2018 Mac Mini (i7-8700B, 32GB RAM, 128GB SSD)
- **OS**: macOS (headless operation)
- **Container Runtime**: Docker Desktop v29.0.1
- **Tunnel**: Cloudflare Tunnel (cloudflared v2025.11.1)
- **Web Server**: Nginx (Alpine-based Docker image)
- **Shell**: Nushell v0.109.1 (primary)
- **Version Control**: jj (Jujutsu) preferred, Mercurial, Git available
- **Package Manager**: Homebrew v5.0.4

#### 3.3 Frameworks Decided On

- **Cloudflare ecosystem** - Pages, Tunnel, Access, Workers, D1 (database)
- **Docker Compose** - For multi-container service definitions
- **WebAuthn/Passkeys** - For biometric authentication

**Frameworks Ruled Out:**

- **Traefik** - Cloudflare handles reverse proxy needs
- **Let's Encrypt certbot** - Cloudflare handles SSL automatically
- **Wireguard VPN** - Cloudflare Tunnel provides similar functionality without VPN complexity
- **Authelia/Authentik** - Cloudflare Access is simpler and better integrated

#### 3.4 Key Architectural Constraints

- Mac Mini has limited internal storage (128GB SSD) - bulk storage must be external/NAS
- macOS 26.2 (Tahoe) has SSH client bug affecting MacBook → Mac Mini connections
- Must work within NASA's web-only cloud access restrictions
- Wife uses Android (fingerprint auth), Dan uses iPhone (Face ID)

#### 3.5 Potential Future Directions

- Run cloudflared directly on host instead of Docker (simpler debugging)
- Migrate to Linux if macOS limitations become problematic
- Add monitoring with Uptime Kuma or similar
- Implement automated container updates (Watchtower)
- Add second Mac Mini for redundancy

---

### Current State of AstralJunction

#### Hardware Configuration

- **Model**: 2018 Mac Mini (Intel)
- **CPU**: Intel Core i7-8700B (6-core, 12 threads)
- **RAM**: 32GB
- **Storage**: 128GB SSD internal
- **External Storage**: 4TB HDD (media), 750GB SSD (backups) - connected via USB
- **Network**: Gigabit Ethernet (not WiFi - critical!)
- **Static IP**: 192.168.70.70 (via DHCP reservation)
- **MAC Address (Ethernet)**: 14:9D:99:81:E9:F2

#### Network Configuration

- **Router**: TP-Link Deco mesh system
- **Subnet**: 192.168.68.x/22 (255.255.252.0)
- **Router IP**: 192.168.68.1
- **Public IP**: 136.53.106.2
- **Bonjour Hostname**: AstralJunction.local
- **Remote Access**: Screen Sharing (VNC) at vnc://192.168.70.70

#### macOS Configuration

- **Computer Name**: AstralJunction
- **Username**: dpriley
- **Remote Login**: Enabled (SSH daemon running)
- **Screen Sharing**: Enabled
- **Auto-login**: Enabled
- **Sleep**: Disabled (always awake)
- **Peripherals**: Disconnected (headless)

#### Active Docker Services

1. **cloudflared** - Cloudflare Tunnel daemon (auto-restart enabled)
2. **test-webserver** (nginx:alpine) - Test page on port 8080

#### Cloudflare Configuration

- **Account**: Danril3y@gmail.com
- **Tunnel Name**: astraljunction
- **Tunnel ID**: 93694f44-5a63-49ef-bbd8-ace63963fb46

**Domains:**
| Domain | Configuration |
|--------|---------------|
| toomanytabs.space | Test/practice domain, Cloudflare Tunnel routes |
| volkyra.com | Production, Cloudflare Pages deployment |

#### Files & Directories

```
/Users/dpriley/
├── .cloudflared/
│   ├── config.yml (tunnel configuration)
│   ├── 93694f44-5a63-49ef-bbd8-ace63963fb46.json (tunnel credentials)
│   └── cert.pem (authentication certificate)
├── test-site/
│   └── index.html (purple gradient test page)
└── [Future: service directories for Paperless-ngx, Plex, etc.]
```

---

### Known Issues 🔴

**CRITICAL: SSH from MacBook Doesn't Work**

- **Issue**: macOS 26.2 (Tahoe) networking bug on MacBook client
- **Error**: "No route to host" when SSH'ing from MacBook to Mac Mini
- **Working Alternatives**:
  - ✅ PC → Mac Mini SSH (works perfectly)
  - ✅ Screen Sharing (VNC) from MacBook
  - ✅ Termius app (MacBook and iPhone)
  - ✅ Port forwarding via public IP works
- **Cause**: macOS 26.2 kernel-level networking bug (not configuration issue)
- **Resolution**: Wait for macOS 26.3 fix; use workarounds

**Cloudflare Tunnel Docker Networking Complexity**

- `host.docker.internal` doesn't always resolve from inside containers
- Docker bridge network (172.17.0.1) has issues reaching Mac host services
- **Working Solution**: Use Mac's LAN IP (192.168.70.70) in service configurations
- **Potential Fix**: Consider running cloudflared directly on host, not in Docker

---

## II. Project Roadmap and Backlog

### ✅ Completed

- [x] Configure Mac Mini as headless server
- [x] Set up static IP via DHCP reservation (192.168.70.70)
- [x] Enable remote access (SSH + VNC)
- [x] Install Homebrew, Docker, Cloudflared
- [x] Migrate volkyra.com from AWS to Cloudflare Pages
- [x] Clean up AWS resources (S3, Route 53)
- [x] Create Cloudflare Tunnel (astraljunction)
- [x] Deploy test page at test.toomanytabs.space
- [x] Configure cloudflared to run in Docker (auto-restart)
- [x] Establish deployment patterns for future services
- [x] Configure Cloudflare Access with Zero Trust (free tier)
- [x] Set up WebAuthn/passkeys authentication
- [x] Document architecture and workflows

### 🚧 In Progress

- [ ] Resolve Cloudflare Tunnel connectivity issues (ssh.toomanytabs.space, vnc.toomanytabs.space)
- [ ] Evaluate running cloudflared on host vs Docker

### 📋 Next Up (High Priority)

- [ ] Deploy Paperless-ngx document management
- [ ] Deploy Plex media server
- [ ] Deploy Nextcloud file sync
- [ ] Deploy Immich photo management
- [ ] Set up Logseq sync server

### 🔮 Future / Backlog

- [ ] Add volkyra.com dynamic features (resume mailer, contact form)
- [ ] Create personal book library database
- [ ] Build dashboard/admin interface for services
- [ ] Create home lab documentation site
- [ ] Set up Mercurial hosting
- [ ] Set up jj (Jujutsu) repository hosting
- [ ] Implement monitoring (Uptime Kuma or similar)
- [ ] Configure automated container updates

### 🎯 Service Deployment Pattern (Established)

1. Start Docker container on a port
2. Add hostname to tunnel config
3. Restart cloudflared
4. Configure Cloudflare Access policy if needed
5. Access at subdomain.toomanytabs.space

---

## III. Conversation Context Summary

### 3.1 Key Discussion Points

- Extensive troubleshooting of SSH connectivity issues between MacBook and Mac Mini
- Deep dive into Cloudflare Tunnel architecture and configuration
- Comparison of self-hosted vs cloud-hosted solutions
- Docker networking intricacies (host.docker.internal vs bridge network)
- Two-tier hosting strategy (Cloudflare Pages + Tunnel)

### 3.2 Notable Insights

- **Mobile-first matters**: "The best organizational system is the one you'll actually use"
- **Cloudflare's free tier is remarkably generous** for personal infrastructure
- **Clean separation principle**: Compute on Mac Mini, storage on NAS
- **SSH bug is Apple's fault**: macOS 26.2 has kernel-level networking issues

### 3.3 Unresolved Questions

- Should cloudflared run on host or in Docker? (Docker has networking complexity)
- Best practices for automated Docker container updates?
- How to implement service health monitoring?

### 3.4 Potential Pivot Points

- If macOS limitations persist, consider migrating to Linux
- If Cloudflare Tunnel complexity continues, evaluate alternatives
- May need to implement port forwarding as backup for SSH/VNC access

### 3.5 User Preferences & Patterns

- **TL;DR requested** at start of complex responses
- **Section-by-section TL;DRs** for multi-part answers
- **Reprint instructions** after debugging/clarification
- **Very specific about which machine** to run commands on
- **Prefers understanding "why"** not just "how"
- **Uses Nushell** as primary shell (not bash)
- **Uses jj (Jujutsu)** for version control (not Git)

### 3.6 Additional Goals/Interests Mentioned

- Custom house design project (Queen Anne Victorian style)
- ~1,000 architectural inspiration photos needing organization
- Learning more Fortran (limited fluency despite 13 years of use)
- Exploring Python sets more (requested use where appropriate)
- GUI development learning (Python, HTML/CSS/JavaScript, Eel framework)

---

## Cross-References

- **DIY NAS & Backup Strategy**: See PMR #2
- **Personal Knowledge Management & Supernote**: See PMR #3
- **Home Design Visual Reference Management**: Included in services to deploy (Immich)
