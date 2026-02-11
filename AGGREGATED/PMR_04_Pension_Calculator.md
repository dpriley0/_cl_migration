# Project Migration Record: Pension Break-Even Calculator

---

# PART I: COMPREHENSIVE PROJECT SUMMARY

## 1. Project Vision

### High-Level Goals
Create an interactive calculator to help determine the break-even salary required from a new job to compensate for lost pension benefits, enabling data-driven career transition decisions.

### Motivations
- **Time-Sensitive Decision**: Laura was considering leaving her government job for a private sector opportunity
- **Complex Financial Trade-off**: Defined benefit pension vs. higher salary investment potential
- **Quantitative Analysis**: Provide concrete numbers for an emotional decision
- **Variable Exploration**: Understand how different assumptions change the outcome

### Apps, Tools, Utilities, and Services of Interest
- **Interactive Calculator** - Real-time updates as parameters change
- **Scenario Comparison** - Conservative, aggressive, and realistic investment approaches
- **Visual Charts** - Graph showing investment growth over time
- **Smart Recommendations** - Feasibility assessment based on calculated salary increase

### Strategic Objectives
1. Calculate present value of lost pension benefits
2. Determine annual salary increase needed to break even
3. Model different investment return scenarios
4. Provide actionable recommendations
5. Create reusable tool for future career decisions

### Long-term Potential / Potential Workflows
- Template for other defined benefit pension analysis
- Expandable to other financial decision tools
- Could be generalized for public use
- Foundation for financial planning toolkit on volkyra.com

### Unique Value Proposition
Personal financial modeling tool that transforms complex pension math into actionable career guidance.

### Core Philosophies Discussed
- **Quick and dirty first**: Get useful numbers fast, refine later
- **Multiple scenarios**: Don't rely on single assumption
- **Visual feedback**: Charts help understand the math
- **Practical recommendations**: Not just numbers, but guidance

---

## 2. Requirements Compilation

### Explicit Requirements

**Pension Parameters (Laura's Situation):**
- Defined benefit: 2% of final salary × years of service
- 10-year vesting requirement
- Currently 5 years into service
- Current salary: $208,000
- Planning 20 more years of work

**Salary Assumptions:**
- Current job: 2.5% annual raises
- New job: 3.5% annual raises

**Investment Parameters:**
- Different return scenarios needed
- Time horizon: 20 years working + 25 years retirement
- Discount rate for present value calculations

### Implied Requirements
- Interactive sliders for parameter adjustment
- Real-time calculation updates
- Visual charts for understanding
- Mobile-friendly (access from phone)
- Save/compare scenarios

### Derived Requirements
- JavaScript-based for responsiveness
- Chart.js for visualization
- No backend needed (client-side only)
- Deployable as static HTML

### Potential Edge Cases or Expansion Areas
- Salary caps (government pay limits)
- Partial pension scenarios
- Social Security integration
- Inflation adjustments
- Tax implications
- Other retirement benefits comparison

---

## 3. Architectural Overview

### Current Design Decisions with Rationale

**Pure JavaScript Web Application (IMPLEMENTED)**
- Originally built with Python/Eel framework
- Converted to pure HTML/CSS/JavaScript for:
  - Easy deployment to volkyra.com via S3
  - No server requirements
  - Instant responsiveness
  - Works offline

**Chart.js for Visualization (IMPLEMENTED)**
- Industry-standard charting library
- Good documentation
- Responsive and interactive

### Technology Stack
- **Frontend**: HTML/CSS/JavaScript (single file)
- **Charting**: Chart.js
- **Deployment**: Initially AWS S3, now Cloudflare Pages (volkyra.com)
- **State Management**: localStorage for saved scenarios (simulated)

### Key Technical Decisions Made

**Investment Return Scenarios Modeled:**
1. **Conservative (6% returns)**: ~$72,400 extra/year needed (35% increase)
2. **Aggressive (10% returns)**: ~$46,500 extra/year needed (22% increase)
3. **Realistic Tapering**: 10% for 13 years → taper to 4% → $52,400 extra/year (25% increase)

**Key Formula:**
- Future Value of Pension = Present Value × (1 + discount_rate)^years
- Required Annual Investment = Pension_FV / Annuity_Factor
- Annuity Factor = ((1 + return)^years - 1) / return

### Issues Encountered and Resolved
1. **Chart.js infinite growth bug** - Fixed with fixed canvas heights and CSS !important
2. **Dual input synchronization** - Added synchronized text inputs alongside sliders with tabindex
3. **Visual redundancy** - Consolidated to show each value only once
4. **Layout displacement on focus** - Resolved with GPU acceleration (transform: translateZ(0), will-change)

---

## 4. Current State

### What Was Built ✅
- Fully functional single-page calculator application
- Interactive sliders with synchronized text inputs
- Three scenario comparison (conservative, aggressive, tapering)
- Real-time Chart.js visualization
- Smart recommendations based on feasibility
- Glassmorphism UI design
- Tab navigation (tabindex 1-13)

### Key Findings from Analysis

| Scenario | Return Assumption | Extra Salary Needed | % Increase |
|----------|-------------------|---------------------|------------|
| Conservative | 6% flat | ~$72,400/year | 35% |
| Aggressive | 10% flat | ~$46,500/year | 22% |
| Realistic Tapering | 10%→4% | ~$52,400/year | 25% |

**Bottom Line**: New job would need to pay $46,500-$72,400 more per year depending on investment assumptions, representing a 22-35% salary increase from $208,000.

---

# PART II: PROJECT ROADMAP AND BACKLOG

## Mission Control Dashboard 🚀🛠️

### Purpose
This backlog tracks the pension break-even calculator - a financial decision tool built during a time-sensitive career evaluation.

### Current Status: ✅ COMPLETE (MVP Delivered)

---

## Completed ✅
- [x] Define financial model and formulas
- [x] Calculate pension value projections
- [x] Model conservative (6%) scenario
- [x] Model aggressive (10%) scenario
- [x] Model realistic tapering scenario
- [x] Create initial Python/Eel application
- [x] Convert to pure JavaScript/HTML
- [x] Implement interactive sliders
- [x] Add synchronized text inputs
- [x] Integrate Chart.js visualization
- [x] Fix canvas height bug
- [x] Fix layout displacement on focus
- [x] Add smart recommendations
- [x] Style with glassmorphism effects
- [x] Deploy to volkyra.com (AWS S3, later Cloudflare)

---

## Potential Enhancements (Future)
- [ ] Add inflation adjustment parameter
- [ ] Include tax implications modeling
- [ ] Add Social Security projections
- [ ] Create scenario save/compare functionality
- [ ] Add print/export feature
- [ ] Generalize for other pension types
- [ ] Add Monte Carlo simulation option

---

## Financial Model Summary

### Inputs
- Current salary
- Years already worked
- Years remaining to work
- Years in retirement
- Current job raise rate
- Pension formula rate (% per year of service)
- Discount rate (time value of money)
- Investment return rate(s)

### Calculations
1. **Final Salary** = Current × (1 + raise_rate)^years_to_work
2. **Annual Pension** = pension_rate × final_salary × total_years_service
3. **Pension PV** = Sum of (annual_pension / (1 + discount)^year) for retirement years
4. **Pension FV** = Pension_PV × (1 + discount)^years_to_work
5. **Annuity Factor** = ((1 + return)^years - 1) / return
6. **Required Investment** = Pension_FV / Annuity_Factor

### Key Insight
The 4% discount rate accounts for time value of money - $100 today is worth more than $100 in 20 years due to inflation and investment opportunity.

---

# PART III: CONVERSATION CONTEXT SUMMARY

## Key Discussion Points
1. **Quick turnaround needed**: This was a time-sensitive decision
2. **Multiple scenarios important**: Single assumptions are misleading
3. **Tapering returns realistic**: Investment strategy should become conservative near retirement
4. **The math is clear**: 22-35% salary increase needed to break even

## Notable Insights
- Defined benefit pensions are VERY valuable - guaranteed ~$170K/year for life in retirement
- The break-even analysis assumes she actually invests the salary difference
- 4% discount rate is a standard assumption for present value calculations
- Tapering investment returns (aggressive→conservative) better models real behavior

## Unresolved Questions
- What decision did Laura ultimately make?
- Should this tool be generalized for others?
- Integration with other financial planning tools?

## Potential Pivot Points
- Could expand to full financial planning suite
- Could publish as a public tool
- Could add other decision calculators

## What I've Learned About This Project
- Built during time-sensitive career evaluation for Laura
- Dan already has AWS infrastructure familiarity
- Chart.js has some quirks with dynamic sizing
- Glassmorphism UI was a design preference

## Technical Lessons Learned
- Chart.js needs explicit height constraints to prevent infinite growth
- `will-change: transform` helps prevent layout reflow during interactions
- Synchronized sliders + text inputs improve UX
- Single-file HTML apps are great for simple tools

---

## Related Projects
- **Volkyra.com Website**: Deployment destination
- **Financial Decision Tools**: Part of broader goal mentioned in backlog
- **AstralJunction**: Could host if more complex backend needed

## Migration Notes
This PMR consolidates the complete pension break-even calculator project. The tool was built, refined through multiple iterations to fix UI bugs, and deployed. It's considered complete (MVP), though enhancements are possible.
