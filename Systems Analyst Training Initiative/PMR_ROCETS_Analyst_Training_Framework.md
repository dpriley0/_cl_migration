# Project Migration Record (PMR)

## ROCETS Analyst Training Framework

### NASA Marshall Space Flight Center

**Migration Date:** January 29, 2026  
**Originating Claude Project:** ROCETS Training Framework Development  
**Project Owner:** Dan (Aerospace Engineer, NASA MSFC)

---

# I. Comprehensive Project Summary

## 1. Project Vision

### 1.1 High-Level Goals

The ROCETS Analyst Training Framework is an initiative to create an AI-augmented, competency-based training program for the next generation of ROCETS (ROCket Engine Transient Simulation) systems analysts at NASA Marshall Space Flight Center. The core goals include:

- **Accelerate analyst onboarding** - Reduce time-to-productivity for new hires learning the complex ROCETS simulation system
- **Preserve institutional knowledge** - Capture expertise from senior analysts before it's lost to retirement/attrition
- **Modernize training delivery** - Move from ad-hoc, tribal knowledge transfer to structured, self-paced learning
- **Maintain security compliance** - Ensure all training materials and platforms meet ITAR/EAR requirements
- **Create a one-stop training hub** - Unified platform that integrates with existing NASA MSFC enterprise infrastructure

### 1.2 Motivations

- **Knowledge Gap Crisis**: Senior ROCETS analysts are approaching retirement, and their deep expertise in liquid rocket engine modeling isn't adequately documented
- **Onboarding Inefficiency**: Current training is informal, inconsistent, and heavily dependent on individual mentors' availability
- **Tool Complexity**: ROCETS is a Fortran-based simulation system developed in the late 1980s with extensive capabilities but steep learning curves
- **Security Constraints**: NASA MSFC operates under strict ITAR/EAR export control regulations, limiting options for commercial cloud-based training platforms
- **Modern Learning Expectations**: Newer engineers expect interactive, self-paced learning experiences rather than "read the manual" approaches

### 1.3 Apps, Tools, Utilities, Services, and Websites of Interest

**Learning Management Systems (LMS) Evaluated:**
| Platform | Status | Notes |
|----------|--------|-------|
| Moodle (self-hosted) | **SELECTED** | Open-source, ITAR-compliant when self-hosted, highly customizable |
| Learn365 (LMS365) | Pending Security Review | Microsoft ecosystem native, ~$10-15/user/month |
| TalentLMS | REJECTED | Cloud-based, cannot meet ITAR/EAR requirements |
| 360Learning | REJECTED | Cloud-based, security concerns |
| SharePoint-based custom LMS | Alternative | Possible with Power Apps/Power Automate, higher dev cost |
| Confluence | REJECTED | Good for docs, lacks LMS capabilities |

**Documentation & Knowledge Base:**
| Platform | Status | Notes |
|----------|--------|-------|
| GitBook | Considered | Excellent code snippet support, Markdown-based |
| Notion | Considered | Flexible, good for mixed content |
| SharePoint | **SELECTED** | Already approved within NASA MSFC |

**Code Repository & Version Control:**
| Platform | Status | Notes |
|----------|--------|-------|
| Enterprise GitHub | **SELECTED** | Already secured within NASA infrastructure |
| GitLab | Not evaluated | Potential alternative |

**Collaboration & Communication:**
| Platform | Status | Notes |
|----------|--------|-------|
| Microsoft Teams | **SELECTED** | Already deployed at NASA MSFC |
| Slack | REJECTED | Not approved for NASA use |

**Interactive Learning & Code Execution:**
| Platform | Status | Notes |
|----------|--------|-------|
| JupyterHub (self-hosted) | Interested | For Python automation training |
| Custom ROCETS sandbox | Future | Containerized environment for hands-on practice |

**Analytics & Reporting:**
| Platform | Status | Notes |
|----------|--------|-------|
| Power BI | Considered | Part of Microsoft ecosystem |

### 1.4 Strategic Objectives

1. **Phase 1 (Months 1-3): Foundation**
   
   - Set up core LMS (self-hosted Moodle)
   - Configure integration with existing GitHub Enterprise
   - Establish SharePoint document libraries for course materials
   - Create initial course structure aligned with competency framework

2. **Phase 2 (Months 3-6): Content Development**
   
   - Develop interactive training modules
   - Create assessment questions and practical exercises
   - Record expert video content (knowledge capture)
   - Build code snippet library for common ROCETS patterns
   - Establish templates and documentation standards

3. **Phase 3 (Months 6-9): Integration**
   
   - Configure Single Sign-On (SSO) via Active Directory across all platforms
   - Set up LTI (Learning Tools Interoperability) between Moodle and GitHub
   - Create unified search capabilities
   - Implement analytics dashboards with Power BI
   - Build custom integrations for ROCETS-specific workflows

4. **Phase 4 (Months 9-12): Launch & Iterate**
   
   - Pilot with small group of new analysts
   - Gather feedback and iterate on content/UX
   - Full rollout across the ROCETS team
   - Establish continuous improvement process
   - Measure effectiveness metrics (completion rates, knowledge retention)

### 1.5 Long-Term Potential / Potential Workflows

**Immediate Workflows:**

- Self-paced onboarding for new ROCETS analysts
- Just-in-time microlearning modules (5-15 minutes each)
- Competency assessments and certifications
- Peer code review circles for model development
- Mentorship program facilitated through the platform

**Future Possibilities:**

- **AI-Powered Learning Assistance**: Integrate Claude or similar AI to answer ROCETS-specific questions contextually
- **Automated Model Validation**: Training exercises that automatically check student models against reference solutions
- **Cross-Team Knowledge Sharing**: Expand beyond ROCETS to other NASA MSFC simulation tools
- **External Collaboration**: Sanitized training modules for contractor onboarding
- **Community of Practice**: Regular "ROCETS Talks" similar to internal TED talks
- **Gamification**: Badges, leaderboards, and model-building competitions

### 1.6 Unique Value Proposition

This framework uniquely addresses the intersection of:

- **Highly specialized domain knowledge** (liquid rocket engine physics and ROCETS-specific implementation)
- **Strict government security requirements** (ITAR/EAR compliance)
- **Legacy technology stack** (Fortran codebase from the 1980s)
- **Modern learning expectations** (interactive, self-paced, mobile-accessible)

No commercial off-the-shelf solution addresses all these requirements, necessitating a custom-integrated approach.

### 1.7 Core Philosophies Discussed

1. **Competency-Based Learning Over Topic-Based**
   
   - Structure curriculum around *what analysts can do*, not just *what they know*
   - Clear progression from fundamentals → application → mastery
   - Assessments tied to real-world tasks

2. **Integration Must Feel Seamless**
   
   - "One-stop shop" experience despite multiple underlying platforms
   - Single sign-on (one login)
   - Unified navigation and branding
   - Federated search across all platforms

3. **Hands-On Over Theoretical**
   
   - Prioritize practical exercises over passive reading
   - Real NASA mission scenarios as learning contexts
   - Progressive complexity in model-building exercises

4. **Security as a Feature, Not a Constraint**
   
   - ITAR/EAR compliance enables trust and broader adoption
   - Self-hosted infrastructure provides full audit capabilities
   - On-premise solutions give complete data control

5. **Organizational Buy-In is Critical**
   
   - Minimize the number of new tools to reduce approval friction
   - Leverage existing Microsoft ecosystem where possible
   - Demonstrate value with pilots before full rollout

---

## 2. Requirements Compilation

### 2.1 Explicit Requirements

**Security & Compliance:**

- [x] Must comply with ITAR/EAR export control regulations
- [x] No data transmission to external/commercial cloud services
- [x] Full audit logging capabilities
- [x] Data must remain within NASA-controlled infrastructure

**Integration:**

- [x] Must integrate with existing Microsoft ecosystem (SharePoint, Teams, Active Directory)
- [x] Must integrate with existing Enterprise GitHub
- [x] Single Sign-On (SSO) via Active Directory
- [x] Learning Tools Interoperability (LTI) for GitHub integration

**Learning Platform:**

- [x] Course authoring and content management
- [x] Progress tracking and completion certificates
- [x] Assessment tools (quizzes, practical exercises)
- [x] Multimedia support (video, images, documents)
- [x] Mobile-accessible (at least for basic features)

**Budget:**

- [x] ~$100/user/month allocated
- [x] Budget covers enterprise hosting, development, and maintenance

### 2.2 Implied Requirements

**User Experience:**

- Platform must feel like a single unified system (not fragmented tools)
- Navigation should be intuitive for engineers (not education specialists)
- Minimal training required to use the training platform itself
- Search must work across all content sources

**Content:**

- Must accommodate code snippets with syntax highlighting (Fortran, Python)
- Must support technical documentation with equations and diagrams
- Version control for course materials
- Ability to update content without disrupting active learners

**Scalability:**

- Must support current team size (estimated 20-50 users initially)
- Architecture should scale for potential center-wide adoption
- Content should be reusable across different contexts

### 2.3 Derived Requirements

Based on discussions, these requirements emerged:

**Technical Infrastructure:**

- Self-hosted Moodle instance (on-premise or Azure Gov)
- Moodle plugins for specialized needs (code highlighting, GitHub integration)
- Custom SSO configuration against NASA Active Directory
- Power Automate workflows for notifications and tracking

**Content Development:**

- Expert interview process to capture institutional knowledge
- Standardized templates for different content types
- Review and approval workflow for new materials
- Localization not required (English only)

**Change Management:**

- Communication plan for rollout
- Training on the new tools for existing staff
- Feedback mechanisms for continuous improvement

### 2.4 Potential Edge Cases or Expansion Areas

**Edge Cases:**

- Contractor access (may require sanitized/non-ITAR training tracks)
- Users at other NASA centers (federated access?)
- Offline access (for users in areas with poor connectivity)
- Accessibility compliance (Section 508)

**Expansion Areas:**

- Integration with other NASA simulation tools (beyond ROCETS)
- AI-powered tutoring or Q&A assistance
- Virtual lab environments for hands-on practice
- Integration with HR systems for competency tracking
- External partnerships with universities

---

## 3. Architectural Overview

### 3.1 Current Design Decisions with Rationale

**Decision 1: Self-Hosted Moodle as Primary LMS**

- **Rationale:** Only open-source LMS that can be fully self-hosted to meet ITAR/EAR requirements while providing enterprise-grade features
- **Alternatives Ruled Out:**
  - TalentLMS, 360Learning: Cloud-based, cannot meet security requirements
  - Custom SharePoint LMS: Higher development cost (~$50-100k), ongoing maintenance burden
  - Learn365: Pending security review, may be considered if approved

**Decision 2: Leverage Existing Microsoft Ecosystem**

- **Rationale:** Minimizes new tool adoption, reduces approval friction, SSO already established
- **Components:** SharePoint for documents, Teams for discussions, Active Directory for authentication, Power BI for analytics

**Decision 3: Enterprise GitHub for Code Repository**

- **Rationale:** Already secured within NASA infrastructure, industry standard, integrates with Moodle via LTI
- **Alternatives Ruled Out:**
  - External GitHub: Security concerns
  - GitLab: Not evaluated due to existing GitHub availability

**Decision 4: Competency-Based Curriculum Structure**

- **Rationale:** More effective for skill development than topic-based learning; aligns assessments with real-world capabilities

- **Structure:**
  
  ```
  Level 1: Fundamentals
  ├── Thermodynamics basics
  ├── Fluid dynamics principles
  ├── ROCETS interface navigation
  └── Basic model setup
  
  Level 2: Application
  ├── Component modeling
  ├── System integration
  ├── Troubleshooting techniques
  └── Python automation basics
  
  Level 3: Advanced
  ├── Complex system modeling
  ├── Custom component development
  ├── Advanced Python integration
  └── Model optimization
  ```

### 3.2 Technology Stack

**Core Platform:**

- **LMS:** Self-hosted Moodle (on-premise or Azure Gov)
- **Authentication:** Microsoft Active Directory with SAML/SSO
- **Document Management:** SharePoint (O365 GCC/GCC-High)
- **Code Repository:** Enterprise GitHub
- **Collaboration:** Microsoft Teams
- **Analytics:** Power BI (if in tenant)

**Integration Layer:**

- LTI (Learning Tools Interoperability) for GitHub ↔ Moodle
- SharePoint embedded content in Moodle courses
- Teams meeting scheduling from Moodle
- Power Automate for workflow automation

**Security Architecture:**

```
[NASA Secure Network]
├── Moodle Server (on-premise/Azure Gov)
│   ├── ITAR/EAR content stays here
│   ├── No external data transmission
│   └── Full audit logging
├── SharePoint (O365 GCC/GCC-High)
│   └── Already approved for CUI
├── GitHub Enterprise
│   └── On-premise or Enterprise Cloud
└── All authenticate via AD/SAML
```

### 3.3 Frameworks Decided to Use

| Framework/Tool | Purpose        | Notes                                             |
| -------------- | -------------- | ------------------------------------------------- |
| Moodle         | LMS            | Self-hosted, extensive plugin ecosystem           |
| LTI            | Integration    | Standard for connecting LMS with external tools   |
| SAML/SSO       | Authentication | Leverages existing AD infrastructure              |
| Power Automate | Workflow       | Notifications, tracking, automation               |
| Markdown       | Documentation  | Standard for technical docs, version-controllable |

**Frameworks Ruled Out:**
| Framework/Tool | Reason for Rejection |
|----------------|---------------------|
| SCORM | Overly complex for our needs; Moodle native tools sufficient |
| External APIs | Security concerns with data leaving NASA network |
| WebSockets | Not required for current use cases |

### 3.4 Key Architectural Constraints

1. **No external data transmission** - All data must remain within NASA-controlled infrastructure
2. **ITAR/EAR compliance** - Training materials may contain export-controlled information
3. **Microsoft ecosystem integration** - Must work seamlessly with existing tools
4. **Limited IT support** - Solution must be maintainable with minimal dedicated IT staff
5. **Budget ceiling** - ~$100/user/month must cover all hosting, licensing, and development

### 3.5 Potential Future Directions

1. **AI-Augmented Learning**
   
   - Claude integration for answering ROCETS-specific questions
   - Automated assessment of student models
   - Personalized learning path recommendations

2. **Virtual Lab Environment**
   
   - Containerized ROCETS sandbox for hands-on practice
   - Pre-configured models for training exercises
   - Automated grading of model outputs

3. **Cross-Center Expansion**
   
   - Federated access for other NASA centers
   - Shared course libraries
   - Collaborative model development

4. **External Collaboration**
   
   - Sanitized training tracks for contractors
   - University partnership programs
   - Open courseware for non-controlled content

---

# II. Project Roadmap and Backlog

> *"Think of this backlog like a mission control dashboard for your project. We'll track project background information, feature ideas, progress, prioritize tasks, and ensure smooth navigation through your technical journey!"* 🚀🛠️

## Project Status Overview

| Category                 | Status                                                     |
| ------------------------ | ---------------------------------------------------------- |
| **Phase**                | Planning / Pre-Implementation                              |
| **Primary Blocker**      | Security verification for Learn365 (alternative to Moodle) |
| **Next Major Milestone** | LMS platform selection finalization                        |
| **Budget Status**        | Allocated (~$100/user/month)                               |

---

## Active Workstreams

### WS-1: LMS Platform Selection & Setup

**Priority:** 🔴 Critical  
**Status:** In Progress

| Task                                          | Status     | Notes                         |
| --------------------------------------------- | ---------- | ----------------------------- |
| Evaluate Moodle self-hosting requirements     | ✅ Complete | Primary recommendation        |
| Investigate Learn365 security compliance      | ⏳ Pending  | Awaiting security team review |
| Design SSO integration with Active Directory  | 📋 Planned | Depends on platform selection |
| Procure/provision server infrastructure       | 📋 Planned | On-prem or Azure Gov          |
| Initial Moodle installation and configuration | 📋 Planned | After platform finalization   |

### WS-2: Curriculum Structure Development

**Priority:** 🟡 High  
**Status:** Defined

| Task                                            | Status     | Notes                                 |
| ----------------------------------------------- | ---------- | ------------------------------------- |
| Define competency framework (Levels 1-3)        | ✅ Complete | See Section 3.1                       |
| Map existing ROCETS documentation to curriculum | 📋 Planned | Two PDFs identified as core reference |
| Identify SMEs for expert interviews             | 📋 Planned | Critical for knowledge capture        |
| Develop assessment criteria per competency      | 📋 Planned |                                       |
| Create course templates                         | 📋 Planned |                                       |

### WS-3: Integration Architecture

**Priority:** 🟡 High  
**Status:** Designed

| Task                                          | Status     | Notes              |
| --------------------------------------------- | ---------- | ------------------ |
| Design GitHub Enterprise ↔ Moodle integration | ✅ Designed | Via LTI            |
| Design SharePoint ↔ Moodle integration        | ✅ Designed | Embedded content   |
| Design Teams ↔ Moodle integration             | ✅ Designed | Meeting scheduling |
| Configure federated search                    | 📋 Planned |                    |
| Build Power BI dashboards                     | 📋 Planned |                    |

---

## Feature Backlog

### Must Have (P0)

- [ ] Self-hosted LMS with course authoring
- [ ] SSO via Active Directory
- [ ] Progress tracking and completion certificates
- [ ] Assessment tools (quizzes, practical exercises)
- [ ] ITAR/EAR compliant infrastructure
- [ ] SharePoint document integration
- [ ] Basic analytics/reporting

### Should Have (P1)

- [ ] GitHub Enterprise integration for code exercises
- [ ] Video hosting/streaming capability
- [ ] Microlearning module support (5-15 min lessons)
- [ ] Mobile-friendly access
- [ ] Automated notifications (assignment reminders, etc.)
- [ ] Peer review workflow

### Nice to Have (P2)

- [ ] Gamification (badges, leaderboards)
- [ ] AI-powered Q&A assistance
- [ ] Virtual lab environment (containerized ROCETS)
- [ ] External collaboration features (contractor access)
- [ ] Advanced analytics (learning path optimization)
- [ ] Offline access capability

---

## Risks and Mitigations

| Risk                                     | Likelihood | Impact | Mitigation                                                 |
| ---------------------------------------- | ---------- | ------ | ---------------------------------------------------------- |
| Learn365 fails security review           | Medium     | Medium | Moodle already identified as secure alternative            |
| SME availability for content development | High       | High   | Start knowledge capture immediately; prioritize interviews |
| IT support for self-hosted Moodle        | Medium     | Medium | Document architecture thoroughly; consider managed hosting |
| User adoption resistance                 | Medium     | Medium | Pilot program, executive sponsorship, clear communication  |
| Budget overrun                           | Low        | Medium | Phased implementation, track costs closely                 |

---

## Reference Materials

### Core Technical Documents

1. **ROCETS Development Final Report** (FR-20284, May 1990) - Located in `/mnt/project/`
   
   - System architecture and component descriptions
   - User's Manual
   - Engineering module documentation

2. **ROCket Engine Transient Simulation (ROCETS) System Paper** - Located in `/mnt/project/`
   
   - System overview
   - Feature summary
   - Design philosophy

### Key ROCETS Concepts for Curriculum

Based on the technical documentation, these are the core concepts trainees must master:

**System Architecture:**

- Library System (module storage)
- Executive Programs (Configuration Processor, Run Processor, Execution Processor, Output Processor)
- Simulation Input/Output
- Documentation Standards
- Maintenance Procedures

**Modeling Concepts:**

- Modules and Sub-modules
- Component-by-component modeling
- Algebraic loops and balances
- Steady-state trim balance
- Transient integration (Trapezoidal, Gear methods)
- Linearization and state-space models

**Practical Skills:**

- Configuration input file creation
- Run input specification
- Debugging and error handling
- Output interpretation
- Module development and interface

---

# III. Conversation Context Summary

## 1. Key Discussion Points

### Platform Evaluation Journey

The primary conversation in this Project space focused on evaluating platforms for the training framework. Key progression:

1. Started with Confluence inquiry → Found it lacks LMS capabilities
2. Explored TalentLMS → Rejected due to cloud-based architecture and ITAR concerns
3. Investigated SharePoint native capabilities → Found it can administer basic training but lacks key LMS features
4. Discovered Learn365 → Pending security verification
5. Identified self-hosted Moodle → Primary recommendation for ITAR compliance

### Integration Philosophy

A significant discussion emerged around what "integrated" truly means:

- Dan pushed back on recommendations for "multiple integrated platforms"
- Clarified that the solution must **feel like a single, one-stop shop**
- Established criteria for "feels integrated":
  - One login (SSO)
  - One search (federated)
  - Consistent navigation and branding
  - Data flows between systems but stays in control

### Security as Primary Constraint

Every recommendation had to pass the ITAR/EAR test:

- No data to external clouds
- Full audit logging
- On-premise or government cloud only
- Leverage already-approved Microsoft ecosystem

## 2. Notable Insights

1. **Budget is NOT the constraint** - With ~$100/user/month allocated, the limiting factor is security compliance, not cost

2. **Organizational approval friction** - The more tools in the stack, the harder to get approved; leverage existing Microsoft ecosystem wherever possible

3. **Self-hosted Moodle is viable** - Contrary to initial assumptions, Moodle can match commercial LMS features while meeting security requirements

4. **Competency-based approach** - Structuring around what analysts *can do* rather than what they *know* was identified as the most effective learning model

5. **Knowledge capture urgency** - Senior analysts approaching retirement makes documenting their expertise time-critical

## 3. Unresolved Questions

| Question                                                     | Status  | Notes                             |
| ------------------------------------------------------------ | ------- | --------------------------------- |
| Does Learn365 meet ITAR/EAR security requirements?           | Pending | Awaiting security team review     |
| What specific Moodle plugins are needed for ROCETS training? | Open    | Needs technical evaluation        |
| Who are the SMEs available for expert interviews?            | Open    | Need to identify and schedule     |
| What existing training materials can be migrated?            | Open    | Need content audit                |
| What is the realistic timeline for Phase 1 completion?       | Open    | Depends on IT resource allocation |

## 4. Potential Pivot Points

1. **Learn365 Approved** → Could simplify implementation significantly by staying fully within Microsoft ecosystem
2. **IT Support Unavailable** → May need to consider managed Moodle hosting (MoodleCloud) if on-premise not feasible
3. **Scope Expansion** → If successful, could expand to other simulation tools beyond ROCETS
4. **Budget Reduction** → Would need to prioritize core LMS over advanced features

## 5. What I've Learned About Dan

### Professional Context

- Aerospace engineer at NASA Marshall Space Flight Center
- Works on ROCETS (ROCket Engine Transient Simulator) for liquid rocket engine modeling
- 13 years of experience with the Fortran-based ROCETS codebase
- Models inform LRE design, development, fabrication, controls, and operation

### Technical Skills & Learning Goals

- **Fortran:** Limited fluency despite years of use; eager to learn more
- **Python:** Self-rated 2.0-2.5 out of 5; actively developing skills
- **OOP:** Relatively new to the paradigm, still developing understanding
- **GUI Development:** Complete newbie, interested in Eel framework (Python + HTML/CSS/JS)

### Learning Style & Preferences

- Appreciates examples and metaphors to illustrate concepts
- Prefers TL;DR at start of complex responses, plus section TL;DRs for multi-section responses
- Wants instructions reprinted after clarifying questions or debugging errors
- Interested in using Python sets more (requests examples where sets are the Pythonic choice)
- Values understanding "why" not just "how"

### Communication Preferences

- Uses "Project" (capitalized) when referring to Claude Project spaces
- Appreciates guidance on best practices and modern approaches
- Open to recommendations beyond the specific question asked
- Explicitly welcomes ideas and suggestions ("in this conversational space, there really aren't" stupid ideas)

### Other Interests & Projects (from Directory Structure document)

- ROCETS GUI development (separate from training framework)
- Home server/homelab projects (volkyra website, etc.)
- Logseq Personal Knowledge Base (PKB)
- Systems engineering practices

## 6. Additional Goals Expressed

1. **Hands-on learning prioritization** - Strong preference for practical exercises over theoretical reading
2. **Mobile accessibility** - Training should work on iPhone since that's where users often are
3. **Mentorship program facilitation** - Platform should support pairing new analysts with experienced ones
4. **Community of Practice** - Interest in "ROCETS Talks" and peer collaboration structures
5. **Knowledge capture through expert interviews** - Record senior analysts explaining complex topics

---

# Appendix: Additional Context for Migration

## Files Included in This Claude Project Space

1. **ROCket_Engine_Transient_Simulation__ROCETS__System.pdf**
   
   - Conference paper overview of ROCETS
   - System features summary
   - Good introduction for curriculum

2. **ROCETS_DEVELOPMENT_FINAL_REPORT_-__aka_Large_LRE_Transient_Performance_Simulation_System_.pdf**
   
   - Comprehensive technical documentation
   - User's Manual (Appendix A)
   - Engineering module examples
   - Core reference for detailed curriculum development

## Recommended Next Steps for Unified Project Space

1. **Finalize LMS Decision**
   
   - If Learn365 security review returns positive → evaluate as primary option
   - Otherwise → proceed with self-hosted Moodle planning

2. **Begin SME Identification**
   
   - Who are the senior ROCETS analysts?
   - What is their availability for knowledge capture?
   - What topics are most at risk of being lost?

3. **Content Audit**
   
   - What existing training materials exist?
   - What documentation can be converted to course modules?
   - What gaps exist between documentation and practical knowledge?

4. **Technical Architecture Document**
   
   - Formal design for the integrated platform
   - Security review documentation
   - IT resource requirements

5. **Stakeholder Communication**
   
   - Brief leadership on vision and approach
   - Identify pilot group
   - Establish feedback mechanisms

---

*This Project Migration Record was generated on January 29, 2026, as part of the AI-Augmented Fluid Project Management methodology. It represents a comprehensive snapshot of the ROCETS Analyst Training Framework project context at the time of migration.*
