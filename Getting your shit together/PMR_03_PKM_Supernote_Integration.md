# Project Migration Record: Personal Knowledge Management & Supernote Integration

---

## I. Comprehensive Project Summary

### 1. Project Vision

#### 1.1 High-Level Goals
Establish a comprehensive personal knowledge management (PKM) system centered on Logseq, with integration of analog note-taking via Supernote A6 X2 e-ink tablet, automated OCR processing, and self-hosted synchronization across all devices.

#### 1.2 Motivations
- **Analog + Digital**: Combine the benefits of handwriting (better retention, thinking) with digital searchability and organization
- **Tag-Based Organization**: Move away from hierarchical folder structures that force artificial single-location decisions
- **Privacy**: Self-hosted sync server means no third-party access to personal notes
- **Cross-Platform**: Access knowledge from iPhone, MacBook, work computer (web access)
- **Workflow Automation**: Minimize friction between writing notes and having them organized

#### 1.3 Apps, Tools, Services of Interest

**Primary PKM Tool**
- **Logseq** (chosen) - Local-first, markdown-based, bidirectional linking, daily notes
- *Obsidian* (considered) - Similar features, different approach
- *Roam Research* (considered) - Cloud-based, subscription cost

**E-Ink Tablet**
- **Supernote A6 X2** (purchased) - Ceramic nib (never wears out), excellent writing feel, WebDAV sync
- *reMarkable 2* (considered) - Good form factor, more limited ecosystem
- *Boox Note Air* (considered) - Android-based, more versatile but more distracting

**Synchronization**
- **WebDAV** - Supernote syncs via WebDAV to Mac Mini
- **Nextcloud** - WebDAV endpoint, can also sync Logseq vault
- **Logseq Sync Server** (planned) - Self-hosted sync for Logseq across devices

**OCR & Automation**
- **Tesseract OCR** or **macOS Vision framework** - Extract text from handwritten notes
- **Python watchdog** - Monitor sync folder for new files
- Custom automation script (planned)

#### 1.4 Strategic Objectives
1. Enable seamless analog → digital workflow (write on Supernote, appears in Logseq)
2. Tag-based organization that solves "which folder?" problem
3. Self-hosted sync with no cloud dependency for sensitive notes
4. Mobile access from iPhone for review and quick capture
5. Work-accessible via web interface (within NASA restrictions)

#### 1.5 Long-term Potential / Potential Workflows

**Supernote → Logseq Pipeline (Planned):**
```
1. Write note on Supernote with hashtag metadata at top
2. Sync to Mac Mini via WebDAV (into Nextcloud or direct folder)
3. Python script detects new file
4. OCR extracts text from top of page (hashtags, title, date)
5. Script creates markdown file with YAML frontmatter
6. Embeds original handwritten PDF/image
7. Files into Logseq vault based on tags
```

**Example Output:**
```markdown
---
tags: [project/rocket-nozzle, meeting]
date: 2025-10-26
source: handwritten
device: supernote
---

# Nozzle Design Meeting Notes

![[2025-10-26-nozzle-meeting.pdf]]

## OCR Text (optional):
[Full OCR'd text here...]
```

#### 1.6 Unique Value Proposition
- Handwritten notes preserve diagrams, equations, spatial relationships
- But organized and searchable like markdown files
- Best of both worlds: analog capture, digital organization

#### 1.7 Core Philosophies Discussed
- **"The best organizational system is the one you'll actually use"** - Mobile-first, low friction
- **"Tag-based solves the tower AND porch problem"** - Items can have multiple classifications
- **"Handwriting preserved is often more valuable"** - Especially for engineering diagrams
- **"Print hashtags, natural handwriting for body"** - OCR accuracy optimization

---

### 2. Requirements Compilation

#### 2.1 Explicit Requirements
- Supernote WebDAV sync to home server
- OCR capability for handwritten text
- Hashtag-based filing into Logseq vault folders
- Markdown file output with YAML frontmatter
- Original handwritten note embedded (PDF or image)
- Cross-device sync for Logseq vault

#### 2.2 Implied Requirements
- Reliable handwriting recognition (at least for hashtags/metadata)
- Consistent note structure (metadata area at top)
- Fast sync latency (write and forget, not wait)
- Error handling for poor OCR quality
- Periodic backups of Logseq vault

#### 2.3 Derived Requirements
- Establish hashtag conventions early and stick to them
- Test OCR quality with personal handwriting style
- Determine trigger for sync (manual vs automatic)
- Decide on PNG vs PDF exports (trade-offs exist)

#### 2.4 Potential Edge Cases / Expansion Areas
- Bi-directional sync (edit in Logseq, appears on Supernote)?
- Handling multi-page notes
- Notes without hashtags (default handling)
- Diagrams-only pages (no text to OCR)

---

### 3. Architectural Overview

#### 3.1 Current Design Decisions with Rationale

**Logseq over Obsidian:**
- Both are markdown-based, local-first
- Logseq's daily notes and page references align with workflow
- Can self-host sync server
- Decision point: Either works, Logseq selected

**Supernote A6 X2 over Alternatives:**
| Device | Verdict | Rationale |
|--------|---------|-----------|
| Supernote A6 X2 | ✅ Chosen | Ceramic nib (zero replacement cost), superior writing feel, WebDAV sync |
| reMarkable 2 | ❌ | Good but limited ecosystem, requires community tools for automation |
| Boox Note Air | ❌ | Android adds distraction, not as focused on writing |

**Why Ceramic Nib Matters:**
- reMarkable nibs wear out, need replacement
- Supernote ceramic nib is permanent
- Long-term cost savings, less friction

**OCR Strategy:**
- Use Supernote's built-in recognition OR external OCR
- Test both approaches during learning phase
- Print metadata clearly (hashtags), natural handwriting for body
- Constrained vocabulary (looking for #, @, specific patterns) improves accuracy

**Ruled Out Approaches:**
- **Full OCR of entire page** for main content - Preserving handwritten PDF is better for diagrams
- **Complex bi-directional sync** - Not worth the complexity for this use case
- **Desktop-only Tropy** - Workflow requires mobile access

#### 3.2 Technology Stack
- **Hardware**: Supernote A6 X2
- **PKM Tool**: Logseq
- **Sync Protocol**: WebDAV
- **WebDAV Server**: Nextcloud (on Mac Mini) or standalone
- **OCR**: Tesseract or macOS Vision framework
- **Automation**: Python with watchdog library
- **File Format**: Markdown with YAML frontmatter

#### 3.3 Key Architectural Constraints
- Supernote doesn't run apps like Boox - dedicated to writing
- OCR accuracy limited by handwriting legibility
- Sync timing depends on manual trigger vs automatic
- NASA web access means Logseq sync must be web-accessible

#### 3.4 Potential Future Directions
- Voice memo integration
- Meeting note templates with auto-date
- Integration with task management
- Sharing specific notebooks with wife

---

### Supernote Learning Plan (7-Day Structured Approach)

**Phase 1: Foundation (Days 1-2)**
- Basic navigation and gestures
- The "star" gesture (diagonal swipe from top-left)
- Toolbar customization
- **Keywords system** (CRITICAL for workflow) - How tags export

**Phase 2: Sync Infrastructure (Days 2-3)**
- WebDAV configuration with Nextcloud
- Test sync reliability and timing
- Understand sync behavior (manual vs automatic)
- Set up "inbox" folder approach if needed

**Phase 3: Handwriting Quality Testing (Days 3-4)**
- Test Supernote's built-in handwriting recognition
- Test external OCR (Tesseract) with exported files
- Determine optimal hashtag format for OCR accuracy
- Decide: Supernote keywords vs handwritten hashtags

**Phase 4: Export Workflow Design (Days 4-5)**
- File naming conventions
- PNG vs PDF export trade-offs
- Page templates for consistent structure
- Metadata preservation in exports

**Phase 5: Full Pipeline Test (Days 6-7)**
- Build minimal viable automation script
- Test end-to-end: write → sync → OCR → Logseq
- Validate reliability and accuracy
- Identify edge cases and friction points

**Key Questions to Answer:**
1. Keywords system vs handwritten hashtags?
2. Manual sync or automatic?
3. PNG or PDF exports?
4. Bi-directional sync worth the effort?

---

## II. Project Roadmap and Backlog

### ✅ Completed

- [x] Decided on Logseq as PKM tool
- [x] Purchased Supernote A6 X2
- [x] Created 7-day learning plan
- [x] Defined Supernote → Logseq pipeline architecture
- [x] Determined tag-based organization approach
- [x] Established metadata conventions (top of page, print hashtags)

### 🚧 In Progress

- [ ] Supernote learning (following 7-day plan)
- [ ] Testing WebDAV sync configuration
- [ ] Evaluating OCR quality with personal handwriting
- [ ] Experimenting with export formats

### 📋 Next Up

- [ ] Set up WebDAV server (Nextcloud on Mac Mini)
- [ ] Configure Supernote to sync to home server
- [ ] Build Python automation script (watchdog + OCR)
- [ ] Create markdown template for Logseq output
- [ ] Test full pipeline end-to-end

### 🔮 Future / Backlog

- [ ] Set up Logseq sync server on Mac Mini
- [ ] Configure Logseq mobile access
- [ ] Create Logseq page templates for different note types
- [ ] Integrate with project backlogs (this methodology!)
- [ ] Set up backup for Logseq vault to NAS
- [ ] Create note templates for common scenarios (meetings, design ideas, etc.)

---

## III. Conversation Context Summary

### 3.1 Key Discussion Points
- E-ink tablet comparison (Supernote vs reMarkable vs Boox)
- OCR accuracy and personalization (most systems don't learn your handwriting)
- Tag-based organization vs hierarchical folders
- Mobile-first workflow importance
- PKM tool selection (Logseq vs Obsidian vs Roam)

### 3.2 Notable Insights
- **"Supernote even name drops Zettelkasten"** - Marketing to PKM crowd
- **"95% accuracy on hashtags is achievable"** with clear printing and patterns
- **"You'll get better at writing for OCR"** - Humans adapt faster than OCR learns
- **"Print metadata, natural handwriting for body"** - Optimization strategy
- **"Handwriting preserved is valuable"** - Especially for diagrams and equations

### 3.3 Unresolved Questions
- Will Supernote's Keywords system be better than handwritten hashtags?
- What's the optimal page template layout?
- Should notes be filed immediately or stay in inbox for review?
- How to handle notes without any tags?

### 3.4 Potential Pivot Points
- If Supernote Keywords work well, might not need OCR for tags
- If sync latency is problematic, might need different trigger approach
- Could use Supernote's built-in OCR if quality is sufficient

### 3.5 Metadata Convention (Proposed)

**Standard Note Header:**
```
[Title line at top]
#tags #go #here @YYYY-MM-DD
---
[Content area - natural handwriting]
```

**Tag Format Examples:**
- `#project/homelab`
- `#type/meeting`
- `#priority/high`
- `@2025-01-15` (date)

---

## Cross-References

- **Home Lab Infrastructure**: See PMR #1 (WebDAV server, sync infrastructure)
- **DIY NAS**: See PMR #2 (Logseq vault backup)
- **Home Design Visual Reference**: Architecture inspiration could link to PKM
