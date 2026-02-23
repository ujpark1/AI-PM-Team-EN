# AI PM Team — Collaboration Workflow Guide

## Workflow Overview

This team operates on a **collaboration/iteration** model. A and B discuss and refine ideas together, C provides directional guidance and makes final decisions, and the UX Writer handles all copy-related tasks delegated by C.

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
│ C: GTM Strategy Development                    │
│    - Target customer + channels + messaging     │
│      + pricing                                  │
│    - Launch timeline + milestones               │
│    - Success criteria definition                │
│                                                 │
│ A: Positioning & Message Strategy              │
│    - Final product positioning                  │
│    - Core messaging (tagline, value prop)       │
│    - Differentiation vs. competitors            │
│                                                 │
│ B: Data Tracking Design                        │
│    - KPI definition (North Star + leading/      │
│      lagging indicators)                        │
│    - Event tracking specs                       │
│    - Dashboard design                           │
│    - A/B testing framework                      │
│                                                 │
│ UX Writer: Copy & Content (delegated by C)     │
│    - Onboarding text                            │
│    - UI microcopy                               │
│    - Error messages                             │
│    - Voice chart (if new product)               │
└────────────────────────────────────────────────┘
         ↓
┌─── C: Final Review & Approval ───────────────┐
│ - GTM strategy finalized                       │
│ - B's data tracking design reviewed & approved │
│ - UX Writer's copy reviewed & approved         │
│ - Launch checklist final check                 │
│ - GO/NO-GO launch decision                     │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- C's GTM Strategy
- A's Positioning & Message Strategy
- B's Data Tracking Design Document (KPI + event specs + dashboard)
- UX Writer's Copy Package (onboarding, UI text, error messages)
- UX Writer's Voice Chart (if applicable)
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
│ B: Write weekly data analysis report           │
│    - Core KPI status                           │
│    - Funnel analysis                           │
│    - Anomalies and initial insights            │
│         ↓                                      │
│ C: Report review + strategic interpretation    │
│    - "What this data means is..."              │
│    - Decide immediate adjustments              │
│    - Request additional analysis from B        │
│      (if needed)                               │
│         ↓                                      │
│ A: Explore opportunities from user feedback    │
│    (if needed)                                 │
│    - Discover opportunities in unexpected      │
│      usage patterns                            │
│         ↓                                      │
│ UX Writer: Iterate copy based on data          │
│    (delegated by C)                            │
│    - Improve underperforming UX text           │
│    - Optimize error messages, CTAs             │
└────────────────────────────────────────────────┘
```

#### Day 31-60: Optimize
```
┌─── Deep Analysis Cycle ──────────────────────┐
│ B: Cohort analysis + segment analysis          │
│    - Which user groups retain best?            │
│    - What are the main causes of churn?        │
│    - A/B test results analysis                 │
│         ↓                                      │
│ C: Determine optimization priorities           │
│    - Where to focus resources?                 │
│    - What experiments to run next?             │
│         ↓                                      │
│ UX Writer: Optimize copy based on insights     │
│    (delegated by C)                            │
└────────────────────────────────────────────────┘
```

#### Day 61-90: Scale or Pivot
```
┌─── Strategic Judgment ───────────────────────┐
│ B: Comprehensive performance analysis report   │
│    - Unit economics: actual vs. plan           │
│    - Growth modeling                           │
│    - Scenario analysis                         │
│         ↓                                      │
│ C: Final direction decision                    │
│    - SCALE: Invest in growth acceleration      │
│    - PIVOT: Change direction (→ back to        │
│      Phase 1)                                  │
│    - KILL: Terminate the project               │
│         ↓                                      │
│ A: (On PIVOT) Explore new opportunity          │
│    directions                                  │
└────────────────────────────────────────────────┘
```

### Deliverables (→ `outputs/`)
- B's Weekly Data Analysis Reports
- B's Cohort/Segment Deep Analysis
- C's Optimization Priority Document
- C's Scale/Pivot/Kill Decision Document
- UX Writer's Copy Iteration Reports (if applicable)

---

## Copy & UX Writing Workflow

### Delegation Flow
When any phase requires copy, UX writing, or content work:

```
┌─── Copy Task Identified ─────────────────────┐
│ C identifies a copy-related need               │
│    ↓                                           │
│ C delegates to UX Writer with context:         │
│    - Component/screen type                     │
│    - User journey stage                        │
│    - Brand voice requirements                  │
│    - Business objectives                       │
│         ↓                                      │
│ UX Writer executes:                            │
│    - References UX Writing Knowledge.md        │
│    - References knowledge-shared-fundamentals  │
│    - Applies 4-step editing filter             │
│    - Delivers output                           │
│         ↓                                      │
│ C reviews output:                              │
│    - Strategic alignment check                 │
│    - GTM messaging consistency check           │
│    - Discusses with UX Writer if needed         │
│         ↓                                      │
│ C approves or requests revision                │
└────────────────────────────────────────────────┘
```

---

## Role Reference Files

| Role | Role Definition | Knowledge Base |
|------|----------------|----------------|
| A (Strategist) | `roles/agent-a-strategist.md` | `knowledge/knowledge-a-strategist.md` |
| B (Analyst) | `roles/agent-b-analyst.md` | `knowledge/knowledge-b-analyst.md` |
| C (Director) | `roles/agent-c-director.md` | `knowledge/knowledge-c-director.md` + `knowledge/UX Writing Knowledge.md` |
| UX Writer | `roles/AI UX Writer.md` | `knowledge/UX Writing Knowledge.md` + `knowledge/knowledge-shared-fundamentals.md` |

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
[To Agent A]
"Analyze the current AI-based B2B SaaS market and brainstorm 3-5 promising product ideas."

[A's results to Agent B]
"Agent A proposed the following ideas. Validate market competitiveness and feasibility for each."
→ Attach A's idea briefs

[A and B's discussion results to Agent C]
"A and B analyzed and discussed as follows. Synthesize and decide on the direction."
→ Attach A-B discussion summary

[When copy is needed — C to UX Writer]
"We need onboarding copy for the chosen product. Here's the context: [product details, target user, voice requirements]."
→ UX Writer delivers, C reviews
```
