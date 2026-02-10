# Project Migration Record: AstralJunction Home Lab & Server Infrastructure

---

# PART I: COMPREHENSIVE PROJECT SUMMARY

## 1. Project Vision

### High-Level Goals

Build a comprehensive DIY personal cloud infrastructure centered around a 2018 Mac Mini server ("AstralJunction") to replace commercial cloud services, prioritizing privacy and preventing companies from profiting from personal data.

### Motivations

- **Privacy First**: Complete control over personal data without corporate data mining
- **Cost Efficiency**: Eliminate ongoing cloud service subscriptions (AWS, Dropbox, Google Photos, Adobe, etc.)
- **NASA Restrictions**: Self-hosted solutions provide full access from anywhere, working around NASA's strict IT restrictions (web-only cloud access)
- **Learning Opportunity**: Deep understanding of infrastructure fundamentals rather than relying on abstractions
- **Long-term Independence**: No vendor lock-in, services can't be changed/shut down by third parties

### Apps, Tools, Utilities, Websites, and Services of Interest

**Currently Deployed ✅**

- **volkyra.com** - Personal website (migrated from AWS to Cloudflare Pages)
- **test.toomanytabs.space** - Test page via Cloudflare Tunnel (CURRENTLY BROKEN - needs repair)
- **Cloudflare Tunnel** - Secure remote access (running in Docker)
- **Docker** - Container platform for services
- **Nginx** - Web server (for test page)

**High Priority - Mentioned Multiple Times**

- **Paperless-ngx** - Document management with OCR, tag-based organization
- **Plex** - Media server for streaming movie/TV collection
- **Nextcloud** - Self-hosted file sync/cloud storage (Dropbox replacement)
- **Immich** - Photo management (self-hosted Google Photos alternative) - Perfect for ~1,000 architectural inspiration photos
- **Logseq Sync Server** - Self-hosted sync for PKM across devices

**Tools & Workflows**

- **Supernote A6 X2** - E-ink tablet for analog note-taking (OCR automation to flow handwritten notes into Logseq planned)
- **jj (Jujutsu)** - Preferred version control over Git
- **Mercurial** - Also using, want to host repositories
- **Nushell** - Primary shell

**Future Infrastructure**

- **DIY NAS** - RAID-Z1, BTRFS/ZFS filesystems, SMS/email alerts
- **Backups** - Local + encrypted offsite (Backblaze B2 with restic)
- **Cloudflare Access** - Authentication using WebAuthn/passkeys (Face ID for Dan on iPhone, Fingerprint for Laura on Android)
- **Version control hosting** - For jj and Mercurial repositories

**Web Projects**

- Dashboard/admin interface for managing services
- Home lab documentation site
- Photo gallery frontend (with Immich backend)
- Timecard app for work

**Other Services Mentioned**

- **Syncthing** - File sync alternative to Nextcloud (considered)
- **iMessage/SMS web interface** - Future project

**Tools/Platforms Used**

- **Proton VPN** - Premium account (caused network issues with WireGuard extension - resolved)
- **Proton Mail Bridge** - Email integration
- **Proton Pass** - Password manager
- **Proton Drive** - File storage
- **Claude Code** - AI-assisted coding (installed via Homebrew, OAuth authenticated)
- **Homebrew** - Package manager
- **Spaceship.com** - Domain registrar (toomanytabs.space, volkyra.com)
- **Cloudflare** - DNS, CDN, Pages, Tunnel, Workers (free tier)
- **GitHub** - Repository hosting (web_lmr for volkyra.com)

### Strategic Objectives

1. Establish AstralJunction as the central hub for all self-hosted services
2. Migrate all personal data off commercial cloud services (Box, Drive, OneDrive)
3. Implement biometric authentication for secure remote access
4. Deploy containerized services following bare-metal-first learning approach
5. Create a comprehensive, always-accessible personal cloud ecosystem

### Long-term Potential / Potential Workflows

- Complete elimination of commercial cloud dependency
- Centralized document management with AI-powered search and OCR
- Unified media streaming across all devices
- Seamless file sync working with NASA firewall restrictions
- Automated backup systems with geographic redundancy
- Self-hosted version control for all personal and professional projects

### Unique Value Proposition

A single, cost-effective ($5-10/month electricity vs $50-200/month cloud) home server that provides complete control, privacy, and independence while working within enterprise firewall restrictions.

### Core Philosophies Discussed

- **Self-hosted solutions for maximum privacy and control**
- **Long-term efficiency over immediate ease of use**
- **Understanding fundamental systems rather than relying on abstractions**
- **Mobile-first approach** (systems must work on iPhone since that's where you actually use them)
- **"Practice like you play"** - Learn bare metal first, then containerize
- **Two-Tier Hosting Strategy**: Static sites → Cloudflare Pages; Dynamic services → Mac Mini via Tunnel

---

## 2. Requirements Compilation

### Explicit Requirements

- Headless server operation (no monitor/keyboard)
- 24/7 availability with auto-recovery after power outages
- Secure remote access from anywhere (including through NASA firewall)
- Biometric authentication (WebAuthn/passkeys) for both iOS and Android devices
- HTTPS for all services (required for mobile camera access, security)
- Docker containerization for service isolation
- Auto-restart for all critical containers

### Implied Requirements

- Low power consumption for always-on operation
- Quiet operation (home environment)
- Reliable network connectivity with static IP
- Sufficient storage for media, documents, and photos
- Regular backup strategy

### Derived Requirements

- Cloudflare Tunnel for zero-trust remote access (no port forwarding, IP hidden)
- Docker with `host.docker.internal` networking for tunnel config
- Separate database containers per application for isolation
- Version control (jj preferred) for configuration management
- Mobile-responsive web interfaces for all services

### Potential Edge Cases or Expansion Areas

- Power outage recovery testing
- Network failover scenarios
- Storage expansion (NAS addition)
- Multi-user access management
- Service monitoring and alerting
- Geographic backup redundancy

---

## 3. Architectural Overview

### Current Design Decisions with Rationale

**Two-Tier Hosting Strategy (ADOPTED)**

1. **Static Sites → Cloudflare Pages**
   
   - Professional hosting, global CDN
   - No home IP exposure
   - Git-based deployment (automatic on push)
   - Free tier
   - Example: volkyra.com, www.volkyra.com

2. **Dynamic Services → Mac Mini via Tunnel**
   
   - Full control over backend
   - Docker containers for isolation
   - Cloudflare Tunnel hides home IP
   - Free tier
   - Pattern: Start container on port → Add to tunnel config → Restart cloudflared → Access at subdomain.toomanytabs.space

**Decisions We Considered and Ruled Out**

- **AWS Serverless (Lambda, S3, DynamoDB)**: Initially explored for volkyra.com but ruled out in favor of self-hosting for:
  
  - Data privacy (control over personal data)
  - Cost savings (one-time hardware vs ongoing cloud)
  - Learning opportunity
  - Better alignment with self-hosting philosophy

- **GitHub Pages**: Considered but Cloudflare Pages chosen for:
  
  - Better integration with Cloudflare ecosystem (Tunnel, Workers)
  - More seamless workflow
  - Unified DNS management

- **Syncthing vs Nextcloud**: Both considered for file sync; Nextcloud preferred for:
  
  - Web interface for NASA access
  - Additional features (calendar, contacts)
  - Better ecosystem integration

### Technology Stack

- **Hardware**: 2018 Mac Mini (Intel i7-8700B, 32GB RAM, 128GB SSD)
- **OS**: macOS (headless operation)
- **Containerization**: Docker Desktop v29.0.1
- **Tunnel**: Cloudflared v2025.11.1
- **Web Server**: Nginx (Alpine)
- **Shell**: Nushell v0.109.1
- **Version Control**: jj (Jujutsu), Mercurial, Git
- **DNS/CDN**: Cloudflare
- **Package Manager**: Homebrew v5.0.4

### Frameworks We've Decided to Use

- **Cloudflare ecosystem** for DNS, CDN, Tunnel, Pages, Workers
- **Docker** for service containerization
- **Flask** for custom Python web applications

### Frameworks Explicitly Ruled Out

- **AWS Lambda/Serverless**: Too abstracted, ongoing costs, data privacy concerns
- **Kubernetes**: Overkill for single-node home server
- **Complex orchestration tools**: Maintaining simplicity for learning

### Key Architectural Constraints

- Single Mac Mini hardware (planning NAS expansion)
- Must work through NASA firewall (web-only access)
- Biometric auth required for both iOS (Face ID) and Android (fingerprint)
- Services must be accessible via HTTPS

### Potential Future Directions

- DIY NAS with RAID-Z1/ZFS
- Offsite encrypted backups to Backblaze B2
- Hardware monitoring with SMS/email alerts
- Custom dashboard for service management
- Integration with AI-powered document analysis (Paperless-ngx + Claude Code)

---

## 4. Current State of AstralJunction

### Hardware

- **Model**: 2018 Mac Mini (Intel)
- **CPU**: Intel Core i7-8700B (6-core)
- **RAM**: 32GB
- **Storage**: 128GB SSD (planning DIY NAS expansion)
- **Network**: Gigabit Ethernet (not WiFi)
- **Static IP**: 192.168.70.70 (DHCP reservation)
- **MAC Address**: 14:9D:99:81:E9:F2

### Network Configuration

- **Router**: TP-Link Deco mesh system
- **Subnet**: 192.168.68.x/22 (255.255.252.0)
- **Router IP**: 192.168.68.1
- **Bonjour Hostname**: AstralJunction.local
- **Remote Access**: Screen Sharing (VNC) at vnc://192.168.70.70

### macOS Configuration

- **Computer Name**: AstralJunction
- **Username**: dpriley
- **Hostname**: dan@AstralJunction
- **Remote Login**: Enabled (SSH daemon running)
- **Auto-login**: Enabled (automatic recovery after power outages)
- **Sleep**: Disabled (24/7 operation)
- **Peripherals**: Disconnected (headless)

### Active Services (Docker Containers)

1. **cloudflared**
   
   - Status: Running, auto-restart enabled
   - Config: `/Users/dpriley/.cloudflared/config.yml`
   - Tunnel Name: astraljunction
   - Tunnel ID: 93694f44-5a63-49ef-bbd8-ace63963fb46
   - Routes: test.toomanytabs.space → http://host.docker.internal:8080

2. **test-webserver (nginx:alpine)**
   
   - Status: BROKEN (needs repair)
   - Port: 8080
   - Volume: ~/test-site mounted to /usr/share/nginx/html
   - Serves: Purple gradient "AstralJunction Online!" test page

### Cloudflare Configuration

- **Account**: Danril3y@gmail.com
- **Domains**:
  - **toomanytabs.space** (practice/test domain)
  - **volkyra.com** (production domain - Cloudflare Pages)

### Known Issues 🔴

**CRITICAL: SSH from MacBook Doesn't Work**

- **Issue**: macOS 26.2 (Tahoe) networking bug on MacBook
- **Error**: "No route to host" from MacBook to Mac Mini
- **Working**: PC → Mac Mini SSH, Screen Sharing from MacBook, all other network services
- **Workarounds**: Use Screen Sharing (VNC), Termius, or PC for SSH
- **Expected Fix**: macOS 26.3 (late January 2026)

### What's Working ✅

1. Mac Mini headless server operational
2. Remote access via Screen Sharing
3. Docker containers running
4. Cloudflare Tunnel active (cloudflared in Docker)
5. volkyra.com deployed on Cloudflare Pages
6. www.volkyra.com working
7. Auto-restart for all containers
8. Git workflow established with jj

### Ready to Deploy 🚀

- Paperless-ngx
- Plex
- Nextcloud
- Immich
- Logseq sync
- Any Docker-based service

### Files & Directories Structure

```
/Users/dpriley/
├── .cloudflared/
│   ├── config.yml (tunnel configuration)
│   ├── 93694f44-5a63-49ef-bbd8-ace63963fb46.json (credentials)
│   └── cert.pem (authentication certificate)
├── test-site/
│   └── index.html (purple gradient test page)
└── [Future: service directories for Paperless-ngx, Plex, etc.]
```

---

# PART II: PROJECT ROADMAP AND BACKLOG

## Mission Control Dashboard 🚀🛠️

### Purpose

This backlog tracks the AstralJunction home server infrastructure project - transforming a 2018 Mac Mini into a comprehensive DIY personal cloud.

### Current Status: Foundation Complete, Ready for Services

---

## Active Priorities

### 🔴 Critical (Blocked/Broken)

- [ ] **FIX: test.toomanytabs.space** - Test webserver is broken, needs debugging
- [ ] **WORKAROUND: SSH from MacBook** - macOS 26.2 bug; use Screen Sharing until macOS 26.3

### 🟡 High Priority

- [ ] Implement biometric authentication (Cloudflare Access with WebAuthn/passkeys)
  - Face ID for Dan (iPhone)
  - Fingerprint for Laura (Android)
- [ ] Configure cloud storage migration
  - [ ] Migrate data from Box, Drive, OneDrive
  - [ ] Set up automatic camera roll backups
  - [ ] *Defer* full photo-management system (Immich) to later phase

### 🟢 Ready to Deploy

- [ ] Deploy Paperless-ngx document library
  - Personal use on Mac Mini (primary)
  - Separate, airgapped instance on work PC for NASA documents
- [ ] Set up Plex media server
- [ ] Deploy Nextcloud file sync
- [ ] Deploy Immich photo management (after camera roll backups working)
- [ ] Configure Logseq sync server

---

## Deployment Pattern (Established)

```
1. Start Docker container on a port
2. Add route to tunnel config (~/.cloudflared/config.yml)
3. Restart cloudflared container
4. Access at subdomain.toomanytabs.space
```

---

## Future Milestones

### Phase 1: Storage & Documents

- [ ] Deploy Paperless-ngx
- [ ] Configure document OCR and tagging
- [ ] Migrate existing documents
- [ ] Test web access from NASA network

### Phase 2: Media & Photos

- [ ] Deploy Plex
- [ ] Configure media library
- [ ] Deploy Immich
- [ ] Migrate ~1,000 architectural inspiration photos
- [ ] Set up camera roll auto-backup

### Phase 3: File Sync & Collaboration

- [ ] Deploy Nextcloud
- [ ] Migrate from Box/Drive/OneDrive
- [ ] Configure calendar/contacts sync
- [ ] Set up Logseq sync

### Phase 4: Infrastructure Hardening

- [ ] Implement biometric authentication
- [ ] Set up DIY NAS (RAID-Z1/ZFS)
- [ ] Configure offsite backups (Backblaze B2 + restic)
- [ ] Implement monitoring and alerting
- [ ] Transition from bare metal to full containerization

### Phase 5: Advanced Features

- [ ] Version control hosting (jj, Mercurial)
- [ ] iMessage/SMS web interface
- [ ] Custom admin dashboard
- [ ] Integration with AI-augmented knowledge management

---

## Completed ✅

- [x] Configure Mac Mini as headless server
- [x] Set up static IP via DHCP reservation
- [x] Enable remote access (SSH + VNC)
- [x] Install Homebrew, Docker, Cloudflared
- [x] Migrate volkyra.com from AWS to Cloudflare Pages
- [x] Clean up AWS resources (S3, Route 53)
- [x] Create Cloudflare Tunnel (astraljunction)
- [x] Deploy test page accessible globally
- [x] Configure cloudflared in Docker (auto-restart)
- [x] Debug DNS, networking, configuration issues
- [x] Establish deployment patterns
- [x] Document architecture and workflows

---

## Technical Notes

### Deployment Checklist Template

```markdown
### [Service Name] Deployment
- [ ] Create docker-compose.yml
- [ ] Configure persistent volumes
- [ ] Add to tunnel config
- [ ] Test local access
- [ ] Test remote access
- [ ] Document configuration
```

### Key Commands

```bash
# Screen share to AstralJunction
open vnc://192.168.70.70

# Restart cloudflared after config change
docker restart cloudflared

# Check Docker container status
docker ps

# View tunnel logs
docker logs cloudflared
```

---

# PART III: CONVERSATION CONTEXT SUMMARY

## Key Discussion Points

1. **Philosophy alignment**: Dan's preference for understanding systems deeply vs using abstractions
2. **Two-tier hosting strategy** emerged from balancing simplicity (static) vs control (dynamic)
3. **Mobile-first approach**: All solutions must work on iPhone since that's primary usage
4. **NASA restrictions** drove the need for web-accessible solutions
5. **Cost analysis**: Detailed comparison of self-hosting (~$5-10/month) vs cloud ($50-200/month)

## Notable Insights

- **Docker networking quirk**: Use `host.docker.internal` instead of `localhost` in tunnel configs
- **Cloudflare integration**: Pages + Tunnel + Workers forms a powerful free-tier ecosystem
- **Bare metal first**: Learning fundamentals before containerization leads to better troubleshooting skills
- **The macOS SSH bug** is a kernel-level issue in Tahoe, not configuration - patience required

## Unresolved Questions

- Best approach for NAS implementation (DIY vs pre-built)
- Optimal backup strategy balancing cost and redundancy
- Whether Syncthing or Nextcloud better fits the use case
- How to integrate Claude Code with Paperless-ngx for AI-augmented document analysis

## Potential Pivot Points

- If Mac Mini becomes insufficient, could migrate to more powerful hardware
- If Cloudflare changes free tier, may need to reassess tunnel strategy
- Could expand to multiple servers for redundancy

## What I've Learned About Dan

- **Work**: Aerospace engineer at NASA, works on rocket engine simulation software (ROCETS)
- **Technical level**: 13 years Fortran experience, Python fluency 2-2.5/5, new to OOP paradigm
- **Learning style**: Prefers visuals, real-life examples, metaphors; wants to understand "why" not just "how"
- **Response preferences**: TL;DR at start, section TL;DRs, clear machine-specific instructions

## Additional Goals Expressed

- Custom house design (Queen Anne Victorian with wife Laura)
- Organization of ~1,000 architectural inspiration photos
- Supernote integration for analog → digital notes workflow
- Learning Fortran fluency, OOP principles, Python sets

---

## Migration Notes

This PMR consolidates context from multiple conversations about home server infrastructure, cloud storage migration, and service deployment. The project is foundational to many other projects (Volkyra.com, Library Database, etc.) and represents the core infrastructure layer.
