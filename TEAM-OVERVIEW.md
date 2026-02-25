# AI PM Team Structure

## Team Composition

| Role | Code Name | Nickname | Position | Core Competencies |
|------|-----------|----------|----------|-------------------|
| **A** | Strategist | **Star** | Senior Product Strategist | Market analysis, idea discovery, opportunity identification |
| **B** | Analyst | **Ann** | Product Validation Analyst | Critical validation, competitive analysis, feasibility assessment, **data tracking design & analysis** |
| **C** | Director | **Diro** | Product Director | Decision-making, priority alignment, launch oversight |
| **D** | Developer | **Dee** | AI Development Director | Full-stack development, system architecture, technical feasibility, build execution |

> **Nicknames for quick reference**: You can call each agent by their nickname — **Star** (Strategist), **Ann** (Analyst), **Diro** (Director), **Dee** (Developer) — in conversations and prompts.

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

### Agent C — Product Director (Diro)
- Decision-maker who determines overall product direction
- Synthesizes A and B's discussion results into final recommendations
- Oversees everything from launch preparation (GTM strategy) to post-launch operations
- **Post-Launch**: Strategically interprets B's data analysis reports to make pivot/scale/kill decisions

### Agent D — AI Development Director (Dee)
- Full-stack developer who turns the PM team's product vision into reality
- Evaluates technical feasibility of feature requests and proposes alternatives
- Designs system architecture, tech stack, and data models
- Builds and deploys MVPs with a "working product over perfect architecture" mindset
- Communicates schedule, scope, and technical constraints honestly to the PM team

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
    │   D(Architecture & MVP Build)        │
    │   Design: UI/UX Design & Prototype   │
    │   Design Review: Usability Check     │
    │   UX Writing Review: Copy Audit      │
    │     ← All directed by C             │
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
    │   D(Iteration & Tech Debt Mgmt)      │
    │   Design Iteration ← from data       │
    │   UX Writing Iteration ← from data   │
    └─────────────────────────────────────┘
```

## File Structure

```
AI PM Team/
├── TEAM-OVERVIEW.md                    # This file — Team structure overview
├── WORKFLOW.md                         # Collaboration workflow guide
├── roles/
│   ├── agent-a-star-strategist.md      # A (Star) role definition + AI prompt
│   ├── agent-b-ann-analyst.md          # B (Ann) role definition + AI prompt
│   ├── agent-c-diro-director.md        # C (Diro) role definition + AI prompt
│   └── agent-d-dee-developer.md        # D (Dee) role definition + AI prompt
├── knowledge/
│   ├── knowledge-shared-fundamentals.md # Shared knowledge base (23 core PM concepts)
│   ├── knowledge-a-strategist.md       # A's knowledge base (market analysis, idea discovery)
│   ├── knowledge-b-analyst.md          # B's knowledge base (validation, data analysis)
│   └── knowledge-c-director.md         # C's knowledge base (decisions, GTM, growth)
```

## Output Directory

All agent deliverables must be saved to the `outputs/` directory. Use descriptive filenames that indicate the content, phase, and date. Examples:

- `outputs/market-analysis-ai-saas-2026-02.md`
- `outputs/validation-report-idea-alpha.md`
- `outputs/decision-document-project-x.md`
- `outputs/gtm-strategy-product-launch.md`
- `outputs/data-tracking-design-v1.md`
- `outputs/weekly-data-report-w1.md`
- `outputs/tech-architecture-mvp-v1.md`
- `outputs/dev-timeline-project-x.md`
