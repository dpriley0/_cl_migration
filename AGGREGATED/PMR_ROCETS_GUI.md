# Project Migration Record: ROCETS GUI Application

**Migration Date:** January 28, 2026  
**Source:** Claude Project - ROCETS GUI Development  
**Status:** Active Development (Pre-MVP)

---

# Part I: Comprehensive Project Summary

## TL;DR

Building a Python/Eel desktop GUI application for ROCETS (ROCket Engine Transient Simulator), enabling aerospace engineers to configure, execute, and analyze rocket engine simulations through an intuitive interface. The app parses Fortran-generated output files, manages model configurations, and provides snapshot/archival capabilities for debugging and cross-run analysis.

---

## 1. Project Vision

### 1.1 High-Level Goals

- **Primary Goal:** Create a desktop GUI application that wraps the ROCETS Fortran-based simulation software, providing modern user interface capabilities for rocket engine modeling
- **Secondary Goals:**
  - Replace manual file handling with automated parsing and data management
  - Enable rapid iteration on engine models with instant feedback
  - Provide robust debugging tools via snapshot/archival system
  - Support cross-run analysis and comparison workflows
  - Build a foundation for future NASA tools using similar architecture

### 1.2 Motivations

- **Pain Point:** ROCETS is a powerful but CLI-only Fortran tool with no visual interface
- **Workflow Friction:** Manual parsing of output files, copying results to spreadsheets
- **Debugging Challenges:** Difficult to revert to known-good model states
- **Learning Opportunity:** Dan (the developer) wants to build Python/GUI skills while solving a real work problem
- **No Existing Alternative:** No commercial or open-source GUI exists for ROCETS

### 1.3 Apps, Tools, Utilities & Services of Interest

#### Core Development Stack
- **Eel** - Python↔JavaScript bridge for desktop GUI (HTML/CSS/JS frontend, Python backend)
- **Python 3.14** - Primary development language (standardized via Homebrew)
- **pandas** - DataFrame manipulation for parsed simulation data
- **Rich** - Terminal output formatting for development/debugging
- **file-read-backwards** - Efficient reverse file parsing
- **pyperclip** - System clipboard integration
- **pathlib** - Cross-platform file path handling

#### Data Storage & Analysis (Future)
- **Parquet** - Columnar storage for parsed simulation data within snapshots
- **zipfile** - Compression for snapshot archives
- **difflib** - Diff generation for snapshot comparison

#### Development Environment
- **Spyder IDE** - Primary IDE (with persistent IPython console + Variable Explorer)
- **Wing IDE** - Under consideration as potential upgrade path
- **VS Code** - Secondary IDE for HTML/CSS/JavaScript work
- **Phoenix Code (Brackets)** - Alternative web dev editor

#### Version Control
- **Jujutsu (jj)** - Primary VCS, preferred over Git for its cleaner mental model
- **Git** - Backend/compatibility layer (colocated with jj)
- **Mercurial** - For personal projects on home server
- **GitHub** - Remote repository hosting

#### Terminal & Shell
- **Nushell** - Primary shell across all platforms
- **Oh-My-Posh** - Shell prompt styling
- **Hyper/iTerm/Terminal** - Various terminal emulators configured with Nushell

#### Cross-Platform
- **Windows 11 PC** - Primary development machine (work)
- **MacBook** - Secondary development machine
- **Mac Mini (AstralJunction)** - Home server (mentioned in past conversations outside this Project)
- **iPhone** - Mobile development via PyTo/Pythonista

### 1.4 Strategic Objectives

1. **MVP First:** Get a working GUI that can execute ROCETS runs and display parsed results
2. **Iterative Enhancement:** Add features incrementally based on actual use
3. **Robust Foundation:** Build proper OOP architecture that scales
4. **Documentation:** Maintain comprehensive backlog and design rationale
5. **Skill Building:** Use project as vehicle for learning Python, OOP, and GUI development

### 1.5 Long-Term Potential / Workflows

#### Post-MVP Features (Roadmap)
- **Snapshot Browser GUI** - Visual interface for browsing/comparing model snapshots
- **Automated Snapshots** - Trigger snapshot after each successful ROCETS run
- **Cross-Run Analysis** - Query across multiple simulation runs using Parquet
- **Diff Viewer** - Side-by-side comparison of configuration files between snapshots
- **Textual TUI** - Terminal-based UI for automation workflows (parallel to GUI)
- **Template Systems** - Standardized analysis patterns for common scenarios

#### Future NASA Tool Potential
- This architecture could serve as template for other NASA engineering tools
- Patterns learned here applicable to other Fortran-based simulation wrappers

### 1.6 Unique Value Proposition

- **Only GUI solution for ROCETS** - No competition, fills genuine gap
- **Engineer-built for engineers** - Understands actual workflow needs
- **NASA context** - Designed with IT restrictions in mind (works on restricted networks)
- **Cross-platform** - Works on Windows (primary), Mac, and potentially mobile

### 1.7 Core Philosophies Discussed

1. **Systematic Learning > Trial-and-Error:** Prefer comprehensive understanding before implementation
2. **Evidence-Based Design:** UX decisions backed by research, not opinion
3. **Simplicity First:** Use Python's built-in capabilities where possible (e.g., `sys.platform` over `platform.system()`)
4. **Proper OOP:** Invest time in class design with properties, methods, and separation of concerns
5. **Safety Through VCS:** Save points via version control before risky changes
6. **Dry-Run by Default:** Non-destructive modes for testing
7. **One Concept at a Time:** Master fundamentals before adding complexity (e.g., debug flags before logging module)

---

## 2. Requirements Compilation

### 2.1 Explicit Requirements

#### Functional Requirements
- [ ] Execute ROCETS simulations from GUI
- [ ] Parse ROCETS output files (.out, .sol, .for) into structured data
- [ ] Display parsed data in readable format
- [ ] Manage configuration files (.cfg, .run)
- [ ] Save/load project configurations between sessions
- [ ] Create compressed snapshots of model state (4 key files)
- [ ] Copy parsed data to system clipboard
- [ ] Cross-platform operation (Windows primary, Mac secondary)

#### Technical Requirements
- [ ] Python 3.x backend with Eel framework
- [ ] HTML/CSS/JavaScript frontend
- [ ] Singleton configuration management pattern
- [ ] Class-based file parsers with state
- [ ] Robust error handling for NASA-level reliability
- [ ] Session persistence via JSON serialization

### 2.2 Implied Requirements

- GUI must be responsive during long-running simulations
- File paths must handle Windows/Mac differences automatically
- Parser must handle varying column counts (typically 4, sometimes hundreds)
- Configuration changes must trigger automatic recalculation of derived paths
- User preferences separate from model configuration
- Must work within NASA IT restrictions (no cloud dependencies)

### 2.3 Derived Requirements

- Need validation at config creation time (catch bad paths immediately)
- Need computed properties for derived file paths
- Need progress indicators for long operations
- Need graceful degradation when components fail
- Need audit trail for debugging (error context showing what led to failure)

### 2.4 Potential Edge Cases / Expansion Areas

- Very large output files (3,000+ lines)
- Files with hundreds of columns vs. typical 4
- Unicode/encoding issues in Fortran output
- Multiple simultaneous ROCETS runs
- Model files on network drives
- Recovery from mid-parse failures

---

## 3. Architectural Overview

### 3.1 Current Design Decisions with Rationale

#### Framework: Eel (Python + HTML/CSS/JS)
- **Chosen because:** Allows Python backend with web frontend, familiar web technologies
- **Alternatives considered:**
  - **PyQt/PySide** - Rejected: Steeper learning curve, less familiar
  - **Tkinter** - Rejected: Dated appearance, limited customization
  - **Textual** - Considered for TUI, but GUI needed first

#### Configuration Pattern: Singleton via `__new__`
- **Chosen because:** Ensures single config instance across all modules
- **Implementation:** `__new__` method checks if instance exists, returns existing or creates new
- **Alternative considered:** Module-level singleton (simpler, but explicit pattern better for learning)

#### File Parsing: Class-based parsers with state
- **Chosen because:** Each parsed file is a "smart container" with data + metadata + methods
- **Mental model:** "Labeled binder" - filename on spine, data inside, built-in tools
- **Key insight:** Use `self.xxx` for data that "needs to leave the room" (persist across methods)

#### Reverse File Parsing: file-read-backwards library
- **Chosen because:** ROCETS output has headers at top, data at bottom; need to parse from end
- **Performance consideration:** For files up to 3,000 lines, `readlines()` + reverse equally fast
- **When reverse parsing wins:** When only needing last 40-50% of file

#### Snapshot Storage: ZIP with Parquet for data files
- **Chosen because:**
  - Config files (.cfg, .run) stored as-is for exact reproduction
  - Data files (.out, .sol, .for) stored as Parquet for instant analysis without re-parsing
- **Not chosen:** 
  - `.tar.gz` - Less Windows-friendly
  - Raw Parquet alone - Need original configs for debugging

#### Debug/Release Modes: Enum-based RunMode
- **Chosen:** `RunMode.DEBUG` and `RunMode.RELEASE` enum at module level
- **Not chosen:** 
  - Boolean `DEBUG_MODE` flag - Less extensible
  - Python logging module - Saved for later (one concept at a time)

### 3.2 Technology Stack

```
┌─────────────────────────────────────────┐
│              FRONTEND                    │
│  HTML/CSS/JavaScript (via Eel)          │
│  - User interface                        │
│  - Input forms                           │
│  - Data visualization                    │
└─────────────────────────────────────────┘
                   ↕ @eel.expose
┌─────────────────────────────────────────┐
│              BACKEND                     │
│  Python 3.14                            │
│  ├── Config (singleton)                 │
│  ├── Parsers (class-based)              │
│  ├── SnapshotManager                    │
│  └── ROCETS execution wrapper           │
└─────────────────────────────────────────┘
                   ↕
┌─────────────────────────────────────────┐
│           EXTERNAL TOOLS                 │
│  ROCETS Executable (Fortran)            │
│  Model files (.cfg, .run, etc.)         │
└─────────────────────────────────────────┘
```

### 3.3 Frameworks Decided To Use

| Framework/Library | Purpose | Status |
|-------------------|---------|--------|
| Eel | Python↔JS bridge | Core |
| pandas | Data manipulation | Core |
| pathlib | Path handling | Core |
| zipfile | Snapshot compression | Core |
| file-read-backwards | Reverse parsing | Core |
| pyperclip | Clipboard access | Core |
| Rich | Terminal formatting | Development |
| Parquet (via pyarrow) | Data storage in snapshots | Future |
| difflib | Snapshot comparison | Future |

### 3.4 Frameworks Explicitly Ruled Out

| Framework | Reason Ruled Out |
|-----------|------------------|
| Parquet for snapshot archiving | Wrong tool - Parquet is for analytical queries, not file bundling |
| PyQt/PySide | Learning curve too steep for current Python level |
| logging module | Deferred - master debug flags first |
| unittest/pytest | Not yet - focus on core functionality first |

### 3.5 Key Architectural Constraints

- Must run on NASA IT-restricted machines (no cloud dependencies)
- Must handle Fortran-generated files with potential encoding issues
- Must maintain data integrity for engineering applications
- Must support offline operation completely
- Python environment must be unified across terminal and IDE

### 3.6 Potential Future Directions

1. **Textual TUI parallel path** - Python-based terminal UI for automation scripts
2. **Multi-model comparison tool** - Side-by-side analysis across runs
3. **Template system** - Pre-built analysis patterns for common scenarios
4. **Report generation** - Automated documentation of simulation results
5. **Version control integration** - Direct jj/git operations from GUI

---

# Part II: Project Roadmap & Backlog

## Project Status: Pre-MVP Active Development

Think of this backlog like a mission control dashboard for your project. We're tracking project background, feature ideas, progress, prioritizing tasks, and ensuring smooth navigation through your technical journey! 🚀🛠️

---

## Current Sprint Focus

### Configuration System (In Progress)
- [x] Basic Config class structure
- [x] Singleton pattern via `__new__`
- [x] Pathlib-based file path handling
- [x] Computed properties for derived paths
- [x] JSON persistence (save/load)
- [ ] Validation on config changes
- [ ] Proper `__repr__` implementation
- [ ] Separate user preferences config

### File Parsing (In Progress)
- [x] Reverse file parsing approach selected (file-read-backwards)
- [x] Class-based parser design established
- [x] Mental model: "smart container" for each parsed file
- [ ] Complete RocetsOutputParser class
- [ ] Clipboard integration (pyperclip)
- [ ] Column extraction by name
- [ ] Error handling for malformed files

---

## Feature Backlog

### MVP Features (Must Have)

| Feature | Priority | Status | Notes |
|---------|----------|--------|-------|
| Config singleton | P0 | 🔄 In Progress | Core dependency for everything |
| Output file parser | P0 | 🔄 In Progress | .out files to DataFrame |
| Basic GUI shell | P0 | ⏳ Not Started | Eel app structure |
| Run ROCETS from GUI | P0 | ⏳ Not Started | Execute simulation |
| Display parsed results | P0 | ⏳ Not Started | Show data in interface |

### Post-MVP Features (Should Have)

| Feature | Priority | Status | Notes |
|---------|----------|--------|-------|
| Snapshot creation | P1 | 📋 Designed | ZIP with 4 files |
| Parquet storage in snapshots | P1 | 📋 Designed | Pre-parsed data |
| Solution file parser | P1 | ⏳ Not Started | .sol files |
| Configuration file parser | P1 | ⏳ Not Started | .cfg files |
| Debug/Release modes | P2 | 📋 Designed | RunMode enum |
| User preferences config | P2 | ⏳ Not Started | Separate from model config |

### Future Features (Nice to Have)

| Feature | Priority | Status | Notes |
|---------|----------|--------|-------|
| Snapshot browser GUI | P3 | 💡 Conceptual | Visual snapshot management |
| Automated snapshots after run | P3 | 💡 Conceptual | Auto-trigger on completion |
| Diff viewer for snapshots | P3 | 💡 Conceptual | Side-by-side comparison |
| Cross-run analysis | P3 | 💡 Conceptual | Query across runs |
| Textual TUI | P4 | 💡 Conceptual | Terminal automation tool |
| Template systems | P4 | 💡 Conceptual | Standardized patterns |

---

## Technical Debt / Cleanup Items

- [ ] Remove debug variables before MVP release (use RunMode pattern)
- [ ] Consolidate any duplicate code paths
- [ ] Document all public methods
- [ ] Add type hints to function signatures
- [ ] Review naming conventions (enforce snake_case)

---

## Learning Objectives (Integrated)

| Concept | Status | Relevance |
|---------|--------|-----------|
| OOP class design | 🔄 In Progress | Core to all parsers |
| Properties vs methods | ✅ Understood | Config class design |
| `self.xxx` usage patterns | ✅ Understood | "Does it need to leave the room?" |
| Singleton pattern | ✅ Implemented | Config class |
| Sets in Python | 🔄 Practicing | Deduplication in snapshots |
| Regex for parsing | ✅ Practiced | File parsing |
| Hash functions | ✅ Understood | Why Path objects work in sets |

---

## Development Environment Status

| Component | Platform | Status |
|-----------|----------|--------|
| Python 3.14 (Homebrew) | Mac | ✅ Configured |
| Spyder IDE | Mac | ✅ Unified with terminal |
| Nushell + Oh-My-Posh | All | ✅ Configured |
| Jujutsu (jj) | All | ✅ Working |
| Rich terminal output | All | ✅ Configured (color issues on dark theme) |

---

## Known Issues / Blockers

1. **Rich color theming in Spyder dark mode** - Green text (#006400) hard to read, no resolution found
2. **Platform detection inconsistency** - PyTo returns 'iOS', Pythonista returns 'Darwin'; use `sys.platform` instead of `platform.system()`

---

# Part III: Conversation Context Summary

## 1. Key Discussion Points

### Configuration Management
- Extensive discussion on Python global variables, namespaces, and module imports
- Singleton pattern chosen via `__new__` method
- Properties with setters for controlled attribute access
- Computed/derived paths auto-calculated from base paths

### File Parsing Architecture
- Reverse parsing approach for ROCETS output files
- Class-based parsers as "smart containers"
- `self.xxx` mental model: "Does this need to leave the room?"
- file-read-backwards library for efficient backward reading

### GUI/UX Design
- Extensive UX principles document created (in Project files)
- Menu design philosophy: traditional top menus best for desktop power tools
- Engineering-specific considerations: data integrity, validation, audit trails
- Accessibility and keyboard navigation requirements

### Development Environment
- IDE comparison: Spyder vs PyCharm vs Wing IDE
- Spyder's persistent IPython console + Variable Explorer considered essential
- Wing IDE identified as potential "Goldilocks" option
- Python environment unification across terminal and IDE

### Version Control
- Jujutsu (jj) mental model and commands
- Bookmarks vs branches, rebasing, conflict handling
- Mercurial for personal projects
- Git colocated for compatibility

### Snapshot System Design
- ZIP compression for file bundles
- Parquet for pre-parsed data within snapshots
- Sets for automatic file deduplication
- Diff viewer for snapshot comparison (future)

## 2. Notable Insights

1. **Python has no true universal globals** - Module imports are the only way to share state
2. **`sys.platform` > `platform.system()`** - More reliable, consistent across iOS apps
3. **Parquet is for analytical queries, not archiving** - Use ZIP for file bundling
4. **SCREAMING_SNAKE_CASE for true constants only** - Not for mutable instance attributes
5. **Properties provide the "what" view, methods the "how" action** - Design distinction
6. **Instance methods add "friction" for important operations** - Require explicit calls
7. **Hash functions enable O(1) lookups** - Why Path objects work in sets/dicts

## 3. Unresolved Questions

1. What should the initial GUI layout look like? (No mockups created yet)
2. How to handle ROCETS executable failures gracefully?
3. Should there be a "recent projects" feature?
4. How to integrate with existing Excel-based workflows?
5. Textual TUI priority - when to start this parallel path?
6. Wing IDE trial - worth the investment?

## 4. Potential Pivot Points

- **If Eel becomes limiting:** Consider migrating to PyQt/PySide (higher capability, steeper curve)
- **If mobile becomes important:** React Native or similar could replace Eel frontend
- **If Textual TUI proves valuable:** Could become primary interface for power users
- **If NASA adopts standardized tooling:** May need to integrate with agency-wide systems

## 5. What I've Learned About Dan

### Work Context
- NASA aerospace engineer (13+ years)
- Builds physics-based computational models for Liquid Rocket Engines
- Uses ROCETS (Fortran-based) for steady-state and transient simulations
- Limited Fortran fluency despite long tenure with the tool

### Technical Profile
- Python fluency: Self-assessed 2.0-2.5/5 (improving)
- New to OOP paradigm, prefers concrete examples and metaphors
- Uses Spyder IDE with persistent IPython console (essential workflow)
- Prefers clean, organized interfaces (dislikes overwhelming features)
- Values systematic learning over trial-and-error

### Learning Preferences
- Appreciates TL;DR summaries at start of complex responses
- Section-by-section TL;DRs for multi-part responses
- Wants to understand "why" behind decisions, not just "how"
- Prefers when instructions are reprinted after debugging assistance
- Likes analogies (rocket engines, filing cabinets, binders)

### Tool Preferences
- Uses Jujutsu (jj) over Git when possible
- Nushell as primary shell across all platforms
- Wants to use sets more in Python (explicitly requested)
- Capitalizes "Project" when referring to Claude Project spaces

### Work Style
- Methodical, asks clarifying questions
- Documents rationale alongside implementation
- Safety-first (VCS save points, dry-run modes)
- Prefers unified environments (same packages everywhere)

## 6. Additional Goals/Interests Mentioned

- **Textual-based terminal UIs** - Excited about Python automation via Rich/Textual ecosystem
- **Logseq/Zettelkasten** - Knowledge management using bidirectional linking
- **Home Server (AstralJunction)** - Mac Mini-based self-hosted infrastructure (separate Project)
- **Mercurial hosting** - Personal projects on home server
- **Deepen Fortran fluency** - Long-term learning goal
- **Master OOP principles** - Active learning focus
- **Sets in Python** - Explicit request to use more

---

# Appendix: Additional Context

## A. File Types in ROCETS Workflow

| Extension | Type | Contents | Parse Strategy |
|-----------|------|----------|----------------|
| .cfg | Config | Model configuration | Store as-is for reproduction |
| .run | Config | Run parameters | Store as-is for reproduction |
| .out | Data | Simulation output | Parse to DataFrame → Parquet |
| .sol | Data | Solution data | Parse to DataFrame → Parquet |
| .for | Data | Fortran output | Parse to DataFrame → Parquet |

## B. Key Code Patterns Established

### Singleton Config
```python
class Config:
    _instance = None
    
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Class-based Parser
```python
class RocetsOutputParser:
    def __init__(self, filename):
        self.filename = filename
        self.data = []
        self.headers = []
        self.parse()  # Auto-parse on creation
    
    def parse(self):
        with FileReadBackwards(self.filename) as f:
            for line in f:
                # parsing logic
                pass
```

### Debug Mode Pattern
```python
from enum import Enum

class RunMode(Enum):
    DEBUG = "debug"
    RELEASE = "release"

MODE = RunMode.DEBUG  # Module-level constant
```

## C. UX Principles Reference

The uploaded UX_Principles.pdf contains comprehensive guidance on:
- Desktop application specific principles
- Data-heavy application design
- Engineering workflow optimization
- Error prevention and recovery
- Performance as UX
- Accessibility considerations
- Modern desktop patterns

(Recommend keeping this as a project reference file)

---

## Document Metadata

- **Created:** January 28, 2026
- **Created By:** Claude (via AI-augmented Fluid Project Management methodology)
- **Purpose:** Project Migration Record for consolidation into unified Claude Project space
- **Sections Ready for Deconstruction:**
  - Part I → Comprehensive Project Summary
  - Part II → Project Roadmap & Backlog
  - Part III → Conversation Context Summary
  - Appendix → Technical Reference

---

*End of Project Migration Record*
