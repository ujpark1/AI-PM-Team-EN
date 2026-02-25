# AI PM Team

> **Turn any AI chatbot into a full product management team.**
> A plug-and-play prompt system that gives you a Senior Strategist, Validation Analyst, Product Director, and Development Director — each with deep domain knowledge and structured collaboration workflows.

---

## What Is This?

This repo contains a set of **role-based AI agent prompts** and **knowledge bases** that simulate a high-functioning PM team. Instead of chatting with a generic AI, you get four specialized agents that think, debate, and decide like experienced product professionals.

| Agent | Nickname | Role | What They Do |
|-------|----------|------|-------------|
| **A** — Strategist | **Star** | Senior Product Strategist | Market analysis, blue ocean opportunity discovery, bold idea generation |
| **B** — Analyst | **Ann** | Product Validation Analyst | Data-driven validation, competitive analysis, risk assessment, KPI design |
| **C** — Director | **Diro** | Product Director | Final decisions, GTM strategy, launch execution, performance management |
| **D** — Developer | **Dee** | AI Development Director | Technical feasibility, system architecture, MVP build, deployment |

> **Quick tip**: Use nicknames — **Star**, **Ann**, **Diro**, **Dee** — when referring to agents in conversations and prompts.

## Why Use This?

### 1. Structured Thinking, Not Just Chat
Most AI conversations are freeform Q&A. This system forces **structured PM frameworks** into every interaction — TAM/SAM/SOM, JTBD, AARRR, RICE scoring, Retention curves, and 20+ more concepts are baked into each agent's decision-making.

### 2. Built-in Debate & Validation
Agent A (Star) proposes bold ideas. Agent B (Ann) stress-tests them. Agent C (Diro) makes the call. Agent D (Dee) builds it. This **adversarial collaboration** catches blind spots that a single AI conversation misses. You get the benefit of multiple perspectives without needing multiple people.

### 3. End-to-End Product Lifecycle
Covers the full journey from **Discovery → Validation → Decision → Pre-Launch → Post-Launch**, with specific deliverables and handoff points at each phase. Not just ideation — all the way through launch and iteration.

### 4. Deep Knowledge, Not Surface-Level
Each agent carries a dedicated knowledge base with real frameworks, not generic advice:
- **23 core PM concepts** (PMF, Crossing the Chasm, Flywheel, Moat, Hook Model, Network Effects, etc.)
- **Role-specific expertise** (A: exit lessons & timing judgment, B: unit economics & cohort analysis, C: GTM playbooks & pivot criteria, D: full-stack development & system architecture)

### 5. Consistent Quality
The prompts encode **specific output formats, decision frameworks, and constraints** so you get consistently high-quality deliverables every time — not random quality based on how you phrase your prompt.

---

## How to Use

### Quick Start (5 minutes)

1. **Pick an agent** from the `roles/` folder
2. **Copy the prompt** from the "AI Agent Prompt" section in the markdown file
3. **Paste it as a system prompt** (or first message) in your AI tool of choice
4. **Attach the knowledge base** from the `knowledge/` folder as context
5. **Start working**

### Recommended AI Tools
- **Claude** (Projects or plain chat)
- **ChatGPT** (Custom GPTs or system prompt)
- **Any LLM** that supports system prompts or long context

### Full Team Workflow

Follow the phases in `WORKFLOW.md` for the complete experience:

```
Phase 1: Discovery
  → Give Agent A a market/domain to explore
  → A brainstorms 3-5 ideas
  → Feed A's output to Agent B for validation
  → A & B debate and refine

Phase 2: Validation
  → A does deep market analysis
  → B does competitive & feasibility analysis
  → A & B discuss, document consensus & disagreements

Phase 3: Decision
  → Feed everything to Agent C
  → C evaluates and makes Go/Pivot/Kill decision

Phase 4: Pre-Launch
  → C (Diro) develops GTM strategy
  → A (Star) handles positioning & messaging
  → B (Ann) designs data tracking & KPIs
  → D (Dee) builds architecture & MVP

Phase 5: Post-Launch
  → B (Ann) analyzes data weekly
  → C (Diro) interprets strategically
  → D (Dee) iterates based on evidence
  → Repeat as needed
```

### Solo Mode (Single Agent)

Don't need the full team? Each agent works independently too:

- **Just need market analysis?** → Use Agent A (Star) alone
- **Need to validate an idea?** → Use Agent B (Ann) alone
- **Making a product decision?** → Use Agent C (Diro) alone
- **Need to build an MVP?** → Use Agent D (Dee) alone

---

## File Structure

```
├── README.md                              # This file
├── TEAM-OVERVIEW.md                       # Team structure & composition
├── WORKFLOW.md                            # Collaboration workflow guide
├── roles/
│   ├── agent-a-star-strategist.md          # Star — Strategist role + prompt
│   ├── agent-b-ann-analyst.md             # Ann — Analyst role + prompt
│   ├── agent-c-diro-director.md           # Diro — Director role + prompt
│   └── agent-d-dee-developer.md           # Dee — Developer role + prompt
└── knowledge/
    ├── knowledge-shared-fundamentals.md   # 23 core PM concepts (all agents)
    ├── knowledge-a-strategist.md          # Market analysis, idea discovery
    ├── knowledge-b-analyst.md             # Validation, data analysis
    └── knowledge-c-director.md            # Decision-making, GTM, growth
```

---

## Knowledge Base Highlights

### 23 Core PM Concepts (Shared by All Agents)

| Category | Concepts |
|----------|----------|
| Product & Market | PMF, JTBD, TAM/SAM/SOM, Crossing the Chasm |
| Growth & Metrics | AARRR, North Star Metric, Retention/Cohort, LTV/CAC |
| Strategy & Decisions | First Principles, Flywheel, Moat, Second-order Thinking, One-way/Two-way Door |
| Execution | MVP, Build-Measure-Learn, ICE/RICE, Opportunity Solution Tree |
| User & Behavior | Hook Model, Network Effects, Switching Cost |
| Organization | OKR, Two-Pizza Team, Disagree and Commit |

### Agent-Specific Knowledge

- **Agent A (Star)**: Blue Ocean Strategy, Porter's Five Forces, timing judgment ("Why Now?" framework), exit lessons from B2B SaaS and Consumer Tech
- **Agent B (Ann)**: Lean Startup validation, The Mom Test, unit economics, event tracking design (AARRR-based), cohort/funnel/A/B test frameworks
- **Agent C (Diro)**: RAPID decision model, RICE/MoSCoW prioritization, PLG/SLG/CLG go-to-market playbooks, 30-60-90 day post-launch framework, pivot criteria
- **Agent D (Dee)**: Full-stack development (Next.js, React, TypeScript, Supabase), system architecture design, MVP build & deploy, technical risk management

---

## Tips for Best Results

1. **Provide context** — The more specific your domain, constraints, and goals, the better the output
2. **Let agents debate** — Feed Star's output to Ann, then Ann's critique back to Star. The back-and-forth produces the sharpest insights
3. **Use Diro as the tiebreaker** — When Star and Ann disagree, Diro synthesizes both views with a clear decision framework
4. **Bring in Dee early** — Have Dee assess technical feasibility before the team commits to an idea
5. **Attach knowledge bases** — The prompts reference specific knowledge files. Include them for the best results
6. **Iterate** — Run multiple rounds. The first pass is a starting point, not the final answer

---

## License

Internal use. Please don't share outside the team without permission.
