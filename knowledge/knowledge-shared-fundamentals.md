# Shared Knowledge — PM & Startup Core Concepts (23)

> **This is shared knowledge that all agents (A, B, C) must be familiar with.**
> Each concept includes its origin, core principles, practical application, and agent learning points.

---

## Part 1: Product & Market

### 1. Product-Market Fit (PMF)

**Origin**: Marc Andreessen (2007), Don Valentine (Sequoia), Andy Rachleff (Benchmark)

**Definition**: Being in a good market with a product that can satisfy that market. When PMF is achieved, you feel like "the product is being pulled out of you by the market."

**How to Assess PMF**:
- **Sean Ellis Test**: "How would you feel if you could no longer use this product?" → PMF is near when 40%+ say 'very disappointed'
- **Retention Curve**: If the retention rate flattens at a certain level over time, it is a strong PMF signal
- **Organic Growth**: User acquisition through word-of-mouth/referrals without marketing
- **NPS**: 50 or above indicates strong PMF

**Before vs After PMF**:
- Before PMF: Focus all resources on finding PMF. Growth investment and organizational expansion come after PMF
- Scaling without PMF = 'building a sandcastle on water'
- Paul Graham: "Do things that don't scale" — In the early days, meet customers personally and provide manual service

**Learning Point**: PMF is not binary but a spectrum. It gradually moves from 'weak PMF → strong PMF,' and PMF can weaken as the market changes. Continuous monitoring is required.

---

### 2. Jobs to Be Done (JTBD)

**Origin**: Clayton Christensen (Harvard), Tony Ulwick (Outcome-Driven Innovation), Bob Moesta (Switch Framework)

**Definition**: Customers don't 'buy' products; they 'hire' products to get a specific 'Job' done. The milkshake case — the Job of customers buying in the morning is not 'a tasty drink' but 'something to make the boring commute bearable while staying full until lunch.'

**3 Dimensions of JTBD**:
- **Functional Job**: The practical task to be accomplished (e.g., sharing files with team members)
- **Emotional Job**: The feeling one wants to experience (e.g., wanting to appear professional and organized)
- **Social Job**: How one wants to be perceived by others (e.g., being seen as tech-savvy)

**Switch Framework (Bob Moesta)** — 4 Forces of Switching:
- **Push**: Dissatisfaction with the current situation
- **Pull**: Attraction of the new solution
- **Anxiety**: Fear about switching
- **Habit**: Inertia of the current state
- Switching condition: **Push + Pull > Anxiety + Habit**

**Learning Point**: In JTBD interviews, instead of asking "Why did you buy that product?", ask "Describe the situation when you decided to buy that product." Context is key.

---

### 3. TAM / SAM / SOM

**Definition**:
- **TAM (Total Addressable Market)**: Theoretical total market size — if all potential customers were to purchase
- **SAM (Serviceable Addressable Market)**: Actual serviceable market considering geographic, technological, and regulatory constraints
- **SOM (Serviceable Obtainable Market)**: Realistically obtainable short-term market share considering the competitive environment and resources

**Calculation Methods**:
- **Top-Down**: Start from industry reports and segment down (e.g., Global SaaS $200B → HR SaaS $30B → SMB HR $5B)
- **Bottom-Up**: Individual customer willingness to pay × number of potential customers. More trusted by investors
- **Value Theory**: For new markets, work backward from the magnitude of value provided

**Learning Point**: VCs prefer TAM of $1B+. However, early-stage startups should focus on SOM. Secure a dominant position in a small market, then expand to adjacent markets (Peter Thiel's advice).

---

### 4. Crossing the Chasm

**Origin**: Geoffrey Moore (1991) — Technology Adoption Lifecycle

**5 Stages of Technology Adoption**:
- **Innovators (2.5%)**: Enthusiastic about technology itself. Will use incomplete products
- **Early Adopters (13.5%)**: Strategic buyers with a vision. Adopt for competitive advantage
- **Early Majority (34%)**: Pragmatists. Demand proven solutions and references
- **Late Majority (34%)**: Conservative. Only adopt after it becomes an industry standard
- **Laggards (16%)**: Resist change. Accept only when unavoidable

**The Chasm**: The massive gap between Early Adopters and Early Majority. Early Adopters buy the vision, but the Early Majority wants a 'Whole Product.'

**Strategy for Crossing the Chasm — Bowling Alley**: Deliver a complete solution in one niche market (Beachhead) → achieve overwhelming market share → expand to adjacent segments. Like bowling pins, reference customers create the next customers.

**Learning Point**: The Whole Product strategy is key to crossing the chasm. Deliver the core product + supplementary services + integrations + training + consulting to a single segment.

---

## Part 2: Growth & Metrics

### 5. AARRR (Pirate Metrics)

**Origin**: Dave McClure (500 Startups, 2007)

**5-Stage Funnel**:
- **Acquisition**: Where do users come from? Traffic by channel, CAC, channel efficiency
- **Activation**: Is the first experience satisfactory? Aha Moment reach rate (Facebook: 'adding 7 friends within 10 days')
- **Retention**: Do they come back? DAU/MAU ratio, weekly/monthly retention rate
- **Revenue**: How do you make money? ARPU, conversion rate, LTV
- **Referral**: Do they bring others? Viral coefficient (K-factor), referral conversion rate

**Practical Principle**: Do not optimize all stages simultaneously. Focus on the biggest bottleneck. Generally, **Retention is the first problem to solve**. Investing in acquisition without retention = pouring water into a bottomless bucket.

**Learning Point**: The Activation → Retention transition is key. Defining the Aha Moment with data is the first task.

---

### 6. North Star Metric

**Definition**: The core value a product delivers to customers expressed as a single number. When this metric grows, both customer value and business value should increase simultaneously.

**Notable Examples**:
- Airbnb: Nights Booked
- Spotify: Total Listening Time
- Slack: Number of teams sending 2,000+ messages per week
- Facebook: DAU
- Uber: Weekly Completed Rides

**Input Metrics**: The NSM cannot be directly manipulated → decompose into sub-metrics (Input Metrics). Airbnb's 'Nights Booked' = search conversion rate + host approval rate + rebooking rate, etc. Each team focuses on its own Input Metric.

**Learning Point**: Revenue is not a good NSM. Revenue is a result of customer value, not the cause. The NSM should reflect the actual value customers receive.

---

### 7. Retention Curve and Cohort Analysis

**Retention Curve**: The percentage of registered users remaining over time. A healthy product shows a curve that flattens at a certain level — the strongest quantitative evidence of PMF. Converging to 0 = no PMF.

**Cohort Analysis**: Grouping users acquired during the same period and tracking their behavior. Reveals trends invisible in overall averages.
- **Time-based Cohort**: Comparing retention of January vs February signups → measuring the effect of product improvements
- **Behavior-based Cohort**: Onboarding completers vs non-completers → discovering the Aha Moment
- **Channel-based Cohort**: Differences in user quality by acquisition channel

**Learning Point**: Track D1, D7, D30 retention as a baseline. B2B SaaS focuses on monthly, consumer apps on daily/weekly. Improving retention by cohort = evidence the product is evolving in the right direction.

---

### 8. LTV / CAC

**Definition**:
- **LTV**: Total revenue from a single customer over the entire relationship. LTV = ARPU × Customer Lifespan (1/Churn Rate) × Margin Rate
- **CAC**: Total cost to acquire a single customer. (Marketing + Sales Costs) ÷ New Customers

**Key Ratios**:
- **LTV:CAC = 3:1 or above** is healthy. 5:1+ means room for growth investment. 1:1 or below means losses with every acquisition
- **CAC Payback Period**: Ideally under 12 months for SaaS. Over 18 months is a warning sign

**Improvement Strategies**:
- Increasing LTV: Price increases, upsell/cross-sell, retention improvement, Net Revenue Retention
- Reducing CAC: Organic channels, referral programs, content marketing, self-serve model

**Learning Point**: Always include margin rate when calculating LTV. LTV based on profit, not revenue, is the true measure of business health. Calculate LTV by cohort to avoid the trap of averages.

---

## Part 3: Strategy & Decision Making

### 9. First Principles Thinking

**Origin**: Aristotle → Elon Musk popularized it in modern business

**Definition**: A way of thinking that strips away all existing assumptions, conventions, and analogies, and reasons from the most fundamental and self-evident truths (First Principles).

**Application Method — Socratic Questioning**:
1. Identify assumptions: "What are we taking for granted?"
2. Decompose assumptions: "Is that assumption actually true?"
3. Derive fundamental truths: "What are the basic facts that can no longer be broken down?"
4. Reconstruct: "What new solutions can we build from these basic facts?"

**Example**: Musk discovered that rocket raw material costs were only 2% of the rocket price → the beginning of SpaceX's cost innovation.

**Learning Point**: Applying it to every problem is inefficient. Use it selectively for core problems that require innovation. For routine decisions, analogy and experience-based judgment are more efficient.

---

### 10. Flywheel Effect

**Origin**: Jim Collins ('Good to Great', 2001), Jeff Bezos executed it most successfully

**Definition**: A self-reinforcing virtuous cycle structure where each component strengthens the next. It takes great effort at first, but once momentum builds, it becomes a competitive advantage that is hard to stop.

**Amazon's Flywheel**: Lower prices → more customers → more sellers → wider selection → better experience → more traffic → economies of scale → lower costs → lower prices (repeat)

**Design Principles**:
- Clear starting point (initial energy input point)
- Causal relationships at each stage (A→B→C→A reinforcement)
- Friction removal (eliminating obstacles to the cycle)

**Learning Point**: If you cannot explain your business logic in a single diagram, your strategy is not clear enough. The entire team must share the same Flywheel.

---

### 11. Moat (Economic Moat)

**Origin**: Warren Buffett, Hamilton Helmer ('7 Powers'), Michael Porter ('Competitive Strategy')

**7 Types (Hamilton Helmer's 7 Powers)**:
1. **Economies of Scale**: Scale ↑ → unit cost ↓ (Walmart, AWS)
2. **Network Effects**: Users ↑ → product value ↑ (Facebook, LinkedIn)
3. **Counter Positioning**: If incumbents follow suit, they cannibalize themselves (Netflix vs Blockbuster)
4. **Switching Costs**: High cost to switch to another product (Salesforce, SAP)
5. **Branding**: Higher price/preference even for identical products (Apple, Tiffany)
6. **Cornered Resource**: Patents, proprietary data, key talent (Google search data)
7. **Process Power**: Organizational capabilities built over long periods (Toyota Production System)

**Learning Point**: It is difficult to have a strong Moat from Day 1. In the early stages, focus on Counter Positioning or Network Effects, and layer Moats as you grow.

---

### 12. Second-order Thinking

**Origin**: Howard Marks (Oaktree Capital)

**Definition**:
- First-order thinking: "What is the result of this action?"
- Second-order thinking: "What is the result of that result?"

**Example**: First-order — "Lowering prices will attract more customers" / Second-order — "Low-price customers may have higher churn rates, reducing LTV. Competitors may also cut prices → intensified price competition. The brand may get a 'cheap' image → difficulty entering the premium market"

**Application Method**:
- Extend the time horizon: 10 minutes later, 10 months later, 10 years later
- Extend stakeholders: Customers, competitors, regulators, partners
- Opposite scenario: "If we succeed, what new problems will arise?"

**Learning Point**: Applying it to every decision leads to Analysis Paralysis. Apply selectively to decisions that have high impact and are difficult to reverse.

---

### 13. Reversible vs Irreversible Decisions (One-way / Two-way Door)

**Origin**: Jeff Bezos (Amazon shareholder letter)

**Definition**:
- **Type 1 (One-way Door)**: Irreversible decisions. Require careful analysis (e.g., changing core tech stack, large-scale acquisitions)
- **Type 2 (Two-way Door)**: Easily reversible decisions. Fast execution > perfect analysis (e.g., UI changes, pricing tests)

**Organizational Application**: Many organizations treat all decisions as Type 1 → slower decision-making, diminished experimentation culture. Type 2 should be delegated to individuals/small groups with good judgment.

**Learning Point**: The first question in any decision should always be "Is this a One-way door or a Two-way door?" Most decisions are Type 2.

---

## Part 4: Product Development & Execution

### 14. MVP (Minimum Viable Product)

**Origin**: Frank Robinson (2001), Eric Ries ('The Lean Startup', 2011)

**Definition**: A product version built with minimum effort to validate a core hypothesis. The key is not 'minimum' but **'learning'**.

**Types of MVP**:
- **Landing Page MVP**: Testing demand with a landing page without a product (Buffer)
- **Concierge MVP**: Providing manual service without automation (Food on the Table)
- **Wizard of Oz MVP**: Automated front-end + humans handling the back-end (early Zappos)
- **Piecemeal MVP**: Combining existing tools (Groupon = WordPress + email)
- **Single Feature MVP**: Implementing only one core feature

**Common Mistake**: MVP ≠ 'a low-quality product.' It is 'a product with fewer features,' not 'a product with low quality.' One core feature done perfectly > multiple features done poorly.

**Learning Point**: Before launching an MVP, clearly define "What do we want to learn from this MVP?" and "What result would validate/invalidate the hypothesis?"

---

### 15. Build-Measure-Learn Loop

**Origin**: Eric Ries ('The Lean Startup')

**3-Stage Cycle**:
- **Build**: Create the minimum needed to test the hypothesis
- **Measure**: Collect actual user behavior data (qualitative + quantitative)
- **Learn**: Analyze data → validate/invalidate hypothesis → Pivot or Persevere

**Key — Plan in Reverse Order**: Learn (What do we want to learn?) → Measure (What do we need to measure?) → Build (What do we need to build?)

**Validated Learning**: "40% of users willing to pay $29/month ranked the team collaboration feature as their #1 priority" — This level of specific learning is required.

**Learning Point**: Cycle speed is key. Monthly vs weekly = 4x difference in learning. Design 'good enough experiments' rather than 'perfect experiments.'

---

### 16. ICE / RICE Scoring

**ICE Model**:
- **I**mpact: 1-10
- **C**onfidence: 1-10
- **E**ase: 1-10
- ICE Score = I × C × E

**RICE Model (Intercom)**:
- **R**each: Number of users affected (specific number)
- **I**mpact: Impact per individual user (3=Massive, 2=High, 1=Medium, 0.5=Low, 0.25=Minimal)
- **C**onfidence: Estimation reliability (100%=High, 80%=Medium, 50%=Low)
- **E**ffort: In person-month units
- RICE Score = (R × I × C) ÷ E

**Practical Tip**: Scoring is not absolute truth but a starting point for structured discussion. Discussing why scores differ is more valuable than the scores themselves.

**Learning Point**: Do not give high scores to items with low Confidence. For uncertain ideas, first run small experiments to increase Confidence, then reassess.

---

### 17. Opportunity Solution Tree

**Origin**: Teresa Torres ('Continuous Discovery Habits', 2021)

**4-Layer Structure**:
- **Outcome**: Measurable customer behavior change connected to business results (e.g., first-week retention from 40%→55%)
- **Opportunity**: Unmet customer needs, pain points (e.g., confusion when creating the first project)
- **Solution**: Ideas to address the opportunity (e.g., interactive onboarding, template gallery)
- **Experiment**: Minimum test to validate the solution

**Key — Continuous Discovery**: Customer touchpoints at least once a week. Weekly interviews with 2-3 people are more effective than quarterly large-scale research.

**Learning Point**: The most important layer is the Opportunity layer. Beware of "Solution-first" thinking where you come up with a solution first and then fit an opportunity to it. Always start from the customer's actual problem.

---

## Part 5: User & Behavior

### 18. Hook Model

**Origin**: Nir Eyal ('Hooked', 2014)

**4-Stage Cycle**:
- **Trigger**: External (notifications, emails) → Internal (emotions, situations). The goal is to form internal triggers (boredom → Instagram)
- **Action**: The simplest action taken in anticipation of a reward. BJ Fogg: B=MAT (Behavior = Motivation × Ability × Trigger)
- **Variable Reward**: Unpredictable rewards trigger dopamine
  - Tribe: Social rewards (likes, comments)
  - Hunt: Resource rewards (new information, content discovery)
  - Self: Intrinsic rewards (sense of achievement, mastery)
- **Investment**: Input of time, data, effort → increases value of the next cycle (follows, profile, content creation)

**Ethical Consideration**: If the product genuinely improves users' lives and the makers themselves use it, it is ethical (Manipulation Matrix).

**Learning Point**: The transition from External Trigger → Internal Trigger is key. True habit is formed when the product comes to mind 'naturally in a specific emotion/situation,' not 'because of a notification.'

---

### 19. Network Effects

**Definition**: As the number of users increases → the value each user receives also increases.

**Types**:
- **Direct Network Effects**: Same type of users ↑ → value ↑ (WhatsApp, Zoom)
- **Indirect (Cross-side) Network Effects**: One side ↑ → other side's value ↑ (Uber: riders ↑ → driver value ↑)
- **Data Network Effects**: Users ↑ → data ↑ → product improvement (Google Search, Waze)
- **Platform Network Effects**: Ecosystem growth → value ↑ (iOS App Store, Salesforce)

**Cold Start Problem**: Low value initially due to lack of users (chicken and egg). Solution: 'Atomic Network' — first build the minimum network that creates value on its own (Uber → San Francisco, Facebook → Harvard).

**Learning Point**: Do not confuse network effects with economies of scale. True network effects create value from interactions between users. Simply having more customers reducing costs = economies of scale.

---

### 20. Switching Cost

**Definition**: All costs incurred when switching from the current product to another. Switching costs ↑ → customer retention ↑ → stronger Moat.

**5 Types**:
- **Financial Cost**: Cancellation fees, new subscription costs, migration costs
- **Time/Effort Cost**: Learning a new product, data migration, workflow reconfiguration
- **Data Cost**: Loss of accumulated data, history, customizations (years of notes in Notion)
- **Social Cost**: Retraining team members, loss of network/connections (LinkedIn connections)
- **Psychological Cost**: Loss of familiarity, anxiety about change, sunk cost psychology

**Strategic Application**: Healthy switching costs form naturally as product value deepens (data accumulation, customization). Artificial lock-in (blocking data export) increases short-term retention ↑ but decreases long-term customer trust ↓.

**Learning Point**: When analyzing competitors, map switching costs across all 5 types. Identify which types are strong/weak → entry point for competitive strategy.

---

## Part 6: Organization & Culture

### 21. OKR (Objectives & Key Results)

**Origin**: Andy Grove (Intel) → John Doerr → Google. Reference: 'Measure What Matters' (2018)

**Structure**:
- **Objective**: Qualitative, inspirational direction (e.g., Dramatically improve the onboarding experience)
- **Key Results**: Quantitative, measurable performance indicators (e.g., Onboarding completion rate from 60%→85%, first-week retention from 40%→55%)

**Operating Principles**:
- 3-5 Objectives per quarter, 3-5 KRs per Objective
- 60-70% achievement rate is ideal (100% = goals were too easy)
- Separate from performance reviews (linking to compensation → conservative goal-setting)
- Transparent sharing (company-wide alignment)

**Learning Point**: Writing KRs as a 'to-do list' leads to failure. "Write 10 blog posts" = Task, "Increase organic traffic by 50%" = KR. KRs are outcomes, Tasks are outputs.

---

### 22. Two-Pizza Team

**Origin**: Jeff Bezos (Amazon)

**Definition**: The optimal team size that can be fed with two pizzas (6-10 people). The key is not team size but **autonomy and ownership**.

**Communication Cost**: n people → n(n-1)/2 paths. 5 people = 10, 10 people = 45, 20 people = 190.

**Practical Principles**:
- **Single-threaded Ownership**: Each team has full responsibility for one mission and metric
- **API-based Collaboration**: Interactions between teams occur through clear interfaces
- **Autonomous Decision-making**: Decide and execute within the mission scope without external approval

**Learning Point**: To be effective, technical infrastructure enabling independent deployment/experimentation is necessary. Organizational structure and technical architecture must be designed together (Conway's Law).

---

### 23. Disagree and Commit

**Origin**: Andy Grove (Intel) → Jeff Bezos (Amazon Leadership Principles)

**Definition**: When opinions differ, discuss thoroughly, but once a decision is made, execute with full commitment regardless of agreement. Consensus ≠ Disagree and Commit.

**Why It Matters**:
- **Speed**: Waiting for consensus → decision-making delays. In startups, speed = survival
- **Execution Quality**: Full commitment even after disagreeing → higher success probability. Passive resistance is the most dangerous
- **Psychological Safety**: A culture where dissenting opinions can be freely expressed is a prerequisite

**Bezos**: "I disagree with this project, but I acknowledge the possibility that you may be right. I will support it with full commitment."

**Learning Point**: It applies in both directions. It includes leaders committing to team members' opinions. It must apply regardless of rank to be a true culture.

---

## Key Concepts by Agent

| Concept | A (Strategist) | B (Analyst) | C (Director) |
|------|:-:|:-:|:-:|
| PMF | ★★★ | ★★★ | ★★★ |
| JTBD | ★★★ | ★★ | ★★ |
| TAM/SAM/SOM | ★★★ | ★★★ | ★★ |
| Crossing the Chasm | ★★★ | ★★ | ★★★ |
| AARRR | ★★ | ★★★ | ★★★ |
| North Star Metric | ★★ | ★★★ | ★★★ |
| Retention/Cohort | ★ | ★★★ | ★★ |
| LTV/CAC | ★★ | ★★★ | ★★★ |
| First Principles | ★★★ | ★★ | ★★ |
| Flywheel | ★★★ | ★ | ★★★ |
| Moat | ★★★ | ★★★ | ★★★ |
| Second-order Thinking | ★★ | ★★ | ★★★ |
| One-way/Two-way Door | ★ | ★ | ★★★ |
| MVP | ★★★ | ★★ | ★★ |
| Build-Measure-Learn | ★★ | ★★★ | ★★ |
| ICE/RICE | ★ | ★★ | ★★★ |
| Opportunity Solution Tree | ★★ | ★★ | ★★★ |
| Hook Model | ★★★ | ★ | ★★ |
| Network Effects | ★★★ | ★★★ | ★★ |
| Switching Cost | ★★ | ★★★ | ★★ |
| OKR | ★ | ★★ | ★★★ |
| Two-Pizza Team | ★ | ★ | ★★★ |
| Disagree and Commit | ★★ | ★★ | ★★★ |

★★★ = Core / ★★ = Important / ★ = Reference
