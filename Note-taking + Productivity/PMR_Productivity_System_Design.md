# Project Migration Record (PMR): Productivity System Design

**Project Owner:** Dan Riley  
**Location:** Huntsville, AL  
**Organization:** NASA (Aerospace Engineer)  
**Migration Date:** January 29, 2026  
**PMR Version:** 1.0  
**Current Status:** Discovery Interview Phase (Question 14 of ~25)

---

> ⚠️ **POST-MIGRATION PRIORITY:** Finishing the discovery interview (Questions 14-25) should be the FIRST priority after consolidation. The system architecture cannot be properly designed until all discovery questions are answered. Dan has momentum—don't let it stall!

---

# I. Comprehensive Project Summary

## 1. Project Vision

### 1.1 High-Level Goals

Build a comprehensive, personalized productivity system that addresses core workplace challenges:

1. **Context-Switching Difficulties** - Severe difficulty transitioning between tasks; getting pulled into rabbit holes when attempting quick checks
2. **Activation Energy Problems** - Difficulty starting work, especially when facing email overwhelm or undocumented obligations
3. **Project Abandonment at 90%** - Persistent pattern of abandoning projects near completion due to perfectionism and optimization spirals
4. **Scattered Note-Taking** - "Write-only" notes across multiple applications that become unfindable and unusable
5. **Undocumented Planning** - Mental task lists that create anxiety and cognitive load

**Ultimate Vision:** Transform scattered, panic-driven capture across multiple apps into a coherent knowledge and project management architecture that accommodates neurodivergent processing while enabling sustained productivity.

### 1.2 Motivations

- **Neurodivergent Processing Style:** Dan has ADHD, OCD, and autism spectrum traits that affect how he captures, processes, and retrieves information
- **Work Environment Constraints:** Long uninterrupted blocks with minimal meetings (2-5 per week) that rarely translate to productive outcomes
- **Career Leverage:** 12 years of accumulated engineering knowledge trapped in an 80-page Word document and scattered notes—needs to be unlocked
- **Urgent Capture Needs:** The system must accommodate "write it down NOW" panic moments when working memory is full
- **Visual Learning Style:** Strong preference for visual organization; experiences "blank canvas anxiety" with empty interfaces

### 1.3 Apps, Tools, Utilities, and Services of Interest

**Note-Taking & Knowledge Management (Extensively Researched):**

| Tool | Status | Key Notes |
|------|--------|-----------|
| **Logseq** | Strong Candidate | Block-based, file-based (markdown), Zettelkasten-friendly, BUT concerns about slowing development and new database version breaking self-hosting |
| **Obsidian** | Strong Candidate | More stable, larger ecosystem, Canvas plugin for visual work, better long-term bet, BUT block references feel "bolted on" vs native |
| **Heptabase** | Researched | Visual/canvas-based, good for blank canvas anxiety IF using Journal-first workflow, BUT not as strong on technical documentation |
| **Roam Research** | Ruled Out | Too expensive ($15/mo), cloud-only |
| **UpNote** | Currently Using | Polished, approachable, supports tagging and linking, BUT encourages hierarchical thinking |
| **Remnote** | Considered | Good for spaced repetition, more academic-focused |
| **Workflowy** | Researched | "Live Copy" feature is marketing fluff—all block-reference apps do this |
| **Capacities** | Mentioned | Block-based with object types, ~$10/mo |
| **Athens Research** | Ruled Out | Development has slowed/potentially abandoned |
| **Tana** | Researched | Promising but early |
| **Zettlr** | Mentioned | Markdown-based |
| **Joplin** | Mentioned | Open source option |

**Visual/Canvas Tools (Researched for Visual Learning Style):**
- Mind mapping plugins for Obsidian (described as "mehh, not intuitive")
- Heptabase canvas features
- Scrintal, Muse, XMind, MindNode (mentioned in research)

**Document Management:**
- **Paperless-ngx** - Transitioning TO (from Zotero), OCR, tag-based organization
- **Zotero** - Transitioning FROM

**File Management:**
- **Files App** (files.community) - ADOPTED as Windows Explorer replacement
- Frustrated by Microsoft removing Quick Access reordering in Explorer

**Sync & Self-Hosting:**
- **Syncthing** - For file synchronization (preferred approach)
- Strong preference for local data ownership and self-hosted solutions
- Logseq's new database version would break this requirement

**Hardware:**
- **Supernote A6 X2** - E-ink tablet for analog note-taking (recently purchased)
- Planning OCR automation: Supernote → OCR → Logseq workflow

**Other Current Tools:**
- Evernote (notes scattered here)
- OneNote (notes scattered here)
- Word documents (notes scattered here)
- Physical notebooks (notes scattered here)

### 1.4 Strategic Objectives

1. **Implement Zettelkasten Methodology** - For developing interconnected knowledge (long-term memory)
2. **Integrate BASB (Building a Second Brain)** - For project management and deliverables (working memory)
3. **Solve "Classify Later" Problem** - System must enforce immediate, lightweight organization during capture
4. **Create Navigational Breadcrumbs** - Quick-reference notes for file locations (currently non-existent)
5. **Accommodate Urgent Capture** - Design FOR the panic-capture behavior, not against it
6. **Prevent Perfectionism Spirals** - Built-in guardrails against optimization rabbit holes

### 1.5 Long-Term Potential / Potential Workflows

**Envisioned Information Flow:**
```
Urgent Capture (panic moment)
    ↓
Quick Inbox (minimal friction)
    ↓
[Regular Processing Session]
    ↓
├── Zettelkasten (permanent knowledge)
├── BASB/Project Folders (active deliverables)
└── Archive (reference material)
```

**Supernote Integration Workflow:**
```
Handwritten Note (Supernote)
    ↓
Automated Upload
    ↓
OCR Processing
    ↓
Insert into PKB (with hashtag conventions or Supernote tags)
    ↓
Later Classification
```

**ROCETS Model Documentation Workflow:**
```
C:\ROCETS\[Model_Name]\     ←→     ..\Work Documents\[Customer]\
    (executable code)              (project context, research)
            ↓                              ↓
            └──────────────┬───────────────┘
                           ↓
                  Zettelkasten Notes
                  (engineering insights)
```

### 1.6 Unique Value Proposition

This isn't about implementing a generic productivity system—it's about building a **cognitive prosthesis** specifically designed for:
- An aerospace engineer's technical documentation needs (LaTeX, code blocks, equations)
- ADHD-driven capture urgency and working memory limitations
- OCD perfectionism tendencies that derail completion
- Visual learning preferences with blank canvas anxiety
- 12 years of accumulated domain knowledge needing liberation

### 1.7 Core Philosophies Established

1. **"Contexts, Not Topics"** - Organize by 2-3 broad contextual categories, not deep hierarchical topics. Let tags and links do the organizational work.

2. **Zettelkasten = Long-Term Memory** - For developing interconnected knowledge that transcends individual projects

3. **BASB = Working Memory** - For active project management and deliverables

4. **Emergence Over Prescription** - Structure emerges from connections, not predetermined folders

5. **Design FOR Urgent Capture** - The system must accommodate panic-capture, not fight it

6. **Minimum Viable System First** - Solve top 3 pain points before expanding; resist optimization rabbit holes

7. **"Classify Later" is a Trap** - This mirrors the exact procrastination pattern that creates the problem; must enforce immediate lightweight organization

---

## 2. Requirements Compilation

### 2.1 Explicit Requirements

**Functional Requirements:**
- [ ] Block-level referencing capability (like Roam/Logseq, not just page linking)
- [ ] LaTeX support for equations
- [ ] Code blocks for Fortran and Python
- [ ] Tagging system (hierarchical tags supported)
- [ ] Bidirectional linking
- [ ] Graph/network visualization
- [ ] Full-text search
- [ ] Mobile accessibility for quick capture

**Technical Requirements:**
- [ ] Local data ownership (files stored locally, not cloud-only)
- [ ] Self-hosted synchronization capability (Syncthing-compatible)
- [ ] Markdown-based storage (avoid proprietary formats)
- [ ] Cross-platform: Windows (work), Mac (personal), iPhone (mobile)
- [ ] Works with NASA IT restrictions (web-only cloud access if needed)

**Usability Requirements:**
- [ ] Low friction for urgent capture
- [ ] Visual organization options (canvas, mind maps)
- [ ] Not overwhelming for blank canvas anxiety
- [ ] Scannable format (like working cheat sheets)
- [ ] Predictable locations for common notes

### 2.2 Implied Requirements

- System must not require organizational decisions at capture time
- Processing/organization should be batchable (end of day/week)
- Must handle both "action-oriented" notes (cheat sheets) and "knowledge-building" notes (Zettelkasten)
- Integration path for existing 80-page PRO-TIPS document
- Integration path for hundreds of bookmarked websites
- Must accommodate dual-hierarchy of ROCETS models (C:\ROCETS\) vs. customer context (Work Documents)

### 2.3 Derived Requirements

Based on discovery interview responses:

- **File Location Index Required** - Dan has NO notes about file locations and relies on memory + Windows Search
- **Navigational Breadcrumbs** - Need quick-reference notes that function as "where did I save X?"
- **Project Context Consolidation** - Must bridge gap between ROCETS code folders and customer context folders
- **Cheat Sheet Pattern** - Successful notes are: action-oriented, predictable location, solve recurring problems
- **Anti-Duplication Mechanism** - Currently creates duplicate notes on same topics (4 separate JS framework research notes)

### 2.4 Potential Edge Cases or Expansion Areas

- Integration with Supernote OCR workflow
- Automated backup/versioning of knowledge base
- Shared notes with team members (NASA colleagues)
- Citation management for technical papers
- Time-tracking integration for NASA project work
- Template system for recurring note types

---

## 3. Architectural Overview

### 3.1 Current Design Decisions with Rationale

**Decision: Hybrid BASB + Zettelkasten Architecture**
- *Rationale:* Dan's needs span both "active project deliverables" (BASB strength) and "interconnected knowledge building" (Zettelkasten strength). Neither alone suffices.
- *Status:* Conceptually adopted, implementation pending tool selection

**Decision: "Contexts, Not Topics" Organization**
- *Rationale:* Dan's brain naturally works in associative networks, not hierarchies. Hierarchical organization fights natural retrieval patterns.
- *Proposed Structure:*
```
📓 Dan's PKB
├── 📁 Personal
├── 📁 Work - General
├── 📁 Work - [Active Project 1]
├── 📁 Work - [Active Project 2]
├── 📁 Inventions & Side Projects
├── 📁 Learning & Research
└── 📁 Archive (if needed)
```

**Decision: Block-Based Tool Required**
- *Rationale:* Page-level linking insufficient for Dan's granular cross-referencing needs. Block references enable "thought paths" instead of "folder paths."
- *Status:* Narrowed to Logseq vs Obsidian (with plugins)

**Decision: Files App Adopted for File Management**
- *Rationale:* Microsoft removed Quick Access reordering; Files restores this plus adds tabs and modern features
- *Status:* IMPLEMENTED ✅

**Decisions Under Consideration:**
- Logseq (file-based version) vs Obsidian: Pending resolution of Logseq development concerns
- Heptabase for visual work vs Obsidian Canvas plugin
- Supernote integration approach

**Ruled Out:**
- **Deep hierarchical organization** - Fights natural cognition
- **Roam Research** - Too expensive, cloud-only
- **Logseq Database Version** - Would require cloud sync, breaking self-hosting requirement
- **"Classify Later" workflows** - Mirrors procrastination pattern

### 3.2 Technology Stack

**Confirmed:**
- Windows (work computer)
- Mac (personal)
- iPhone (mobile)
- Files app (file manager)
- Syncthing (file sync)
- Paperless-ngx (document management)

**Pending Selection:**
- Primary PKB tool (Logseq vs Obsidian)
- Visual/canvas tool (Heptabase vs Obsidian Canvas)
- Handwriting integration (Supernote workflow)

### 3.3 Frameworks Decided

**Adopted:**
- Zettelkasten methodology (for knowledge development)
- BASB/PARA methodology (for project management)
- "Contexts, not topics" principle

**Explicitly Ruled Out:**
- GTD (Getting Things Done) - Not discussed as primary framework
- Pure hierarchical folder systems

### 3.4 Key Architectural Constraints

1. **NASA IT Restrictions** - May limit cloud-based tools; web access works
2. **Self-Hosting Requirement** - Must maintain local data ownership
3. **Cross-Platform Sync** - Must work across Windows/Mac/iPhone
4. **Markdown Interoperability** - Avoid lock-in to proprietary formats
5. **Anti-Perfectionism Guardrails** - System design must prevent optimization spirals

### 3.5 Potential Future Directions

- AI-assisted note expansion and semantic linking
- Automated tagging based on content analysis
- Knowledge graph visualization and exploration
- Integration with Claude Code for local file management
- Backlog management via markdown files (per "AI-Augmented Fluid Project Management" methodology)

---

# II. Project Roadmap & Backlog

> *"Think of this backlog like a mission control dashboard for your project. We'll track project background information, feature ideas, progress, prioritize tasks, and ensure smooth navigation through your technical journey! 🚀🛠️"*

## Current Phase: Discovery Interview

**Progress:** Question 14 of ~25 (over halfway!)

**Interview Clusters:**
1. ✅ Note-taking behaviors and types (complete)
2. ✅ Project and task management reality (complete)
3. 🔄 Tools and technical constraints (in progress - Question 14)
4. ⏳ Neurodivergent traits intersection
5. ⏳ Integration with existing life patterns

---

## Active Tasks

### Discovery Phase

- [x] Establish core philosophies ("contexts not topics", hybrid BASB+ZK)
- [x] Identify current pain points (context-switching, 90% abandonment, scattered notes)
- [x] Research block-reference tools (Logseq, Obsidian, Roam, etc.)
- [x] Research visual/canvas tools (Heptabase, etc.)
- [x] Evaluate file management solutions → Files app ADOPTED
- [x] Understand ROCETS file organization pattern
- [x] Answer Questions 1-8 of discovery interview
- [x] Answer Question 9 - What makes coding cheat sheets work vs. notes that disappear
- [x] Answer Questions 10-13 of discovery interview
- [ ] **CURRENT: Answer Question 14** ← YOU ARE HERE
- [ ] Complete Questions 15-25 of discovery interview ⚠️ **POST-MIGRATION PRIORITY**
- [ ] Validate Supernote integration assumptions
- [ ] Final tool selection (Logseq vs Obsidian)

### Implementation Phase (Pending Discovery Completion)

- [ ] Select and install primary PKB tool
- [ ] Create initial folder/context structure
- [ ] Design note templates (fleeting, literature, permanent, structure, project)
- [ ] Import 80-page PRO-TIPS document (atomic note conversion)
- [ ] Create file location index system
- [ ] Set up Syncthing synchronization
- [ ] Configure mobile capture workflow
- [ ] Integrate Supernote OCR pipeline
- [ ] Import/organize bookmarked websites

### Anti-Perfectionism Checkpoints

- [ ] Define "minimum viable system" scope
- [ ] Establish time-boxed implementation phases
- [ ] Create "good enough" criteria for each component
- [ ] Build in review points to prevent optimization spirals

---

## Backlog (Future Considerations)

### Near-Term
- [ ] Paperless-ngx full deployment
- [ ] Visual canvas workflow for brainstorming
- [ ] Cheat sheet template system
- [ ] Daily/weekly review routine design

### Medium-Term
- [ ] Spaced repetition integration for learning
- [ ] Project backlog markdown file system
- [ ] Cross-project insight surfacing
- [ ] Archive and retrieval strategy

### Long-Term / Exploratory
- [ ] AI-augmented note expansion
- [ ] Automated semantic linking
- [ ] Team knowledge sharing (NASA colleagues)
- [ ] Integration with Claude Code workflow

---

## Known Issues & Blockers

| Issue | Status | Notes |
|-------|--------|-------|
| Logseq development pace concerns | Monitoring | File-based version still maintained as of Nov 2024 |
| Logseq DB version incompatible with self-hosting | Blocker for DB version | Stay on file-based version |
| "Classify later" mirrors procrastination | Design constraint | Must enforce immediate lightweight org |
| No file location notes exist | Critical gap | Need navigational breadcrumb system |
| 4+ duplicate note sets on same topics | Symptom | System must prevent this pattern |

---

# III. Conversation Context Summary

## 1. Key Discussion Points

### The "Contexts, Not Topics" Breakthrough
Dan's brain naturally creates deep hierarchical folder structures, but this fights how human cognition actually works. The system should use 2-3 broad contextual categories with tags and links doing organizational work.

### Zettelkasten Is For Knowledge, Not Everything
Major breakthrough: Zettelkasten is specifically for developing knowledge and ideas, NOT for managing temporary information like todo lists. This distinction significantly reduced Dan's overwhelm about what content belongs in the system.

### The "Cheat Sheet Pattern"
Dan's coding cheat sheets are his only consistently successful note type. They work because they're:
- Action-oriented (recipes, not reflections)
- In predictable locations
- Solve recurring problems
- Scannable format

This pattern should inform all note template design.

### Block References vs Page References
Dan correctly identified that most block-reference apps provide the same "live copy" functionality—Workflowy's marketing is just marketing. The key differentiator is whether block references are native (Logseq, Roam) or bolted-on (Obsidian with plugins).

### The Dual-Hierarchy Problem
Dan maintains two parallel hierarchies:
- `C:\ROCETS\[Model]` - Executable code with disciplined naming
- `..\Work Documents\[Customer]` - Project context, agreements, research

These are structurally separated due to ROCETS' dependency management. The productivity system must bridge this gap without creating duplication.

## 2. Notable Insights

1. **"Walking the tree" is the problem** - Dan's current hierarchical notes force mental tree-traversal instead of following thought paths

2. **Panic capture is a strength** - The "write it down NOW" urgency shows self-awareness of working memory limits; system should accommodate, not fight this

3. **Blank canvas anxiety** - Visual tools need structured starting points (like Heptabase's Journal-first workflow)

4. **80-page PRO-TIPS doc is gold** - Contains 12 years of engineering rules-of-thumb ready for atomic note conversion

5. **Meta-irony** - Dan has notes about his note-taking problems scattered in UpNote, perfectly demonstrating the problem

6. **5-25% of workday on file management** - Significant enough that friction compounds; Files app adoption was immediate win

## 3. Unresolved Questions

1. **Logseq vs Obsidian final decision** - Waiting for discovery completion and possibly Logseq development trajectory clarity

2. **Supernote integration specifics** - OCR quality, export formats, automation pipeline details

3. **Questions 14-25** - Remaining discovery interview to complete (POST-MIGRATION PRIORITY)

4. **Time allocation for note processing** - How much time can Dan realistically dedicate to end-of-day/week organization?

5. **Mobile capture workflow** - Exact friction requirements for iPhone capture

## 4. Potential Pivot Points

- If Logseq development stalls further → Full pivot to Obsidian
- If Supernote OCR proves unreliable → Reconsider handwriting integration approach
- If "classify later" keeps failing → May need stricter immediate-classification enforcement
- If visual tools cause more anxiety than relief → Focus on outline-based approaches

## 5. What I've Learned About Dan

### Professional Context
- NASA aerospace engineer, 12 years experience
- Works with ROCETS (Fortran-based rocket engine simulator)
- Builds physics-based computational models for LRE performance/operation
- Python fluency ~2.5/5, Fortran limited but eager to learn
- Work breakdown: 15-30% coding, 5-10% meetings, 30-60% admin, 15-35% research
- 80-85% solo work
- Long uninterrupted blocks with minimal meetings

### Neurodivergent Profile
- ADHD: Urgent capture needs, context-switching difficulty, activation energy problems
- OCD: Perfectionism spirals, optimization rabbit holes, 90% abandonment pattern
- Autism spectrum: Systematic thinking, pattern recognition, deep domain expertise

### Working Patterns
- Bad days: Start with email → overwhelm → racing mind through undocumented tasks
- Good days: Substantial coding time on relevant work
- Peak focus: Not explicitly stated (pending Question 17+)
- Creates notes in panicked bursts, then nothing for periods

### Learning Style
- Visual learner (💯^💯)
- Needs examples and metaphors
- Blank canvas anxiety
- Appreciates structured starting points

### Preferences
- Self-hosted solutions
- Local data ownership
- Markdown-based storage
- Wants to understand systems deeply, not just use them
- Willing to tinker but needs perfectionism guardrails

## 6. Additional Goals & Interests Expressed

- Wants to get familiar with Python sets (where appropriate)
- Learning OOP paradigm
- Building GUIs with Python (Eel framework)
- Self-hosted cloud infrastructure (mentioned in other contexts)
- Custom house design project with wife (Queen Anne Victorian)
- Architectural inspiration photos (~1,000) needing organization

---

## Resume Point: Question 14

**The conversation is currently at Question 14 in the "⭐️⭐️⭐️ 🗒️ Note-taking system design" chat.**

Questions 9-13 have been answered. The discovery interview is over halfway complete.

> ⚠️ **POST-MIGRATION PRIORITY:** Complete the remaining discovery questions (14-25) before attempting system architecture. Dan has built momentum—capitalize on it immediately after migration.

**Ready to continue from Question 14 upon migration.**

---

## Cross-Reference: Related Conversations in This Project

| Conversation | Key Content | Status |
|--------------|-------------|--------|
| ⭐️⭐️⭐️ 🗒️ Note-taking system design | Main discovery interview | **ACTIVE - Question 9** |
| ⭐️⭐️⭐🔲 Networked note-taking with Zettelkasten | "Contexts not topics" breakthrough | Complete |
| 🔲 ⭐⭐️⭐️ ZK + HeptaBase + VISUAL references | Visual tool research | Complete |
| ⭐️🔲 Zettelkasten Example Walkthrough | ZK implementation details, Obsidian templates | Complete |
| Workflowy's live copy feature marketing claim | Block reference deep dive, Logseq concerns | Complete |
| Hybrid knowledge management systems | BASB + ZK integration concept | Complete |
| Handwritten notes in productivity system | Supernote integration planning | Paused |
| ⏹️ File Explorer alternative | Files app adoption, ROCETS structure | Complete |
| Note-taking, Obsidian, Zettelkasten, Logseq, block-based | Initial tool comparison | Complete |

---

*End of Project Migration Record*

---

**Document Metadata:**
- Created: January 29, 2026
- Source Project: Productivity System Design (Claude Project)
- Migration Target: Unified Project Space
- Primary Author: Claude (synthesized from conversation history)
- Review Status: Pending Dan's review
