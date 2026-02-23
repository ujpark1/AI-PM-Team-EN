# Knowledge Base — Agent C (Product Director)

## 1. Decision-Making Frameworks

### RAPID Decision Model
- **R (Recommend)**: Recommender — A and B provide analysis and recommendations
- **A (Agree)**: Agreeer — Whether key stakeholder agreement is required
- **P (Perform)**: Performer — The entity responsible for execution after the decision
- **I (Input)**: Input Provider — The person who provides information needed for decision-making
- **D (Decide)**: Decider — **C (Director) makes the final decision**

### Decision Speed vs Quality Matrix
| | Easy to Reverse | Hard to Reverse |
|--|--------------|----------------|
| **High Impact** | Decide quickly, execute immediately | Decide carefully, review thoroughly |
| **Low Impact** | Decide instantly, can delegate | Decide quickly, monitor |

**Jeff Bezos's Type 1 / Type 2 Decisions**:
- **Type 1 (One-way door)**: Decisions that are hard to reverse → Carefully, data-driven
- **Type 2 (Two-way door)**: Decisions that are easy to reverse → Quickly, GO at 70% confidence

### Decision Anti-Patterns (Things to Avoid)
1. **Analysis Paralysis**: Delaying decisions while waiting for perfect data
2. **HiPPO**: Highest Paid Person's Opinion — Relying solely on the intuition of the most senior person
3. **Sunk Cost Fallacy**: "We've already invested, so let's keep going" — Judge based on future value
4. **Confirmation Bias**: Seeking only data that supports the desired conclusion
5. **Groupthink**: If everyone agrees, be suspicious

---

## 2. Product Strategy Frameworks

### Product Vision → Strategy → Roadmap
- **Vision**: How will the world be different in 3-5 years?
- **Strategy**: What path will we take toward that vision?
- **Roadmap**: What will we do in the next quarter/half-year?
- **Key Know-How**: Vision is fixed, strategy is updated annually, roadmap is updated quarterly

### RICE Prioritization Framework
- **R (Reach)**: Number of users this feature will affect
- **I (Impact)**: Impact on individual users (0.25/0.5/1/2/3)
- **C (Confidence)**: Confidence level of the estimate (%)
- **E (Effort)**: Required work effort (person-weeks)
- **RICE Score = (Reach × Impact × Confidence) / Effort**

### MoSCoW Prioritization
- **Must have**: Product is meaningless without it (MVP essential)
- **Should have**: Important but product works without it (v1.1)
- **Could have**: Nice to have but low priority (backlog)
- **Won't have (this time)**: Not doing it this time (explicitly excluded)

---

## 3. Go-To-Market (GTM) Strategy Know-How

### GTM Motion Types

**1. Product-Led Growth (PLG)**
- The product itself drives acquisition and conversion
- Suitable for: Self-service capable, low barriers to entry, viral elements present
- Examples: Slack, Notion, Figma, Dropbox
- **Key Metrics**: Sign-up → Activation rate, Free → Paid conversion rate, Viral coefficient (k)
- **Know-How**: The key in PLG is minimizing the time to the Aha Moment

**2. Sales-Led Growth (SLG)**
- Sales team drives customer acquisition
- Suitable for: B2B, high Annual Contract Value (ACV), complex decision-making structures
- **Key Metrics**: Pipeline value, Conversion rate, Average sales cycle, ACV
- **Know-How**: In early stages, Founder-led Sales is the most effective approach

**3. Community-Led Growth (CLG)**
- Community builds awareness and trust
- Suitable for: Developer tools, open source, creator tools
- **Key Metrics**: Community size, Number of contributors, Content production volume
- **Know-How**: A community is cultivated, not manufactured. Authenticity is key.

### Launch Strategy

**Soft Launch**
- Quiet release to a small group
- Purpose: Bug discovery, early feedback, establishing baseline for key metrics
- Duration: 2-4 weeks

**Public Launch**
- Product Hunt, Hacker News, social media, press, etc.
- **Product Hunt Know-How**:
  - Post at midnight PST on Tuesday-Thursday
  - Share with the community in advance (but no asking for upvotes)
  - The first hour is the most critical
  - Having a Hunter is advantageous

**Phased Launch**
1. **Alpha**: Internal testing + 5-10 close users
2. **Beta (Closed)**: Invite-only for 50-200 people
3. **Beta (Open)**: Anyone can sign up, use a waitlist
4. **GA (General Availability)**: Official release

### Pricing Strategy
- **Penetration Pricing**: Capture market share quickly with low prices → Raise later
- **Value-Based Pricing**: 10-20% of the value the customer receives
- **Competitor-Based**: 10-20% lower than competitors, or premium positioning
- **Key Know-How**: Lowering prices is much harder than raising them. Don't go too cheap from the start.

---

## 4. Post-Launch Performance Management Know-How

### 30-60-90 Day Framework

**Day 1-30: Validate**
- Goal: Confirm whether core hypotheses are correct
- Focus Metrics: Sign-up rate, Activation rate, Early retention
- Decision: Is the core feature working properly?
- **Analysis to Request from B**: Sign-up funnel analysis, Activation behavior analysis

**Day 31-60: Optimize**
- Goal: Improve weak metrics
- Focus Metrics: Conversion rate, Retention, NPS
- Decision: Where should resources be focused?
- **Analysis to Request from B**: Cohort analysis, Churn cause analysis, A/B test results

**Day 61-90: Scale or Pivot**
- Goal: Data-driven direction decision
- Focus Metrics: LTV/CAC, Unit economics, Growth rate
- Decision: Scale / Pivot / Kill
- **Analysis to Request from B**: Segment analysis, Growth modeling, Scenario analysis

### Pivot Decision Criteria

**Signals That a Pivot Is Needed**:
- Retention keeps declining without flattening out (leaky bucket)
- Core hypotheses are disproven by data
- CAC keeps rising and LTV is not improving
- Power users are using the product differently than expected

**Pivot Types**:
1. **Zoom-in Pivot**: Expand a single feature into the entire product
2. **Zoom-out Pivot**: Turn the entire product into a feature of a larger product
3. **Customer Segment Pivot**: Same product, different target
4. **Value Capture Pivot**: Same product, different revenue model
5. **Channel Pivot**: Same product, different distribution channel
6. **Technology Pivot**: Same problem, different technology solution

---

## 5. Stakeholder Communication

### SCQA Framework (Situation-Complication-Question-Answer)
- **S (Situation)**: Current situation/background
- **C (Complication)**: Problem/challenge
- **Q (Question)**: The key question to solve
- **A (Answer)**: The proposed solution

### Decision Document Template
```
## Title: [Decision Item]
## Date: [YYYY-MM-DD]
## Status: Draft / Under Review / Approved

### Background
Why is this decision needed?

### Options
| Option | Pros | Cons | Risks |
|------|------|------|--------|
| Option A | ... | ... | ... |
| Option B | ... | ... | ... |
| Option C | ... | ... | ... |

### Decision
The chosen option and rationale

### Reversal Conditions
What data would trigger a re-evaluation of this decision?

### Next Steps
Who, what, by when
```

---

## 6. Product Growth Engines

### Three Growth Engines (Eric Ries)

**1. Sticky Engine**
- Core: High retention → New users > Churned users → Growth
- Key Metric: Churn Rate
- Strategy: Strengthen core product value, build habits, increase switching costs
- Suitable for: Utilities, productivity tools, infrastructure

**2. Viral Engine**
- Core: Users bring in users → Viral coefficient (k) > 1
- Key Metric: Viral coefficient (k), Viral cycle time
- Strategy: Sharing incentives, network effects, social features
- Suitable for: Social, communication, collaboration tools

**3. Paid Engine**
- Core: LTV > CAC → Growth proportional to investment
- Key Metric: LTV/CAC ratio, Payback period
- Strategy: Marketing channel optimization, Sales efficiency
- Suitable for: B2B SaaS, high ACV products

### North Star Metric Connection
C should select a North Star Metric aligned with the growth engine and request B to track and analyze data centered around it.

| Growth Engine | North Star Metric Direction |
|-----------|----------------------|
| Sticky | WAU/MAU ratio, D7/D30 retention |
| Viral | Invites/shares count, Viral coefficient (k) |
| Paid | LTV/CAC ratio, Payback Period |
