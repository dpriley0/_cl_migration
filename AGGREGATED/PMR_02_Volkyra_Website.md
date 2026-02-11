# Project Migration Record: Volkyra.com Website

---

# PART I: COMPREHENSIVE PROJECT SUMMARY

## 1. Project Vision

### High-Level Goals
Build a dual-purpose personal website for Laura (Dan's wife) that serves as both a professional networking tool and a personal photo management platform, replacing expensive commercial services.

### Motivations
- **Professional Networking**: Create a seamless resume-sharing system via NFC business cards
- **Photo Independence**: Replace Adobe cloud photo storage with a self-controlled solution
- **Cost Savings**: Eliminate ongoing subscription fees for photo hosting
- **Learning Opportunity**: Full-stack web development project with practical, real-world use
- **Privacy Control**: Keep personal photos and data under family control

### Apps, Tools, Utilities, and Services of Interest
- **NFC Business Cards** - Physical cards that trigger automatic resume delivery
- **Resume Email Automation** - Collect contact info and auto-send resume
- **Photo Storage Platform** - Self-hosted photo storage and organization
- **Photo Albums** - Organize photos into shareable collections
- **Photo Editing** (Stretch Goal) - Integrate existing JavaScript photo editing libraries
- **Contact Database** - Track who's received Laura's resume for networking analytics

### Strategic Objectives
1. Create professional NFC landing page for resume distribution
2. Implement automated email system for resume delivery
3. Build secure photo storage with album organization
4. Enable photo sharing via private links
5. Track networking contacts for follow-up

### Long-term Potential / Potential Workflows
**Networking Workflow:**
1. Laura meets someone at a networking event
2. They tap her NFC card with their phone
3. Their browser opens volkyra.com landing page
4. They enter their email and name
5. System automatically sends her resume
6. Contact is logged for follow-up
7. Laura can review analytics on who's interested

**Photo Workflow:**
1. Upload photos from any device
2. Organize into albums (events, categories, inspiration)
3. Share specific albums via private links
4. Edit photos using built-in tools (stretch goal)
5. Download or share edited versions

### Unique Value Proposition
A personalized professional presence that makes a memorable impression while giving complete control over personal data and eliminating recurring subscription costs.

### Core Philosophies Discussed
- **"Practice like you play"** - Build for real use from day one
- **Mobile-first** - Primary usage will be on phones
- **Iterative development** - Start simple, add features incrementally
- **User experience focus** - Laura's ease of use is paramount

---

## 2. Requirements Compilation

### Explicit Requirements

**NFC/Resume System:**
- Landing page displays when NFC card is scanned
- Form collects visitor's email address and name
- System automatically emails resume as PDF attachment
- Professional, polished design that matches Laura's brand
- Contact information stored for networking records

**Photo System:**
- Upload photos from any device (primarily phone)
- Organize photos into albums
- View photos in gallery format
- Share albums privately (not publicly indexed)
- Replace Adobe cloud subscription functionality

### Implied Requirements
- HTTPS required (security, and camera access for future features)
- Mobile-responsive design (most interactions via phone)
- Fast loading (professional impression matters)
- Reliable email delivery (can't miss resume requests)
- Sufficient storage for photo library

### Derived Requirements
- Cloudflare Pages for static hosting (fast, free, HTTPS)
- Flask backend for dynamic features (form handling, email)
- SQLite or JSON for simple data storage
- Pillow (Python) for thumbnail generation
- SMTP integration for email delivery

### Potential Edge Cases or Expansion Areas
- Multiple resume versions for different contexts
- Analytics on which NFC cards generate most interest
- Photo editing capabilities (stretch goal)
- Video support (future consideration)
- Guest upload functionality for events
- Integration with social media sharing

---

## 3. Architectural Overview

### Current Design Decisions with Rationale

**Static Site on Cloudflare Pages (CURRENT)**
- Migrated from AWS S3 to Cloudflare Pages
- Automatic HTTPS
- Global CDN for fast loading
- Git-based deployment from GitHub (dpriley0/web_lmr)
- Free tier covers needs

**Planned: Flask Backend on AstralJunction**
- For NFC form processing
- Email automation via Flask-Mail
- Contact database management
- Photo upload and management
- Access via Cloudflare Tunnel

### Decisions We Considered and Ruled Out

**AWS Serverless (Lambda + S3 + DynamoDB + SES) - RULED OUT**
- Initially explored and partially implemented
- Ruled out because:
  - Data privacy concerns (prefer self-control)
  - Ongoing costs add up
  - Learning Flask provides more transferable skills
  - Better alignment with self-hosting philosophy
- Migration path: Already cleaned up AWS resources, moved to Cloudflare Pages

**GitHub Pages - RULED OUT**
- Cloudflare Pages chosen for better ecosystem integration
- Unified management with Tunnel, Workers
- Simpler DNS configuration

### Technology Stack

**Current (Static Site):**
- Hosting: Cloudflare Pages
- DNS: Cloudflare
- Source: GitHub (dpriley0/web_lmr)
- Deployment: Automatic on git push

**Planned (Dynamic Features):**
- Backend: Flask (Python)
- Database: SQLite
- Email: Flask-Mail or smtplib
- Photo Processing: Pillow
- Host: AstralJunction (Mac Mini) via Cloudflare Tunnel
- Storage: Local filesystem with organized directories

### Frameworks We've Decided to Use
- **Flask** - Python web framework (simple, well-documented)
- **Pillow** - Image processing (thumbnails, resizing)
- **Flask-Mail** - Email functionality
- **Html5-QrcodeScanner** or **QuaggaJS** - For barcode scanning (library feature)

### Frameworks Explicitly Ruled Out
- **Django** - Overkill for this project's scope
- **AWS Lambda** - Moving away from cloud lock-in

### Key Architectural Constraints
- Must work with Cloudflare Tunnel for backend access
- HTTPS required for all features
- Mobile-first responsive design
- Storage limited to Mac Mini (initially)

### Potential Future Directions
- Photo editing integration (Fabric.js, CropperJS)
- Event-specific landing pages (conference booth, coffee meeting)
- Tracking which cards/events generate most interest
- Integration with Immich for backend photo management
- NFC cards with unique identifiers for analytics

---

## 4. Current State

### What's Working ✅
- Domain: volkyra.com and www.volkyra.com active
- Hosting: Cloudflare Pages deployment working
- DNS: Configured and propagated
- GitHub Integration: Auto-deploy on push to main
- Basic landing page deployed

### What's Been Completed
- [x] Domain purchased (Spaceship.com)
- [x] AWS account setup and free tier exploration
- [x] Route 53 DNS configuration (later migrated)
- [x] S3 static hosting setup (later migrated)
- [x] Migration from AWS to Cloudflare Pages
- [x] AWS resource cleanup (S3, Route 53)
- [x] Cloudflare DNS configuration
- [x] Basic welcome page deployed

### Historical Note: AWS Journey
The project originally started with AWS:
1. Created S3 bucket for website hosting
2. Configured Route 53 for DNS
3. Encountered and resolved bucket naming issue (must be exactly "volkyra.com")
4. Fixed Route 53 A record pointing to S3 website endpoint
5. Later migrated to Cloudflare Pages for better integration with self-hosting strategy

---

# PART II: PROJECT ROADMAP AND BACKLOG

## Mission Control Dashboard 🚀🛠️

### Purpose
This backlog tracks the development of volkyra.com - Laura's personal website featuring NFC business card integration, automated resume delivery, and photo management.

### Current Status: Static Site Live, Backend Development Pending

---

## Active Projects

### NFC Business Card & Resume System
- [ ] Design NFC landing page with contact form
- [ ] Implement Flask backend for form processing
- [ ] Set up email automation (Flask-Mail)
- [ ] Create professional email template with Laura's branding
- [ ] Store contacts in database for networking records
- [ ] Program NFC cards to point to landing page
- [ ] Test email delivery reliability
- [ ] Add contact analytics/tracking

**Technical Notes:**
- NFC cards can be programmed with any smartphone using free apps (NFC Tools, TagWriter)
- NTAG213 tags are sufficient for URL storage
- Each card can have unique identifier for source tracking (e.g., `volkyra.com/resume?source=conference_booth`)

### Photo Storage Platform
- [ ] Implement file upload system with drag-and-drop
- [ ] Create photo thumbnail generation pipeline
- [ ] Build album creation and organization features
- [ ] Develop photo viewing interface (gallery)
- [ ] Implement album sharing (private links)
- [ ] Add photo metadata extraction (EXIF)
- [ ] Build search and filtering capabilities
- [ ] Ensure responsive design for mobile

### Photo Editing (Stretch Goal)
- [ ] Research JavaScript photo editing libraries
- [ ] Integrate basic adjustments (brightness, contrast, crop, rotate)
- [ ] Add filters and effects
- [ ] Implement save/export of edited versions

---

## Development Timeline (Original Estimate)
At 2-5 hours per week:

**Phase 1: Foundation (Weeks 1-4)** - 8-12 hours
- Flask app setup
- NFC landing page with contact form
- Email functionality for resume sending
- Deploy to domain

**Phase 2: Photo Upload & Storage (Weeks 5-8)** - 12-16 hours
- File upload system with drag-and-drop
- Photo thumbnail generation
- Basic album creation and organization
- Simple photo viewing interface

**Phase 3: Advanced Photo Features (Weeks 9-12)** - 12-16 hours
- Album sharing (private links)
- Photo metadata extraction (EXIF data)
- Search and filtering capabilities
- Responsive design improvements

**Phase 4: Photo Editing (Weeks 13-16)** - Stretch Goal - 8-12 hours
- JavaScript-based photo editor
- Basic adjustments
- Filters and effects
- Save edited versions

---

## Completed ✅
- [x] Domain purchase and configuration
- [x] AWS initial setup and exploration
- [x] Migration to Cloudflare Pages
- [x] Basic welcome page deployed
- [x] DNS configuration complete
- [x] Auto-deployment pipeline working

---

## Technical Architecture

### Project Structure
```
volkyra.com/
├── app.py                 # Main Flask application
├── config.py             # Configuration settings
├── requirements.txt      # Python dependencies
├── static/
│   ├── css/
│   ├── js/
│   └── uploads/          # Photo storage
├── templates/            # HTML templates
└── database.db          # SQLite database
```

### NFC System Flow
```
NFC Card Scan → volkyra.com → Email Collection → Resume Email → Thank You Page
```

### Key Routes (Planned)
- `/` - Home page
- `/nfc` or `/resume` - NFC landing page with form
- `/submit_contact` - Form submission handler
- `/photos` - Photo gallery
- `/albums` - Album management
- `/upload` - Photo upload interface

---

# PART III: CONVERSATION CONTEXT SUMMARY

## Key Discussion Points
1. **Dual-purpose design**: Combining professional networking and personal photo management
2. **AWS vs Self-hosting**: Started with AWS, pivoted to self-hosting for privacy/control
3. **NFC simplicity**: Programming NFC cards is surprisingly easy with smartphone apps
4. **Iterative approach**: Start with MVP, add features based on actual use

## Notable Insights
- NFC TagInfo and TagWriter apps can program blank NFC tags from any smartphone
- NTAG213 chips have sufficient memory (180 bytes) for URL storage
- Cloudflare Pages integrates better with self-hosting strategy than AWS
- The hardest part of NFC cards is designing them to look tap-able

## Unresolved Questions
- Exact design aesthetic for landing page
- Whether to use existing Dot cards or create custom NFC cards
- Photo storage size requirements (how many photos total?)
- Whether photo editing stretch goal is worth the complexity

## Potential Pivot Points
- Could integrate with Immich on AstralJunction instead of building custom photo system
- Could use Cloudflare Workers for form handling instead of Flask (simpler)
- Could expand to include portfolio/work samples

## What I've Learned About the Project
- Laura already has Dot NFC cards; this project could enhance or replace them
- The resume-sharing pain point is real and drives the NFC feature priority
- Adobe cloud costs are a motivating factor for photo self-hosting
- Mobile-first usage pattern (scanning cards, viewing photos on phone)

## Additional Context
- Dan can dedicate 2-5 hours per week to technical projects
- Learning Flask is a key development goal alongside the project
- The project serves as practical learning for Python web development
- Laura's input on design/UX will be important

---

## Related Projects
- **AstralJunction Home Lab**: Provides backend hosting infrastructure
- **Laura's Library Database**: Similar Flask-based web app pattern
- **AI-Collaborative Project Dashboard**: Could showcase this project's status

## Migration Notes
This PMR consolidates context from multiple conversations about volkyra.com development, AWS setup and migration, NFC business card implementation, and photo storage planning. The project began with AWS exploration but pivoted to align with the self-hosting philosophy.
