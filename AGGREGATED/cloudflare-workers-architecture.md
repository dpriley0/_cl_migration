# Cloudflare Workers Architecture: AI-Collaborative Project Dashboard

## Overview

This document describes the technical architecture for adding dynamic, interactive features to the static markdown-based project dashboard using Cloudflare Workers.

### Design Principles

1. **Progressive Enhancement** - Static site works without Workers; Workers add richness
2. **Edge Performance** - All computation at Cloudflare's edge, near the user
3. **Zero Backend Infrastructure** - No servers to maintain beyond AstralJunction
4. **Free Tier Friendly** - Design within Cloudflare's generous free limits
5. **Mobile-First** - Optimized for iPhone as primary consumption device

### System Context

```
┌─────────────────────────────────────────────────────────────────┐
│                        User Devices                              │
│   (iPhone, MacBook, NASA workstation, any browser)              │
└─────────────────────────┬───────────────────────────────────────┘
                          │ HTTPS
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Cloudflare Edge Network                       │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │ Cloudflare Pages│  │    Workers      │  │  Workers KV     │ │
│  │ (Static Assets) │  │ (Dynamic Logic) │  │ (Cached Data)   │ │
│  │                 │  │                 │  │                 │ │
│  │ • Docsify       │  │ • Dashboard API │  │ • Parsed data   │ │
│  │ • Markdown files│  │ • Search        │  │ • Metrics       │ │
│  │ • CSS/JS        │  │ • Aggregation   │  │ • Session state │ │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘ │
│           │                    │                     │          │
│           └────────────────────┴─────────────────────┘          │
└─────────────────────────┬───────────────────────────────────────┘
                          │ On deploy
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                         GitHub                                   │
│                   (Source of Truth)                             │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Repository: [TBD - project-dashboard or mono-repo]      │   │
│  │                                                          │   │
│  │  /backlogs/           - Project markdown files           │   │
│  │  /daily-notes/        - Daily log entries                │   │
│  │  /workers/            - Worker source code               │   │
│  │  /docs/               - Docsify configuration            │   │
│  │  /_headers             - Cloudflare headers config       │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────┬───────────────────────────────────────┘
                          │ jj/git push
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Local Development                             │
│              (MacBook + Claude Code)                            │
│                                                                  │
│  • Edit markdown backlogs                                       │
│  • Write daily notes                                            │
│  • Develop Worker functions                                     │
│  • Test locally with Wrangler                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Worker Functions

### 1. Morning Dashboard Worker (`/api/dashboard`)

**Purpose:** Generate personalized daily briefing from current project state

**Endpoint:** `GET /api/dashboard`

**Response:**

```json
{
  "greeting": "Good morning, Dan!",
  "lastSession": {
    "date": "2025-01-27",
    "project": "homelab",
    "summary": "Configured Cloudflare Tunnel, deployed test page"
  },
  "priorities": [
    {
      "project": "project-dashboard",
      "task": "Choose repository structure",
      "reason": "Blocking all other implementation work"
    },
    {
      "project": "homelab",
      "task": "Deploy Paperless-ngx",
      "reason": "Ready to deploy, pattern established"
    }
  ],
  "activeProjects": [
    {
      "name": "AI-Collaborative Project Dashboard",
      "status": "Architecture phase",
      "lastUpdated": "2025-01-28",
      "nextAction": "Review decision points"
    }
  ],
  "upcomingMilestones": [],
  "dailyNotePrompt": "What are you working on today?"
}
```

**Logic:**

1. Fetch all backlog files from KV cache (or parse from Pages if cache miss)
2. Extract "Current State" sections
3. Identify items marked as priorities or blocking
4. Find most recent collaboration notes for "last session"
5. Sort and rank by urgency/relevance
6. Return structured JSON for frontend rendering

**Caching Strategy:**

- Parse backlogs on deploy (via GitHub webhook or scheduled)
- Store parsed data in Workers KV
- TTL: Until next deploy (cache invalidation on push)

---

### 2. Search Worker (`/api/search`)

**Purpose:** Full-text search across all project documentation

**Endpoint:** `GET /api/search?q={query}&project={optional}&status={optional}`

**Request Parameters:**

- `q` (required): Search query string
- `project` (optional): Filter to specific project
- `status` (optional): Filter by status (active, horizon, icebox)
- `type` (optional): Filter by content type (backlog, daily-note, doc)

**Response:**

```json
{
  "query": "cloudflare workers",
  "results": [
    {
      "file": "backlogs/project-dashboard.md",
      "project": "AI-Collaborative Project Dashboard",
      "section": "On The Horizon > Phase 3",
      "snippet": "...Implement **Cloudflare Workers** for interactive dashboard...",
      "score": 0.95,
      "url": "/backlogs/project-dashboard#phase-3"
    }
  ],
  "totalResults": 5,
  "searchTime": "12ms"
}
```

**Implementation Options:**

*Option A: Simple (Phase 1)*

- Store pre-tokenized content in KV
- Basic substring/keyword matching
- Good enough for <100 documents

*Option B: Advanced (Phase 3+)*

- Use Cloudflare Vectorize for semantic search
- Enable "find similar" functionality
- Natural language queries ("what was I working on last week?")

**[DECISION NEEDED]:** Start with Option A, upgrade to B if needed?

---

### 3. Metrics Worker (`/api/metrics`)

**Purpose:** Track and visualize project progress over time

**Endpoint:** `GET /api/metrics?range={timerange}`

**Response:**

```json
{
  "range": "30d",
  "tasksCompleted": 12,
  "tasksByProject": {
    "homelab": 7,
    "project-dashboard": 3,
    "volkyra": 2
  },
  "activityByDay": [
    { "date": "2025-01-28", "commits": 3, "notesWritten": 1 },
    { "date": "2025-01-27", "commits": 5, "notesWritten": 1 }
  ],
  "projectHealth": [
    { "project": "homelab", "status": "active", "daysSinceUpdate": 1 },
    { "project": "volkyra", "status": "stale", "daysSinceUpdate": 14 }
  ]
}
```

**Data Sources:**

- Git commit history (via GitHub API, cached)
- Backlog file modification dates
- Completed task counts (parsed from backlogs)
- Daily notes frequency

**Visualization Ideas:**

- Activity heatmap (GitHub-style)
- Project status cards
- Burndown/burnup charts (if tracking estimates)
- Time-to-completion trends

---

### 4. Daily Notes Worker (`/api/daily-notes`)

**Purpose:** Manage daily note creation and bi-weekly aggregation

**Endpoints:**

`GET /api/daily-notes/today`

- Returns today's note if exists, or template if not

`GET /api/daily-notes/range?start={date}&end={date}`

- Returns all notes in date range

`GET /api/daily-notes/biweekly-summary`

- Aggregates last 2 weeks into NASA memo format

**Bi-Weekly Summary Response:**

```json
{
  "period": "2025-01-13 to 2025-01-27",
  "accomplishments": [
    "Migrated volkyra.com from AWS to Cloudflare Pages",
    "Established Cloudflare Tunnel infrastructure on AstralJunction",
    "Designed AI-Collaborative Project Dashboard architecture",
    "Created comprehensive project documentation"
  ],
  "projectUpdates": [
    {
      "project": "Homelab",
      "summary": "Core infrastructure operational, ready for service deployment"
    }
  ],
  "upcomingWork": [
    "Deploy Paperless-ngx for document management",
    "Implement project dashboard Phase 1"
  ],
  "formattedMemo": "## Accomplishments (Jan 13-27)\n\n- Migrated volkyra.com..."
}
```

**Daily Note Template:**

```markdown
# Daily Notes: {date}

## What I'm Working On
- 

## Accomplishments
- 

## Blockers/Challenges
- 

## Notes for Future Self
- 

## Links to Today's Work
- 
```

**[DECISION NEEDED]:** Daily notes storage location

- Option A: `/daily-notes/2025/01/2025-01-28.md` (year/month folders)
- Option B: `/daily-notes/2025-01-28.md` (flat with date prefix)
- Option C: Single file per month with dated sections

---

### 5. Project Parser Worker (Background)

**Purpose:** Parse markdown backlogs into structured data on deploy

**Trigger:** GitHub webhook on push to main branch

**Process:**

1. Receive webhook from GitHub
2. Fetch changed files (or all backlogs if full rebuild)
3. Parse markdown structure:
   - Extract frontmatter (YAML metadata)
   - Identify sections (Purpose, Current State, On The Horizon, etc.)
   - Parse task lists (checkbox items)
   - Extract dates, mentions, links
4. Store parsed data in Workers KV
5. Invalidate relevant caches

**Parsed Data Schema:**

```json
{
  "project-dashboard": {
    "title": "AI-Collaborative Project Dashboard",
    "status": "active",
    "lastUpdated": "2025-01-28T22:30:00Z",
    "sections": {
      "purpose": "A self-hosted, web-accessible dashboard...",
      "currentState": {
        "status": "Architecture documented",
        "completed": ["Concept validated", "Technical feasibility confirmed"],
        "blocking": ["Repository structure decision", "Subdomain selection"]
      },
      "horizon": {
        "phase1": { "title": "Foundation", "tasks": [...] },
        "phase2": { "title": "Polish", "tasks": [...] }
      }
    },
    "tasks": {
      "total": 34,
      "completed": 6,
      "inProgress": 2,
      "blocked": 4
    },
    "collaborationNotes": [
      { "date": "2025-01-28", "summary": "Initial concept discussion..." }
    ]
  }
}
```

---

### 6. Session Context Worker (`/api/session`)

**Purpose:** Track and retrieve context from Claude Code sessions

**Endpoints:**

`POST /api/session/log`

- Log a session summary (called manually or via automation)

`GET /api/session/last`

- Get most recent session for "where we left off"

`GET /api/session/project/{projectName}`

- Get recent sessions for specific project

**Session Schema:**

```json
{
  "id": "session-2025-01-28-001",
  "timestamp": "2025-01-28T22:30:00Z",
  "project": "project-dashboard",
  "summary": "Designed Cloudflare Workers architecture, created documentation",
  "keyDecisions": [
    "Use Docsify for Phase 1 rendering",
    "Workers for dynamic features in Phase 3"
  ],
  "nextSteps": [
    "Review decision points",
    "Choose repository structure"
  ],
  "artifacts": [
    "project-dashboard-backlog.md",
    "cloudflare-workers-architecture.md"
  ]
}
```

**[DECISION NEEDED]:** How to log sessions?

- Option A: Manual entry via dashboard
- Option B: Claude Code automation (end of session prompt)
- Option C: Parse from git commits + collaboration notes

---

### 7. Cross-Reference Worker (`/api/references`)

**Purpose:** Find connections between projects and documents

**Endpoint:** `GET /api/references/{documentPath}`

**Response:**

```json
{
  "document": "backlogs/project-dashboard.md",
  "referencedBy": [
    {
      "file": "backlogs/homelab.md",
      "context": "Uses same Cloudflare Pages pattern"
    }
  ],
  "references": [
    {
      "file": "backlogs/homelab.md",
      "context": "Cloudflare Tunnel infrastructure"
    },
    {
      "file": "backlogs/pm-methodology.md",
      "context": "Implementation of methodology"
    }
  ],
  "relatedTopics": ["cloudflare", "documentation", "automation"]
}
```

**Use Cases:**

- "What other projects relate to this one?"
- Visualize project dependency graph
- Suggest relevant context when viewing a project

---

## Data Flow Architecture

### On Git Push (Deploy Flow)

```
1. Developer pushes to GitHub
         │
         ▼
2. GitHub triggers Cloudflare Pages build
         │
         ├──► Static files deployed to Pages CDN
         │
         ▼
3. GitHub webhook triggers Parser Worker
         │
         ▼
4. Parser Worker fetches changed files
         │
         ▼
5. Markdown parsed into structured JSON
         │
         ▼
6. Parsed data stored in Workers KV
         │
         ▼
7. Cache invalidation for affected endpoints
         │
         ▼
8. Dashboard reflects changes within ~30 seconds
```

### On User Request (Read Flow)

```
1. User opens dashboard on iPhone
         │
         ▼
2. Browser requests /api/dashboard
         │
         ▼
3. Morning Dashboard Worker executes at edge
         │
         ├──► Check Workers KV for cached data
         │         │
         │         ├── Cache HIT: Return parsed data
         │         │
         │         └── Cache MISS: Parse from Pages, cache, return
         │
         ▼
4. Worker returns JSON response
         │
         ▼
5. Frontend renders personalized dashboard
         │
         ▼
6. Total latency: <100ms (edge computing + CDN)
```

---

## Workers KV Schema

**Namespace:** `PROJECT_DASHBOARD`

**Keys:**

| Key Pattern              | Value                 | TTL          |
| ------------------------ | --------------------- | ------------ |
| `backlog:{project-name}` | Parsed backlog JSON   | Until deploy |
| `daily-note:{date}`      | Parsed daily note     | Until deploy |
| `metrics:{range}`        | Computed metrics      | 1 hour       |
| `session:latest`         | Most recent session   | Until deploy |
| `session:{id}`           | Individual session    | 90 days      |
| `search-index`           | Tokenized search data | Until deploy |
| `github:commits:{range}` | Cached commit data    | 1 hour       |

**Storage Limits (Free Tier):**

- 1 GB total storage
- 100,000 reads/day
- 1,000 writes/day
- More than enough for personal use

---

## API Authentication

### Options

**Option A: Public (Simplest)**

- No auth required
- Rely on obscurity (unique subdomain)
- Fine for non-sensitive project data

**Option B: Cloudflare Access**

- Requires authentication to access entire site
- Uses WebAuthn/passkeys (Face ID for Dan, fingerprint for Laura)
- Already planned for AstralJunction services
- Recommended for production

**Option C: API Keys**

- Simple bearer token for API endpoints
- Dashboard pages public, API protected
- Good middle ground

**[DECISION NEEDED]:** Start with Option A for development, add Option B before going "production"?

---

## Security Considerations

1. **No Sensitive Data in Backlogs**
   
   - Keep NASA work details vague
   - No credentials, tokens, or secrets
   - Personal but not private

2. **CORS Configuration**
   
   - Restrict API access to dashboard domain only
   - Prevent unauthorized embedding

3. **Rate Limiting**
   
   - Cloudflare provides automatic DDoS protection
   - Workers have built-in CPU time limits

4. **Input Validation**
   
   - Sanitize search queries
   - Validate date ranges
   - Prevent injection attacks

---

## Performance Optimization

### Edge Caching Strategy

- Static assets: Cached indefinitely (hash in filename)
- Parsed backlog data: Cached until deploy
- Computed metrics: 1-hour TTL
- Search results: Not cached (fast enough)

### Lazy Loading

- Dashboard loads core data first
- Metrics and charts load async
- Search index loaded on first search

### Mobile Optimization

- Minimal JavaScript bundle
- Critical CSS inlined
- Images lazy-loaded
- Service Worker for offline (Phase 4)

---

## Implementation Phases

### Phase 1: Static Foundation

- Docsify renders markdown
- No Workers yet
- Basic search via Docsify plugin
- **Timeline:** 1 afternoon

### Phase 2: Basic Workers

- Morning Dashboard Worker
- Search Worker (simple version)
- Parser Worker (on deploy)
- **Timeline:** 1-2 weekends

### Phase 3: Full Dynamic

- Metrics Worker
- Daily Notes Worker
- Session Context Worker
- **Timeline:** 2-3 weekends

### Phase 4: Advanced Features

- Cross-Reference Worker
- Semantic search (Vectorize)
- PWA offline support
- Bi-weekly memo generator
- **Timeline:** Ongoing enhancement

---

## Development Workflow

### Local Development

```bash
# Install Wrangler (Cloudflare Workers CLI)
npm install -g wrangler

# Authenticate
wrangler login

# Run locally with live reload
wrangler dev

# Deploy to production
wrangler deploy
```

### Testing

- Use Wrangler's local mode for development
- Miniflare for unit testing Workers
- Manual testing on staging subdomain

### Deployment

- Workers deploy via Wrangler CLI
- Pages deploy automatically on git push
- Both can be triggered from Claude Code session

---

## Cost Analysis

**Cloudflare Free Tier Includes:**

- Pages: Unlimited static requests
- Workers: 100,000 requests/day
- KV: 100,000 reads/day, 1,000 writes/day
- Bandwidth: Unlimited

**Expected Usage:**

- Dashboard views: ~10-50/day
- API calls: ~100-500/day
- KV operations: ~50-200/day

**Verdict:** Comfortably within free tier. No cost.

---

## Open Technical Questions

1. **Worker bundling:** Single Worker with routing vs. multiple Workers?
   
   - Single is simpler to manage
   - Multiple provides better isolation
   - Recommendation: Start single, split if needed

2. **Markdown parser:** Which library?
   
   - marked.js (fast, minimal)
   - remark (powerful, plugin ecosystem)
   - gray-matter (for frontmatter)
   - Recommendation: gray-matter + marked.js

3. **State management:** How to handle dashboard state?
   
   - URL query params (shareable, bookmarkable)
   - Workers KV for persistence
   - Recommendation: URL params for view state, KV for user preferences

4. **Offline support:** Service Worker complexity worth it?
   
   - PWA adds development overhead
   - Useful for airplane/NASA network issues
   - Recommendation: Defer to Phase 4, evaluate need first

---

## Next Steps

1. **Dan decides:** Repository structure, subdomain, daily notes format
2. **Create repo:** Initialize with basic structure
3. **Deploy Docsify:** Get static rendering working
4. **First Worker:** Morning Dashboard (highest value)
5. **Iterate:** Add features based on actual usage patterns
