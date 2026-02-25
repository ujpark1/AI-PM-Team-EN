# AI PM Team — Collaboration Workflow Guide

## Workflow Overview

This team operates on a **collaboration/iteration** model. A (Star) and B (Ann) discuss and refine ideas together, C (Diro) provides directional guidance and makes final decisions, and D (Dee) builds the product. Design work, design review, and UX writing review are integrated into the Pre-Launch and Post-Launch phases, directed by C (Diro).

**All deliverables from every phase must be saved to the `outputs/` directory.**

---

## Phase 1: Discovery

### Purpose
Discover market opportunities and derive promising product ideas.

### Process
```
┌─── Round 1 ───────────────────────────────────┐
│ A: Market analysis + brainstorm 3-5 ideas      │
│    ↓                                           │
│ B: Initial reactions + questions for each idea  │
│    ↓                                           │
│ A & B: Discussion — sharpen and refine ideas    │
└────────────────────────────────────────────────┘
         ↓ Share with C
┌─── C: Direction Feedback ────────────────────┐
│ "This direction is worth pursuing"             │
│ "Drop this one, dig deeper into that one"      │
│ "Consider this angle as well"                  │
└────────────────────────────────────────────────┘
         ↓
┌─── Round 2 (if needed) ─────────────────────┐
│ A & B: Additional exploration/discussion      │
│        reflecting C's feedback                 │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- A's Market Analysis Report
- A's Idea Briefs (3-5)
- A-B Discussion Summary
- C's Direction Feedback

### Required Inputs
- Domain/market to explore (if none, A explores based on trends)
- Team strengths/weaknesses (if available, A uses for fit assessment)
- Existing constraints (budget, tech stack, timeline, etc.)

---

## Phase 2: Validation

### Purpose
Validate the market competitiveness and feasibility of promising ideas.

### Process
```
┌─── Parallel Work ──────────────────────────────┐
│ A: Deep market opportunity analysis             │
│    - Detailed TAM/SAM/SOM estimation            │
│    - Positioning strategy draft                  │
│    - Business model options                      │
│                                                 │
│ B: Competitiveness & feasibility analysis       │
│    - Competitive analysis matrix                 │
│    - Unit economics review                       │
│    - Risk assessment                             │
│    - Technical feasibility                       │
└────────────────────────────────────────────────┘
         ↓
┌─── A & B Discussion (Core) ─────────────────┐
│ A: "This market is this large, with these     │
│     opportunities"                             │
│ B: "But there are these risks, and            │
│     competitors are strong"                    │
│ A: "From my experience, this pattern succeeds  │
│     because..."                                │
│ B: "Let's verify if that experience applies    │
│     here..."                                   │
│                                                │
│ → Organize consensus and disagreements         │
│ → Derive recommendation for C                  │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- A's Deep Market Opportunity Analysis Report
- B's Validation Report (competitiveness + feasibility + risk)
- A-B Discussion Conclusion (consensus/disagreements/recommendations)

---

## Phase 3: Decision

### Purpose
C synthesizes A and B's analysis to decide the final direction.

### Process
```
┌─── C's Comprehensive Judgment ───────────────┐
│ Inputs:                                       │
│   - A's market opportunity analysis            │
│   - B's validation report                      │
│   - A-B discussion conclusion                  │
│                                                │
│ C's Evaluation Criteria:                       │
│   1. Strategic Fit                             │
│   2. Market Attractiveness                     │
│   3. Executability                              │
│   4. Risk-Reward                               │
│                                                │
│ Decision:                                      │
│   → GO: Proceed with this idea                 │
│   → PIVOT: Revisit with modified direction     │
│   → KILL: Discard this idea                    │
│   → EXPLORE MORE: Additional exploration needed│
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- C's Decision Document (decision + rationale + reversal conditions)
- Execution Direction Brief

---

## Phase 4: Pre-Launch

### Purpose
Prepare strategy and infrastructure for product launch.

### Process
```
┌─── Parallel Work ──────────────────────────────┐
│ C (Diro): GTM Strategy Development             │
│    - Target customer + channels + messaging     │
│      + pricing                                  │
│    - Launch timeline + milestones               │
│    - Success criteria definition                │
│                                                 │
│ A (Star): Positioning & Message Strategy       │
│    - Final product positioning                  │
│    - Core messaging (tagline, value prop)       │
│    - Differentiation vs. competitors            │
│                                                 │
│ B (Ann): Data Tracking Design                  │
│    - KPI definition (North Star + leading/      │
│      lagging indicators)                        │
│    - Event tracking specs                       │
│    - Dashboard design                           │
│    - A/B testing framework                      │
│                                                 │
│ D (Dee): Architecture & MVP Build              │
│    - System architecture & tech stack           │
│    - Core feature implementation                │
│    - Deployment & CI/CD setup                   │
└────────────────────────────────────────────────┘
         ↓
┌─── Design Work (directed by C) ──────────────┐
│ - UI/UX design & interactive prototype         │
│ - Visual design system (colors, typography,    │
│   spacing, components)                         │
│ - Responsive layout & accessibility review     │
└────────────────────────────────────────────────┘
         ↓
┌─── Design Review (full team) ────────────────┐
│ - Usability & user flow consistency check      │
│ - Design-to-spec alignment with A's strategy   │
│ - Technical feasibility sign-off by D (Dee)    │
│ - C (Diro) approves final design               │
└────────────────────────────────────────────────┘
         ↓
┌─── UX Writing Review (directed by C) ────────┐
│ - Copy audit: tone, clarity, consistency       │
│ - Onboarding text, UI microcopy, error msgs    │
│ - Voice chart alignment (if new product)       │
│ - Conversion-focused CTA optimization          │
│ - C (Diro) approves final copy                 │
└────────────────────────────────────────────────┘
         ↓
┌─── C (Diro): Final Review & Approval ────────┐
│ - GTM strategy finalized                       │
│ - B's data tracking design reviewed & approved │
│ - Design reviewed & approved                   │
│ - UX writing reviewed & approved               │
│ - Launch checklist final check                 │
│ - GO/NO-GO launch decision                     │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- C's GTM Strategy
- A's Positioning & Message Strategy
- B's Data Tracking Design Document (KPI + event specs + dashboard)
- D's Technical Architecture & MVP
- UI/UX Design & Prototype
- Design Review Sign-off
- UX Writing Copy Package (onboarding, UI text, error messages)
- UX Writing Review Sign-off
- C's Launch Checklist
- C's Launch GO/NO-GO Decision

---

## Phase 5: Post-Launch

### Purpose
Adjust product direction based on post-launch data.

### Process

#### Day 1-30: Validate
```
┌─── Weekly Cycle ─────────────────────────────┐
│ B (Ann): Write weekly data analysis report     │
│    - Core KPI status                           │
│    - Funnel analysis                           │
│    - Anomalies and initial insights            │
│         ↓                                      │
│ C (Diro): Report review + strategic interpret. │
│    - "What this data means is..."              │
│    - Decide immediate adjustments              │
│    - Request additional analysis from B        │
│      (if needed)                               │
│         ↓                                      │
│ A (Star): Explore opportunities from user      │
│    feedback (if needed)                        │
│    - Discover opportunities in unexpected      │
│      usage patterns                            │
│         ↓                                      │
│ D (Dee): Implement fixes & improvements        │
│         ↓                                      │
│ Design Iteration (directed by C):              │
│    - Update UI based on user feedback & data   │
│    - Usability improvements                    │
│         ↓                                      │
│ UX Writing Iteration (directed by C):          │
│    - Improve underperforming UX text           │
│    - Optimize error messages, CTAs             │
└────────────────────────────────────────────────┘
```

#### Day 31-60: Optimize
```
┌─── Deep Analysis Cycle ──────────────────────┐
│ B (Ann): Cohort analysis + segment analysis    │
│    - Which user groups retain best?            │
│    - What are the main causes of churn?        │
│    - A/B test results analysis                 │
│         ↓                                      │
│ C (Diro): Determine optimization priorities    │
│    - Where to focus resources?                 │
│    - What experiments to run next?             │
│         ↓                                      │
│ D (Dee): Implement optimizations               │
│ Design: Refine UI based on insights            │
│ UX Writing: Optimize copy based on insights    │
│    (all directed by C)                         │
└────────────────────────────────────────────────┘
```

#### Day 61-90: Scale or Pivot
```
┌─── Strategic Judgment ───────────────────────┐
│ B (Ann): Comprehensive performance analysis    │
│    - Unit economics: actual vs. plan           │
│    - Growth modeling                           │
│    - Scenario analysis                         │
│         ↓                                      │
│ C (Diro): Final direction decision             │
│    - SCALE: Invest in growth acceleration      │
│    - PIVOT: Change direction (→ back to        │
│      Phase 1)                                  │
│    - KILL: Terminate the project               │
│         ↓                                      │
│ A (Star): (On PIVOT) Explore new opportunity   │
│    directions                                  │
│ D (Dee): (On SCALE) Scale architecture         │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- B's Weekly Data Analysis Reports
- B's Cohort/Segment Deep Analysis
- C's Optimization Priority Document
- C's Scale/Pivot/Kill Decision Document
- Design Iteration Reports (if applicable)
- UX Writing Iteration Reports (if applicable)

---

## Design & UX Writing Workflow

### When Design or UX Writing Is Needed
C (Diro) identifies design or copy needs at any phase and directs the work:

```
┌─── Design Task ──────────────────────────────┐
│ C (Diro) identifies a design need              │
│    ↓                                           │
│ Design work executes:                          │
│    - UI/UX wireframes & prototypes             │
│    - Visual design (colors, typography, etc.)  │
│    - Interaction design & user flows           │
│         ↓                                      │
│ Design Review (full team):                     │
│    - A (Star): Brand & positioning alignment   │
│    - B (Ann): Data-informed usability check    │
│    - D (Dee): Technical feasibility sign-off   │
│    - C (Diro): Final approval                  │
└────────────────────────────────────────────────┘

┌─── UX Writing Task ──────────────────────────┐
│ C (Diro) identifies a copy need                │
│    ↓                                           │
│ UX Writing executes:                           │
│    - Onboarding text, UI microcopy             │
│    - Error messages, empty states              │
│    - CTAs, tooltips, notifications             │
│    - Voice & tone consistency                  │
│         ↓                                      │
│ UX Writing Review:                             │
│    - Tone, clarity, consistency audit          │
│    - Conversion optimization check             │
│    - A (Star): Messaging alignment             │
│    - C (Diro): Final approval                  │
└────────────────────────────────────────────────┘
```

---

## Role Reference Files

| Role | Role Definition | Knowledge Base |
|------|----------------|----------------|
| A — Star (Strategist) | `roles/agent-a-star-strategist.md` | `knowledge/knowledge-a-strategist.md` |
| B — Ann (Analyst) | `roles/agent-b-ann-analyst.md` | `knowledge/knowledge-b-analyst.md` |
| C — Diro (Director) | `roles/agent-c-diro-director.md` | `knowledge/knowledge-c-director.md` |
| D — Dee (Developer) | `roles/agent-d-dee-developer.md` | `knowledge/knowledge-shared-fundamentals.md` |

All roles share: `knowledge/knowledge-shared-fundamentals.md`

---

## Usage Guide

### When Using as AI Agents
1. Copy the prompt section from each role's `roles/*.md` file
2. Provide the corresponding `knowledge/*.md` content as reference material
3. Interact with each agent following the workflow phases above
4. **All outputs must be saved to the `outputs/` directory**

### Example: Starting the Discovery Phase
```
[To Star (Agent A)]
"Analyze the current AI-based B2B SaaS market and brainstorm 3-5 promising product ideas."

[Star's results to Ann (Agent B)]
"Star proposed the following ideas. Validate market competitiveness and feasibility for each."
→ Attach Star's idea briefs

[Star and Ann's discussion results to Diro (Agent C)]
"Star and Ann analyzed and discussed as follows. Synthesize and decide on the direction."
→ Attach Star-Ann discussion summary

[After Go decision — to Dee (Agent D)]
"Diro approved this product direction. Design the technical architecture and build the MVP."
→ Attach Diro's decision document
```
