# Project Migration Record: Home Design Visual Reference Management

---

## I. Comprehensive Project Summary

### 1. Project Vision

#### 1.1 High-Level Goals
Create an organized, searchable photo gallery system to manage nearly 1,000 architectural inspiration photos for a custom Queen Anne Victorian house design project, enabling tag-based filtering and mobile-first access.

#### 1.2 Motivations
- **Organization Crisis**: ~1,000 inspiration photos currently scattered across iPhone Camera Roll, MacBook, and Pinterest with no unified organization
- **"Tower AND Porch" Problem**: Hierarchical folders fail when a photo belongs to multiple categories
- **Collaborative**: Both Dan and wife collecting and using these references
- **Mobile Collection**: Most photos found/saved while browsing on iPhone
- **Actionable Reference**: Need to actually find and use photos during design process

#### 1.3 Apps, Tools, Services of Interest

**Self-Hosted Photo Management**
- **Immich** (chosen) - Self-hosted Google Photos alternative, native iOS app, AI-powered search, excellent mobile experience
- *PhotoPrism* (considered) - Web-based, good tagging, can index existing folders
- *Piwigo* (considered) - Functional but dated interface
- *Nextcloud Photos* (considered) - Good if already running Nextcloud

**Desktop Photo Management (Ruled Out for Primary Use)**
- *Tropy* (considered) - Excellent for research, custom metadata, but desktop-only, no mobile
- *DigiKam* (considered) - Powerful but complex, desktop-focused

**Pinterest** (current partial solution)
- Problem: Hierarchical boards don't allow multi-tagging
- Migration needed to self-hosted solution

#### 1.4 Strategic Objectives
1. Consolidate all inspiration photos into single searchable system
2. Enable multi-tag organization (tower + porch + exterior all on same photo)
3. Mobile-first: Upload and tag directly from iPhone while browsing
4. Collaborative: Wife can access and contribute
5. AI-powered search for natural language queries

#### 1.5 Long-term Potential / Potential Workflows

**Collection Workflow (Mobile-First):**
```
1. Find inspiration photo on Instagram/Pinterest/web
2. Save to iPhone Camera Roll
3. Open Immich app
4. Upload photo
5. Add tags: "tower", "Victorian", "blue", "porch"
6. Add description: "Love this wraparound porch. Source: [URL]"
7. Done - tagged and searchable in 30 seconds
```

**Search & Retrieval:**
- `tag:tower AND tag:porch` - Find all photos with both features
- `tag:blue AND tag:exterior` - Blue exterior examples
- Natural language AI search: "Victorian houses with turrets"

**Collaborative Design Review:**
- Create shared albums: "Final Porch Designs", "Approved Colors"
- Wife can browse and add favorites from her phone
- Comment on specific photos with notes

#### 1.6 Unique Value Proposition
- Tag-based organization solves the hierarchical folder limitation
- Mobile-first means photos actually get tagged (not "I'll do it later")
- Self-hosted preserves privacy and control
- AI search enables finding photos you forgot existed

#### 1.7 Core Philosophies Discussed
- **"The best organizational system is the one you'll actually use"** - Mobile access is key
- **"Tropy workflow: hours to tag. Immich workflow: 30 seconds"** - Friction kills adoption
- **"These aren't precious originals"** - Collected from Pinterest/Instagram, copies already
- **"Import and manage is fine"** - For inspiration photos, don't need indexing-only approach

---

### 2. Requirements Compilation

#### 2.1 Explicit Requirements
- Tagging (ABSOLUTE MUST-HAVE #1)
- Mobile upload and tagging (iPhone native app)
- Multi-tag per photo (solves "tower AND porch" problem)
- Search by tag combinations
- Description/notes field
- Source URL field (nice to have)
- Wife can access and contribute

#### 2.2 Implied Requirements
- Fast, responsive mobile interface
- Cross-platform access (iPhone, MacBook, work computer via web)
- Good gallery/grid browsing experience
- Drag-and-drop reorganization (within albums)

#### 2.3 Derived Requirements
- Self-hosted on Mac Mini via Cloudflare Tunnel
- Biometric authentication via Cloudflare Access
- Backup of photos to NAS (when built)
- Import from Pinterest (export then bulk upload)

#### 2.4 Potential Edge Cases / Expansion Areas
- Very large albums (performance with 1000+ photos?)
- Duplicate detection
- Offline access to favorites
- PDF/document inspiration alongside photos

---

### 3. Architectural Overview

#### 3.1 Current Design Decisions with Rationale

**Immich over Alternatives:**
| Solution | Verdict | Rationale |
|----------|---------|-----------|
| Immich | ✅ Chosen | Native iOS app, 30-second tagging, AI search, active development |
| PhotoPrism | Runner-up | Excellent but web-only mobile, slight friction increase |
| Tropy | ❌ | Desktop-only, hours to tag instead of seconds |
| Pinterest | ❌ | Hierarchical boards, external dependency |

**Why Mobile-First Won:**
```
Scenario: Find photo while browsing Instagram at 10 PM on couch

WITH TROPY:
1. Save photo → Camera Roll
2. Later: Transfer to MacBook
3. Move to Mac Mini server
4. Open Tropy
5. Import photos
6. Add tags and notes
7. Close Tropy
TIME TO TAG: Hours to days later, at computer

WITH IMMICH:
1. Save photo → Camera Roll
2. Open Immich app
3. Upload photo
4. Add tags and description
5. Done
TIME TO TAG: 30 seconds, right now
```

**Import vs Index Decision:**
- **Import (Immich manages files)** - Chosen
- *Index (files stay in original location)* - Not needed for this use case
- Rationale: These are collected inspiration photos, not precious originals

**Ruled Out Approaches:**
- **Desktop-first workflow** - "I'll tag it later" = never tagged
- **Hierarchical folders** - Tower OR porch, not both
- **Cloud-only solutions** - Privacy concerns, control issues
- **Complex custom metadata** - Tags + description sufficient for inspiration photos

#### 3.2 Technology Stack
- **Server**: Mac Mini (AstralJunction)
- **Application**: Immich (Docker container)
- **Database**: PostgreSQL (separate container per Immich requirements)
- **Storage**: Initially Mac Mini external, later NAS
- **Access**: Cloudflare Tunnel + Access
- **Mobile**: Immich iOS app

#### 3.3 Key Architectural Constraints
- Immich wants to manage/import photos, not just index
- PostgreSQL database required for Immich
- iPhone Camera Roll is primary collection source
- Wife uses Android (Immich has Android app too)

#### 3.4 Potential Future Directions
- AI auto-tagging to speed up organization
- Face detection for photos with people
- Integration with home design software
- Link photos to specific rooms in house plans
- Export curated albums for architect meetings

---

### Current Photo Inventory

**Estimated Photos:**
- ~1,000 total architectural inspiration photos

**Current Locations:**
- iPhone Camera Roll (majority)
- MacBook (some)
- Pinterest boards (hundreds, well-organized but hierarchical)

**Photo Categories (Tag Ideas):**
- **Architectural Features**: tower, porch, dormer, gable, turret, bay window
- **Styles**: Victorian, Queen Anne, Italianate, Craftsman
- **Colors**: blue, green, painted lady, natural wood
- **Areas**: exterior, interior, kitchen, bathroom, entryway
- **Details**: trim, molding, spindles, brackets, shingles
- **Favorites**: reference, approved, maybe

---

## II. Project Roadmap and Backlog

### ✅ Completed

- [x] Identified problem: ~1,000 unorganized inspiration photos
- [x] Evaluated solutions (Tropy, PhotoPrism, Immich, Piwigo)
- [x] Chose Immich for mobile-first workflow
- [x] Defined tag-based organization strategy
- [x] Confirmed self-hosted on Mac Mini architecture

### 📋 Next Up (After Cloudflare Tunnel Fixed)

- [ ] Deploy Immich Docker container on Mac Mini
- [ ] Configure PostgreSQL database for Immich
- [ ] Set up Cloudflare Tunnel route for photos.toomanytabs.space
- [ ] Configure Cloudflare Access policy (Dan + wife)
- [ ] Install Immich iOS app
- [ ] Test upload and tagging workflow

### 🔮 Future / Backlog

- [ ] Bulk import photos from Camera Roll
- [ ] Export and import Pinterest boards
- [ ] Establish tag taxonomy/vocabulary
- [ ] Create shared albums for design collaboration
- [ ] Migrate storage to NAS when built
- [ ] Configure automatic iPhone photo backup
- [ ] Set up wife's Android app access
- [ ] Explore AI auto-tagging features

### 🎯 Tag Taxonomy (Draft)

**Architectural Features:**
`tower`, `turret`, `porch`, `wraparound-porch`, `dormer`, `gable`, `bay-window`, `balcony`, `cupola`

**Styles:**
`victorian`, `queen-anne`, `italianate`, `gothic-revival`, `craftsman`, `colonial`

**Exterior Elements:**
`trim`, `brackets`, `spindles`, `shingles`, `fish-scale`, `clapboard`, `paint-colors`

**Interior Areas:**
`kitchen`, `bathroom`, `bedroom`, `living-room`, `entryway`, `staircase`

**Evaluation:**
`favorite`, `approved`, `reference`, `maybe`, `rejected`

**Collaboration:**
`dan-pick`, `laura-pick`, `architect-share`

---

## III. Conversation Context Summary

### 3.1 Key Discussion Points
- Deep comparison of photo management solutions
- Mobile-first vs desktop-first workflow impact on actual usage
- Tag-based vs hierarchical organization
- Import vs index file management approaches
- Pinterest migration strategy

### 3.2 Notable Insights
- **"Ask yourself: will I actually follow through with the workflow?"** - Friction kills systems
- **"30 seconds vs hours"** - The Immich vs Tropy workflow comparison
- **"These aren't precious originals"** - Import/manage is fine for inspiration photos
- **"The tower AND porch problem"** - Perfect articulation of hierarchical limitation

### 3.3 Unresolved Questions
- Best practices for bulk importing from Pinterest?
- How to handle photos with no relevant tags?
- Duplicate detection workflow?
- Storage migration path when NAS is ready?

### 3.4 Potential Pivot Points
- If Immich AI search is excellent, might need fewer manual tags
- Could add PhotoPrism later as second viewer if needed
- Might want Tropy for a subset of "serious research" photos

### 3.5 User Story (Original Problem Statement)

> "My wife and I are designing a completely custom house in the Queen Anne Victorian style. So over the last several years, we've both been collecting images of Victorian houses and specific design features that we like to use as inspiration for our design. Now, I have a gallery of nearly 1,000 of these images just in my own collection. There are so many that it's actually NOT that helpful because it lacks organization.

> A lot of the images are in my iPhone's camera roll, some are on my MacBook, and hundreds are on Pinterest. But let's just focus on the Pinterest images because that's where all of the other ones were headed before I gave up. I did a very good job organizing them on Pinterest, but Pinterest suffers from the exact same problems and downsides as any other hierarchical file system—'Urghh! This image is an example of both a tower that we like AND a wraparound porch that we like—do I put it on the 'Towers' board or the 'Porches' board?!!? AHHHHH 😫'"

---

## Cross-References

- **Home Lab Infrastructure**: See PMR #1 (Immich deployment, Cloudflare Access)
- **DIY NAS**: See PMR #2 (photo storage migration)
- **PKM & Supernote**: Design notes could link to visual references
