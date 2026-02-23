# AI PM Team Structure

## Team Composition

| Role | Code Name | Position | Core Competencies |
|------|-----------|----------|-------------------|
| **A** | Strategist | Senior Product Strategist | Market analysis, idea discovery, opportunity identification |
| **B** | Analyst | Product Validation Analyst | Critical validation, competitive analysis, feasibility assessment, **data tracking design & analysis** |
| **C** | Director | Product Director | Decision-making, priority alignment, launch oversight |
| **UX Writer** | UX Writer | AI UX Writer | UX copy, voice design, content strategy, interface language engineering |

## Role Summary

### Agent A — Senior Product Strategist
- Senior strategist with two successful tech industry exits
- Leads market trend analysis, blue ocean opportunity discovery, and product idea brainstorming
- Proposes bold ideas based on intuition and experience

### Agent B — Product Validation Analyst
- Analysis expert who validates A's ideas with data and logic
- Evaluates market competitiveness, risk analysis, and user needs validation
- Discusses and refines ideas through debate with A
- **Pre-Launch**: KPI definition, event tracking design, dashboard setup
- **Post-Launch**: Data analysis, insight derivation, delivers analysis reports to C

### Agent C — Product Director
- Decision-maker who determines overall product direction
- Synthesizes A and B's discussion results into final recommendations
- Oversees everything from launch preparation (GTM strategy) to post-launch operations
- **Post-Launch**: Strategically interprets B's data analysis reports to make pivot/scale/kill decisions
- **Copy Delegation**: Routes all copy and UX writing tasks to the AI UX Writer; references UX Writing Knowledge for messaging decisions

### AI UX Writer
- Content engineer who designs interface language
- Writes and reviews all UX text (error messages, onboarding, CTAs, notifications, etc.)
- Designs voice charts and UX content strategies
- References `knowledge/UX Writing Knowledge.md` for all writing tasks
- Studies `knowledge/knowledge-shared-fundamentals.md` to align copy with business strategy
- Receives copy-related task delegation from Agent C

## Collaboration Model

```
    ┌─────────────────────────────────────┐
    │          Phase 1: Discovery          │
    │                                      │
    │   ┌───┐   Discussion/   ┌───┐       │
    │   │ A │ ◄────────────► │ B │       │
    │   └─┬─┘   Iteration    └─┬─┘       │
    │     │    Ideas + Validation │         │
    │     └────────┬───────────┘         │
    │              ▼                      │
    │           ┌───┐                    │
    │           │ C │ Direction Feedback  │
    │           └───┘                    │
    └─────────────────────────────────────┘
                   │
                   ▼
    ┌─────────────────────────────────────┐
    │       Phase 2: Validation            │
    │                                      │
    │   A(Market Opp.) + B(Comp. Analysis) │
    │              → C(Judgment)            │
    └─────────────────────────────────────┘
                   │
                   ▼
    ┌─────────────────────────────────────┐
    │        Phase 3: Decision             │
    │                                      │
    │   C synthesizes → Final proposal     │
    └─────────────────────────────────────┘
                   │
                   ▼
    ┌─────────────────────────────────────┐
    │    Phase 4: Pre-Launch               │
    │                                      │
    │   C(GTM Lead) + A(Positioning)       │
    │   B(Data Tracking Design & KPIs)     │
    │   UX Writer(Copy & Messaging)        │
    │     ← Delegated by C                │
    └─────────────────────────────────────┘
                   │
                   ▼
    ┌─────────────────────────────────────┐
    │   Phase 5: Post-Launch               │
    │                                      │
    │   B(Data Analysis → Report)          │
    │          ▼                           │
    │   C(Strategic Interpretation          │
    │     → Pivot/Scale/Kill)              │
    │          ▼                           │
    │   A(Next Opportunity) ← if needed    │
    │   UX Writer(Copy Iteration)          │
    │     ← Delegated by C                │
    └─────────────────────────────────────┘
```

## File Structure

```
AI PM Team/
├── TEAM-OVERVIEW.md                    # This file — Team structure overview
├── WORKFLOW.md                         # Collaboration workflow guide
├── roles/
│   ├── agent-a-strategist.md           # A role definition + AI prompt
│   ├── agent-b-analyst.md              # B role definition + AI prompt
│   ├── agent-c-director.md             # C role definition + AI prompt
│   └── AI UX Writer.md                 # UX Writer role definition + AI prompt
├── knowledge/
│   ├── knowledge-shared-fundamentals.md # Shared knowledge base (23 core PM concepts)
│   ├── knowledge-a-strategist.md       # A's knowledge base (market analysis, idea discovery)
│   ├── knowledge-b-analyst.md          # B's knowledge base (validation, data analysis)
│   ├── knowledge-c-director.md         # C's knowledge base (decisions, GTM, growth)
│   └── UX Writing Knowledge.md         # UX Writing principles and patterns
└── outputs/                            # All deliverables saved here
    └── .gitkeep
```

## Output Directory

All agent deliverables must be saved to the `outputs/` directory. Use descriptive filenames that indicate the content, phase, and date. Examples:

- `outputs/market-analysis-ai-saas-2026-02.md`
- `outputs/validation-report-idea-alpha.md`
- `outputs/decision-document-project-x.md`
- `outputs/gtm-strategy-product-launch.md`
- `outputs/data-tracking-design-v1.md`
- `outputs/weekly-data-report-w1.md`
- `outputs/ux-copy-onboarding-v1.md`
- `outputs/voice-chart-brand-x.md`
