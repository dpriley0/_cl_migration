# Project Backlog: AI-Collaborative Project Dashboard

## Purpose & Context

**What is this project?**
A self-hosted, web-accessible dashboard for viewing and interacting with project backlogs managed through Claude Code collaboration. Integrates GitHub version control with Cloudflare Pages/Workers for a living documentation system.

**Why does it matter?**

- Provides a single, web-accessible view of all project states
- Enables "morning check-in" workflow for daily priorities
- Auto-generates documentation from AI collaboration sessions
- Eliminates manual copy/paste between Claude Project spaces
- Supports bi-weekly NASA accomplishment memo generation from daily notes
- Demonstrates the power of associative, non-hierarchical project management

**How did it originate?**
Dan made a lateral connection between two previously separate domains:

1. Homelab project: GitHub → Cloudflare Pages hosting pattern (used for volkyra.com)
2. PM Methodology project: Need for accessible, version-controlled project artifacts

This synthesis exemplifies the exact associative thinking the methodology is designed to enable.

---

## Current State

**Status:** Architecture documented, awaiting implementation decisions

**What exists:**

- ✅ Concept validated
- ✅ Technical feasibility confirmed
- ✅ GitHub → Cloudflare Pages pattern proven (via volkyra.com migration)
- ✅ Cloudflare Tunnel infrastructure operational (via AstralJunction homelab)
- ✅ Architecture documentation created
- ✅ Project summary narrative created
- ✅ This backlog created

**What's blocking progress:**

- ⏳ Decision: Repository structure (mono-repo vs multi-repo)
- ⏳ Decision: Subdomain name selection
- ⏳ Decision: Daily notes integration approach
- ⏳ Dan's availability (weekend project timing)

**Recent activity:**

- 2025-01-28: Initial concept discussion and validation
- 2025-01-28: Created architecture documentation
- 2025-01-28: Created project summary and backlog
- 2025-01-28: Identified key decision points requiring Dan's input

---

## On The Horizon

### Phase 1: Foundation (Get It Working)

**Goal:** Basic markdown rendering accessible via web

- [ ] **[DECISION NEEDED]** Choose repository structure
  - Option A: Separate repo for dashboard only
  - Option B: Mono-repo with all backlogs + dashboard code (recommended)
  - Option C: Dashboard repo + separate backlogs repo
- [ ] Create GitHub repository
- [ ] Set up basic folder structure for project backlogs
- [ ] Deploy Docsify for markdown rendering
  - Just an index.html + configuration
  - Zero build process
  - Auto-renders all markdown files
- [ ] Connect repo to Cloudflare Pages
- [ ] **[DECISION NEEDED]** Choose subdomain
  - `projects.toomanytabs.space`
  - `dashboard.toomanytabs.space`
  - `pm.toomanytabs.space`
  - Other?
- [ ] Verify auto-deployment on git push
- [ ] Migrate existing project backlogs into repo

### Phase 2: Polish (Make It Right)

**Goal:** Usable daily driver with good navigation

- [ ] Implement Docsify search functionality
- [ ] Create custom sidebar navigation
- [ ] Add project status indicators
- [ ] Implement dark mode / theme selection
- [ ] Create index/home page with project overview
- [ ] Add mobile-responsive styling
- [ ] Test on iPhone (mobile-first validation)

### Phase 3: Dynamic Features (Make It Smart)

**Goal:** Cloudflare Workers for interactive dashboard

- [ ] Set up Cloudflare Workers environment
- [ ] Implement Morning Dashboard Worker
  - Parse all backlogs
  - Extract "Current State" sections
  - Generate daily priorities
  - Show "last session" context
- [ ] Implement Search Worker
  - Full-text search across all documents
  - Filter by project, status, date
- [ ] Implement Metrics Worker
  - Track tasks completed
  - Project activity graphs
  - Progress over time
- [ ] **[DECISION NEEDED]** Daily notes integration approach
  - Specific directory structure?
  - Frontmatter tags?
  - Separate "Daily Log" section?
- [ ] Implement Daily Notes Worker
  - Template generation
  - Bi-weekly aggregation
  - NASA memo formatting

### Phase 4: Advanced Integration

**Goal:** Full ecosystem integration

- [ ] Bi-weekly memo generator
  - Pull daily notes from date range
  - Extract accomplishments from backlogs
  - Format for NASA manager requirements
- [ ] **[DECISION NEEDED]** Logseq integration depth
  - Reference only?
  - Bidirectional sync?
  - Separate systems?
- [ ] PWA features for mobile
  - Offline access to recent data
  - Home screen shortcut
  - Push notifications (optional)
- [ ] Cross-project linking
  - Auto-detect references between projects
  - Generate relationship graphs
- [ ] Session context capture
  - Auto-log Claude Code sessions
  - Link to relevant backlog items

### Icebox (Future Possibilities)

- AI-generated project summaries
- Voice memo → daily note transcription
- Calendar integration for milestone tracking
- RSS feed of project updates
- Slack/Discord webhook notifications
- Supernote OCR → daily notes pipeline

---

## Key Learnings & Principles

### Validated

1. **Associative thinking pays off fast** - The connection between homelab hosting and PM methodology came within days of consolidating projects, not months
2. **Infrastructure investments compound** - Cloudflare Pages/Workers pattern from homelab directly enables this project
3. **Documentation through conversation works** - This entire project spec emerged from natural discussion, not forced planning sessions

### Emerging

- [To be filled as we build]

### Technical Decisions Made

1. **Docsify over Jekyll/Hugo** - Zero build process, immediate feedback, good enough for v1
2. **Cloudflare Workers over traditional backend** - Free tier, edge performance, already in our stack
3. **Markdown-first** - Works with Claude Code, git-friendly, portable
4. **Mobile-first design** - Dan's primary consumption device is iPhone

---

## Collaboration Notes

### 2025-01-28 Session

**Context:** Evening conversation, Dan had "brilliant idea" connecting homelab and PM methodology

**Key Moments:**

- Dan made the lateral connection between GitHub+Pages hosting and project artifact storage
- Validated that Cloudflare Workers can add dynamic features
- Discussed morning check-in dashboard vision
- Identified daily notes as critical for NASA bi-weekly memos
- Created comprehensive documentation (meta: using the methodology to document the methodology)

**Dan's Vision for Morning Check-In:**

- Summary of where we left off last session
- Backlog summaries across all projects
- Upcoming milestones
- Priorities and recommended actions
- Daily notes entry point

**Decisions Deferred:**

- Repository structure
- Subdomain name
- Daily notes integration approach
- Logseq integration depth
- Mobile experience priorities

---

## Integration Points

### With Homelab Project (AstralJunction)

- Uses same Cloudflare account and patterns
- May eventually run Workers that interact with homelab services
- Shares DNS management approach
- Benefits from tunnel infrastructure if needed

### With PM Methodology Project

- This IS the implementation of that methodology
- Serves as proof-of-concept
- Backlogs follow the established template
- Demonstrates fluid, conversational project tracking

### With NASA Work

- Daily notes feed into bi-weekly accomplishment memos
- Could track ROCETS-related tasks
- Respects work/personal separation
- Web-accessible from NASA network

### With Logseq PKM

- **[DECISION NEEDED]** How tightly to integrate
- Could serve as complementary view
- Daily notes might live in both systems
- Knowledge graph could reference project backlogs

---

## Technical Stack

### Confirmed

- **Hosting:** Cloudflare Pages (static) + Workers (dynamic)
- **Source Control:** GitHub (+ jj for local workflow)
- **Markdown Rendering:** Docsify (Phase 1)
- **Domain:** *.toomanytabs.space (subdomain TBD)

### Planned

- **Workers Runtime:** Cloudflare Workers (JavaScript)
- **Data Format:** Markdown with YAML frontmatter
- **Search:** Docsify built-in → Workers-enhanced
- **Styling:** Custom CSS (mobile-first)

### Potential Future

- **PWA Framework:** Workbox or vanilla service worker
- **Visualization:** Mermaid diagrams, D3.js for graphs
- **Sync:** Potentially Logseq integration

---

## Open Questions

1. How should project backlogs be organized in the repo?
   
   - By project name?
   - By domain (work/personal/homelab)?
   - Flat structure?

2. What metadata should be in frontmatter?
   
   - Status?
   - Priority?
   - Last updated?
   - Tags?

3. How to handle sensitive/work content?
   
   - Separate private repo?
   - Cloudflare Access authentication?
   - Just keep it vague?

4. What's the MVP for "morning check-in"?
   
   - Just rendered markdown?
   - Basic status page?
   - Full dynamic dashboard?

---

## Success Criteria

**Phase 1 Success:**

- Can view all project backlogs from phone
- Auto-updates when pushing to GitHub
- Takes less than 5 minutes to set up

**Phase 2 Success:**

- Pleasant to use daily
- Search works well
- Navigation is intuitive

**Phase 3 Success:**

- Morning check-in provides actionable daily view
- Daily notes workflow is frictionless
- Bi-weekly memo takes 5 minutes instead of 30

**Overall Success:**

- Dan actually uses it every day
- Reduces cognitive overhead of project management
- Enables more lateral connections between projects
- Makes bi-weekly NASA memos trivial
