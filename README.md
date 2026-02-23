# AI PM Team

> **Turn any AI chatbot into a full product management team.**
> A plug-and-play prompt system that gives you a Senior Strategist, Validation Analyst, Product Director, and UX Writer — each with deep domain knowledge and structured collaboration workflows.

---

## What Is This?

This repo contains a set of **role-based AI agent prompts** and **knowledge bases** that simulate a high-functioning PM team. Instead of chatting with a generic AI, you get four specialized agents that think, debate, and decide like experienced product professionals.

| Agent | Role | What They Do |
|-------|------|-------------|
| **A** — Strategist | Senior Product Strategist | Market analysis, blue ocean opportunity discovery, bold idea generation |
| **B** — Analyst | Product Validation Analyst | Data-driven validation, competitive analysis, risk assessment, KPI design |
| **C** — Director | Product Director | Final decisions, GTM strategy, launch execution, performance management |
| **UX Writer** | AI UX Writer | Interface copy, voice design, UX content strategy, text audits |

## Why Use This?

### 1. Structured Thinking, Not Just Chat
Most AI conversations are freeform Q&A. This system forces **structured PM frameworks** into every interaction — TAM/SAM/SOM, JTBD, AARRR, RICE scoring, Retention curves, and 20+ more concepts are baked into each agent's decision-making.

### 2. Built-in Debate & Validation
Agent A proposes bold ideas. Agent B stress-tests them. Agent C makes the call. This **adversarial collaboration** catches blind spots that a single AI conversation misses. You get the benefit of multiple perspectives without needing multiple people.

### 3. End-to-End Product Lifecycle
Covers the full journey from **Discovery → Validation → Decision → Pre-Launch → Post-Launch**, with specific deliverables and handoff points at each phase. Not just ideation — all the way through launch and iteration.

### 4. Deep Knowledge, Not Surface-Level
Each agent carries a dedicated knowledge base with real frameworks, not generic advice:
- **23 core PM concepts** (PMF, Crossing the Chasm, Flywheel, Moat, Hook Model, Network Effects, etc.)
- **Role-specific expertise** (A: exit lessons & timing judgment, B: unit economics & cohort analysis, C: GTM playbooks & pivot criteria)
- **UX Writing principles** (voice design, component-specific patterns, 4-step editing filter)

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
  → C develops GTM strategy
  → A handles positioning & messaging
  → B designs data tracking & KPIs
  → UX Writer creates product copy (delegated by C)

Phase 5: Post-Launch
  → B analyzes data weekly
  → C interprets strategically
  → Iterate based on evidence
```

### Solo Mode (Single Agent)

Don't need the full team? Each agent works independently too:

- **Just need market analysis?** → Use Agent A alone
- **Need to validate an idea?** → Use Agent B alone
- **Making a product decision?** → Use Agent C alone
- **Writing UI copy?** → Use the UX Writer alone

---

## File Structure

```
├── README.md                              # This file
├── TEAM-OVERVIEW.md                       # Team structure & composition
├── WORKFLOW.md                            # Collaboration workflow guide
├── roles/
│   ├── agent-a-strategist.md              # Strategist role + prompt
│   ├── agent-b-analyst.md                 # Analyst role + prompt
│   ├── agent-c-director.md                # Director role + prompt
│   └── AI UX Writer.md                    # UX Writer role + prompt
└── knowledge/
    ├── knowledge-shared-fundamentals.md   # 23 core PM concepts (all agents)
    ├── knowledge-a-strategist.md          # Market analysis, idea discovery
    ├── knowledge-b-analyst.md             # Validation, data analysis
    ├── knowledge-c-director.md            # Decision-making, GTM, growth
    └── UX Writing Knowledge.md            # UX writing principles & patterns
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

- **Agent A**: Blue Ocean Strategy, Porter's Five Forces, timing judgment ("Why Now?" framework), exit lessons from B2B SaaS and Consumer Tech
- **Agent B**: Lean Startup validation, The Mom Test, unit economics, event tracking design (AARRR-based), cohort/funnel/A/B test frameworks
- **Agent C**: RAPID decision model, RICE/MoSCoW prioritization, PLG/SLG/CLG go-to-market playbooks, 30-60-90 day post-launch framework, pivot criteria
- **UX Writer**: Voice chart design (6 linguistic variables), component-specific heuristics, 4-step editing filter (Purposeful → Concise → Conversational → Clear), inclusive design principles

---

## Tips for Best Results

1. **Provide context** — The more specific your domain, constraints, and goals, the better the output
2. **Let agents debate** — Feed A's output to B, then B's critique back to A. The back-and-forth produces the sharpest insights
3. **Use C as the tiebreaker** — When A and B disagree, C synthesizes both views with a clear decision framework
4. **Attach knowledge bases** — The prompts reference specific knowledge files. Include them for the best results
5. **Iterate** — Run multiple rounds. The first pass is a starting point, not the final answer

---

## License

Internal use. Please don't share outside the team without permission.
