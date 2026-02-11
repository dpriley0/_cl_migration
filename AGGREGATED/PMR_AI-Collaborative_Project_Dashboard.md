# Project Migration Record: AI-Collaborative Project Dashboard

---

# I. Comprehensive Project Summary

## 1. Project Vision

### High-Level Goals

- Build a self-hosted, web-accessible dashboard for viewing and interacting with project backlogs managed through Claude Code collaboration
- Create a "morning check-in" experience that surfaces priorities, context, and actionable next steps
- Automate bi-weekly NASA accomplishment memo generation from daily notes
- Eliminate manual copy/paste between Claude Project spaces by having a single, always-current web view

### Motivations

- **Lateral connection insight**: Dan connected the GitHub → Cloudflare Pages hosting pattern (proven with volkyra.com in homelab project) with the PM methodology's need for accessible, version-controlled artifacts
- **Reduce cognitive overhead**: One place to see all project states from any device
- **NASA memo pain point**: Currently takes 30+ minutes to reconstruct bi-weekly accomplishments from memory; daily notes + auto-aggregation could reduce this to 5 minutes
- **Validate the PM methodology**: This project IS the implementation of the "AI-augmented Fluid Project Management" approach—proof of concept in action

### Apps, Tools, Utilities, Services of Interest

**Core Stack (Confirmed):**
| Tool | Purpose |
|------|---------|
| Cloudflare Pages | Static site hosting, auto-deploy from GitHub |
| Cloudflare Workers | Serverless functions for dynamic features |
| Cloudflare Workers KV | Key-value storage for cached parsed data |
| GitHub | Repository hosting, single source of truth |
| Docsify | Client-side markdown renderer (zero build) |
| jj (Jujutsu) | Dan's preferred version control |

**Integration Points:**
| Tool | Relationship |
|------|--------------|
| Claude Code | Primary interface for editing backlogs |
| Logseq | PKM system, potential integration TBD |
| Supernote A6 X2 | Analog notes → OCR → daily notes pipeline |
| AstralJunction (Mac Mini) | Shared Cloudflare infrastructure |

**Planned Workers (7 total):**

1. Morning Dashboard Worker (`/api/dashboard`)
2. Search Worker (`/api/search`)
3. Metrics Worker (`/api/metrics`)
4. Daily Notes Worker (`/api/daily-notes`)
5. Project Parser Worker (background, on deploy)
6. Session Context Worker (`/api/session`)
7. Cross-Reference Worker (`/api/references`)

### Strategic Objectives

1. **Phase 1**: Get basic markdown rendering working (Docsify + Pages)
2. **Phase 2**: Polish UI/UX for daily use
3. **Phase 3**: Add dynamic features via Workers
4. **Phase 4**: Full ecosystem integration (memos, Logseq, PWA)

### Long-Term Potential / Workflows

**Morning Routine:**

```
Wake up → Open dashboard on iPhone → See priorities + last session context → Start work with full context
```

**Daily Notes Flow:**

```
End of day → 30-second daily note entry → Accumulates over 2 weeks → One-click bi-weekly memo generation
```

**Cross-Project Discovery:**

```
View Project A → See automatic links to related Projects B, C → Enable lateral thinking
```

### Unique Value Proposition

- **Self-hosted Confluence alternative** that's free, git-backed, AI-collaborative
- **Documentation emerges from conversation** rather than being written separately
- **Associative connections** surfaced automatically rather than manually maintained
- **Mobile-first** works on iPhone (Dan's primary device)

### Core Philosophies Discussed

1. **"Associative over hierarchical"** - Projects connect through relationships, not folders
2. **"Conversation as documentation"** - Artifacts emerge from AI collaboration
3. **"Progressive enhancement"** - Static works first, dynamic adds richness
4. **"The best system is the one you'll use"** - Mobile-first, low friction
5. **"Make it work, make it right, THEN make it pretty"** - Combat perfectionism
6. **"Infrastructure investments compound"** - Homelab patterns directly enable this project

---

## 2. Requirements Compilation

### Explicit Requirements

- Must render markdown files as browsable web documentation
- Must auto-deploy when pushing to GitHub
- Must work on iPhone (mobile-first)
- Must support daily notes workflow
- Must generate bi-weekly memo summaries
- Must show "where we left off" context from last session
- Must be free (stay within Cloudflare free tier)

### Implied Requirements

- Must be fast (edge computing, CDN)
- Must handle Dan's ~8-10 project backlogs
- Must integrate with existing jj/GitHub workflow
- Must not require complex build processes
- Must preserve privacy (no third-party analytics)

### Derived Requirements

- Need YAML frontmatter schema for structured metadata
- Need consistent backlog template across projects
- Need session logging mechanism (manual or automated)
- Need date-based organization for daily notes
- Need caching strategy for parsed backlog data

### Potential Edge Cases / Expansion Areas

- Offline access (PWA with service worker)
- Multi-user support (if Laura wants access)
- Private vs public content separation
- Work-sensitive content handling (keep NASA details vague)
- Historical backlog versioning (git provides this)
- Search across very large document sets (may need Vectorize)

---

## 3. Architectural Overview

### Current Design Decisions with Rationale

**Decision: Docsify for Phase 1 rendering**

- Rationale: Zero build process, just drop in index.html, immediate results
- Considered: Jekyll, Hugo, MkDocs
- Ruled out because: Build processes add complexity; Docsify is "good enough" for MVP

**Decision: Cloudflare Workers for dynamic features**

- Rationale: Free tier generous (100k req/day), edge performance, already in our stack
- Considered: Traditional backend on AstralJunction
- Ruled out because: Adds complexity, requires tunnel routing, Workers are simpler

**Decision: Workers KV for data caching**

- Rationale: Fast reads, simple API, free tier sufficient
- Considered: D1 (Cloudflare SQL), external database
- Ruled out because: Overkill for parsed markdown; KV is perfect for key-value lookups

**Decision: GitHub as single source of truth**

- Rationale: Already using for other projects, Pages integration seamless
- Considered: Self-hosted Gitea on AstralJunction
- Ruled out because: Adds maintenance burden, GitHub integration is turnkey

**Decision: Mono-repo structure (recommended, pending Dan's confirmation)**

- Rationale: All backlogs + dashboard code together = simplest deployment
- Considered: Separate repos for dashboard and backlogs
- Still open: Awaiting Dan's decision

### Technology Stack

| Layer              | Technology                  | Notes                      |
| ------------------ | --------------------------- | -------------------------- |
| Static Hosting     | Cloudflare Pages            | Auto-deploy from GitHub    |
| Dynamic Logic      | Cloudflare Workers          | JavaScript at edge         |
| Data Store         | Workers KV                  | Cached parsed data         |
| Markdown Rendering | Docsify                     | Client-side, no build      |
| Version Control    | GitHub + jj                 | jj locally, push to GitHub |
| Content Format     | Markdown + YAML frontmatter | Human + machine readable   |
| Domain             | *.toomanytabs.space         | Subdomain TBD              |

### Frameworks Decided

**Using:**

- Docsify (markdown rendering)
- gray-matter (YAML frontmatter parsing)
- marked.js (markdown → HTML in Workers)

**Explicitly Ruled Out:**

- Jekyll/Hugo/MkDocs - Build process overhead
- React/Vue SPA - Overkill for documentation
- Traditional database - KV sufficient
- Self-hosted Git - Maintenance burden

### Key Architectural Constraints

- Must stay within Cloudflare free tier
- Must work without JavaScript for basic viewing (progressive enhancement)
- Must not expose sensitive work details
- Must support git-based workflow (no database-first)

### Potential Future Directions

- Cloudflare Vectorize for semantic search
- PWA for offline access
- Bidirectional Logseq sync
- Voice memo → daily note transcription
- RSS feed of project updates
- Slack/Discord notifications

---

# II. Project Roadmap and Backlog

## Mission Control Dashboard 🚀

> "Think of this backlog like a mission control dashboard for your project. We'll track project background information, feature ideas, progress, prioritize tasks, and ensure smooth navigation through your technical journey!"

---

## Project Status Overview

| Aspect             | Status                                      |
| ------------------ | ------------------------------------------- |
| **Phase**          | Architecture & Documentation                |
| **Blocking**       | Decision points requiring Dan's input       |
| **Next Milestone** | Phase 1 deployment (1 afternoon when ready) |
| **Technical Risk** | Low (patterns proven via volkyra.com)       |

---

## Decisions Needed ⚠️

### 1. Repository Structure

**Options:**

- A. Separate repo for dashboard only
- B. **Mono-repo with all backlogs + dashboard code** ← Recommended
- C. Dashboard repo + separate backlogs repo

### 2. Subdomain Name

**Options:**

- `projects.toomanytabs.space`
- `dashboard.toomanytabs.space`
- `pm.toomanytabs.space`
- Other?

### 3. Daily Notes File Structure

**Options:**

- A. `/daily-notes/2025/01/2025-01-28.md` (year/month folders)
- B. **`/daily-notes/2025-01-28.md`** (flat with date prefix) ← Recommended
- C. Single file per month with sections

### 4. Logseq Integration Depth

**Options:**

- Reference only (links between systems)
- Bidirectional sync
- **Keep separate initially** ← Recommended

### 5. Authentication

**Options:**

- A. **Public (obscurity)** ← Start here
- B. Cloudflare Access (WebAuthn) ← Add before "production"

---

## Implementation Phases

### Phase 1: Foundation (Get It Working) 🎯

**Effort:** 1 afternoon | **Status:** Ready when Dan is

- [ ] **[DECISION]** Choose repository structure
- [ ] **[DECISION]** Choose subdomain name
- [ ] Create GitHub repository
- [ ] Set up basic folder structure
- [ ] Add Docsify configuration (index.html + config)
- [ ] Connect repo to Cloudflare Pages
- [ ] Configure DNS for subdomain
- [ ] Push existing backlogs
- [ ] Verify auto-deployment
- [ ] Test on iPhone

### Phase 2: Polish (Make It Right)

**Effort:** 1 weekend | **Status:** After Phase 1

- [ ] Custom sidebar navigation
- [ ] Dark mode / theme selection
- [ ] Project status indicators in sidebar
- [ ] Index/home page with project overview
- [ ] Mobile-responsive styling improvements
- [ ] Docsify search configuration

### Phase 3: Dynamic Features (Make It Smart)

**Effort:** 2-3 weekends | **Status:** After Phase 2

- [ ] Set up Wrangler (Workers CLI)
- [ ] Morning Dashboard Worker
  - [ ] Parse all backlogs
  - [ ] Extract current state sections
  - [ ] Generate priorities list
  - [ ] Show last session context
- [ ] Search Worker (enhanced)
- [ ] Parser Worker (on deploy webhook)
- [ ] **[DECISION]** Daily notes structure
- [ ] Daily Notes Worker
  - [ ] Today's note template
  - [ ] Date range retrieval
  - [ ] Bi-weekly aggregation
- [ ] Metrics Worker
  - [ ] Task completion tracking
  - [ ] Activity by project
  - [ ] Project health indicators

### Phase 4: Advanced Integration

**Effort:** Ongoing | **Status:** Future

- [ ] Session Context Worker
- [ ] Cross-Reference Worker
- [ ] Bi-weekly memo generator (NASA format)
- [ ] **[DECISION]** Logseq integration
- [ ] PWA features (offline, home screen)
- [ ] Visualization/graphs

### Icebox (Future Possibilities)

- AI-generated project summaries
- Voice memo → daily note transcription
- Calendar integration
- RSS feed of updates
- Webhook notifications
- Supernote OCR → daily notes automation

---

## What's Working ✅

- [x] Concept validated through discussion
- [x] Technical feasibility confirmed
- [x] Architecture fully documented
- [x] Cloudflare account active (Danril3y@gmail.com)
- [x] GitHub workflow established
- [x] Hosting pattern proven (volkyra.com)
- [x] Project backlog created
- [x] Technical architecture documented
- [x] Project summary created

---

## Technical Notes

### Cloudflare Free Tier Limits (Plenty of Headroom)

| Resource         | Limit     | Expected Usage |
| ---------------- | --------- | -------------- |
| Pages requests   | Unlimited | ~10-50/day     |
| Workers requests | 100k/day  | ~100-500/day   |
| KV reads         | 100k/day  | ~50-200/day    |
| KV writes        | 1k/day    | ~10-50/day     |
| KV storage       | 1 GB      | <10 MB         |

### Data Flow

```
Dan + Claude Code → Edit markdown
        ↓
   jj commit + push
        ↓
      GitHub
        ↓
   ┌────┴────┐
   ↓         ↓
Pages     Webhook
   ↓         ↓
Static   Parser Worker
   ↓         ↓
   └────┬────┘
        ↓
   Workers KV (cached data)
        ↓
   Edge Network
        ↓
   User Device
```

### Key Files to Create (Phase 1)

```
/
├── index.html          # Docsify entry point
├── _sidebar.md         # Navigation
├── README.md           # Home page
├── backlogs/
│   ├── project-dashboard.md
│   ├── homelab.md
│   ├── rocets-gui.md
│   └── ...
└── daily-notes/
    └── (TBD structure)
```

---

## Integration Map

```
┌─────────────────────────────────────────────────────────────┐
│                 AI-Collaborative Dashboard                   │
│              (projects.toomanytabs.space?)                  │
└──────────────────────────┬──────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│   Homelab     │  │ PM Methodology │  │  ROCETS GUI   │
│  (AstralJxn)  │  │   (Meta!)      │  │   (NASA)      │
└───────────────┘  └───────────────┘  └───────────────┘
        │
        └── Shares: Cloudflare account, DNS, proven patterns
```

---

# III. Conversation Context Summary

## Key Discussion Points

1. **The "brilliant idea" moment**: Dan connected homelab hosting pattern with PM methodology need—exactly the lateral thinking the methodology is designed to enable
2. **Cloudflare Workers viability**: Confirmed Workers can add dynamic/interactive features while keeping static markdown foundation
3. **Daily notes = bi-weekly memos**: Identified that daily notes workflow directly addresses a real pain point (NASA manager's reinstated memo requirement)
4. **Meta observation**: We're documenting a project management methodology using the methodology itself 🤯

## Notable Insights

1. **"This is the payoff happening fast"** - Associative connections working within days of consolidation, not months
2. **"Infrastructure investments compound"** - Homelab work directly enables this project with no new learning
3. **"The best organizational system is the one you'll actually use"** - Mobile-first philosophy applies here too
4. **Daily notes motivation problem solved** - Abstract "future you will appreciate this" becomes concrete "5-minute memos instead of 30"

## Unresolved Questions

1. Repository structure: mono-repo vs multi-repo?
2. Subdomain naming preference?
3. Daily notes file organization?
4. How tightly to integrate with Logseq?
5. What metadata belongs in frontmatter?
6. How to handle work-sensitive content?
7. Session logging: manual entry, Claude Code automation, or git commits?

## Potential Pivot Points

- If Docsify proves limiting → Could migrate to proper SSG
- If search needs scale → Could add Cloudflare Vectorize
- If daily notes friction persists → Could add voice memo transcription
- If mobile experience insufficient → Could build native app wrapper

## What I've Learned About Dan

### Work Context

- NASA aerospace engineer, 13 years on ROCETS (Fortran-based rocket engine simulator)
- Strict IT restrictions (web-only cloud access)—self-hosted web solutions work perfectly
- Manager reinstating bi-weekly accomplishment memos → real pain point

### Technical Profile

- Python: 2.0-2.5/5 (self-assessed)
- New-ish to OOP, learning
- Uses Nushell as primary shell
- Uses jj (Jujutsu) over Git
- Prefers understanding "why" not just "how"
- Learning style: examples, metaphors, visuals

### Personal Context

- Wife: Laura (volkyra.com is her site)
- House project: Custom Queen Anne Victorian, ~1,000 inspiration photos
- Recently purchased Supernote A6 X2 for analog notes
- Location: Huntsville, Alabama (NASA MSFC area)

### Working Preferences

- TL;DR at start of complex responses
- Section TL;DRs for multi-part answers
- Reprint instructions after debugging
- Very specific about which machine to run commands on
- Welcomes constructive criticism on code architecture
- Wants to learn, not just get answers

### Known Challenges (Self-Identified)

- Context-switching difficulties
- Project abandonment at 90% completion
- Perfectionism → analysis paralysis
- Fragmented notes across platforms
- Spent 2.5+ weeks in planning mode before implementing accountability measures

### Core Values

- Privacy-first (prevent companies profiting from personal data)
- Self-hosted over commercial services
- "Practice like you play" - understand deeply through doing
- "NO REINVENTING THE WHEEL" - use existing frameworks
- "Make it work, make it right, THEN make it pretty"

## Additional Goals/Interests Mentioned

- Grocery order data extraction (Kroger)
- NASA timecard app (replace 32.7MB Excel spreadsheet)
- iMessage/SMS web interface
- Book cataloging with barcode scanning (for Laura's library)
- Photo management for architectural inspiration (Immich)
- DIY NAS build (TrueNAS SCALE, ZFS RAID-Z1, 24-40TB)

---

## Appendix: Session Timeline

### 2025-01-28 (Evening)

**Trigger:** Dan says "I think I just had a brilliant idea"

**Flow:**

1. Dan connects homelab hosting pattern with PM methodology
2. I validate technical feasibility
3. We explore Cloudflare Workers capabilities
4. Dan describes morning check-in vision
5. Daily notes → bi-weekly memo connection identified
6. I (embarrassingly) describe files without creating them
7. Dan calls me out
8. Files actually created and shared

**Artifacts Produced:**

- project-dashboard-backlog.md
- cloudflare-workers-architecture.md
- project-summary.md
- This PMR

**Decisions Deferred:**

- Repository structure
- Subdomain name
- Daily notes organization
- Logseq integration depth
- Authentication approach

**Meta Moment:**

> "This is getting meta - we're documenting a project management methodology using the methodology itself!" 🤣

---

## Quick Reference for New Conversations

Copy these key points:

- "Building AI-Collaborative Project Dashboard using Cloudflare Pages + Workers"
- "GitHub repo auto-deploys to static site rendered by Docsify"
- "Workers add dynamic features: morning dashboard, search, daily notes, metrics"
- "Goal: morning check-in experience + bi-weekly NASA memo automation"
- "Phase 1 = just Docsify markdown rendering (1 afternoon)"
- "Decisions pending: repo structure, subdomain, daily notes format"
- "This project IS the PM methodology in action—proof of concept"
