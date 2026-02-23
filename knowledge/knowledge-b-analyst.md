# Knowledge Base — Agent B (Product Validation Analyst)

## 1. Validation Frameworks & Methodologies

### Lean Startup Methodology
- **Build → Measure → Learn Cycle**: Build quickly, measure, and learn
- **MVP (Minimum Viable Product)**: The minimum product that can validate your core hypothesis
  - **Wizard of Oz MVP**: Backend is manual, but the frontend appears automated
  - **Concierge MVP**: Provide the service directly to a small number of customers
  - **Landing Page MVP**: Test demand with a landing page without an actual product
  - **Key Insight**: An MVP is not "the minimum set of features" but rather "the minimum experiment to validate your core hypothesis"

### Hypothesis-Driven Development
- **Hypothesis Structure**: "We believe that [target customer] will achieve [expected outcome] when they use [our solution] in [specific situation]"
- **Validation Criteria**: "If this hypothesis is correct, [measurable metric] should be at or above [specific threshold]"
- **Kill Criteria**: Set criteria in advance such as "if the metric falls below this threshold, we abandon this idea"

### Customer Development
Steve Blank's 4 Stages:
1. **Customer Discovery**: Verify that the problem is real
2. **Customer Validation**: Confirm willingness to pay for the solution
3. **Customer Creation**: Test acquisition channels
4. **Company Building**: Scale

**Interview Best Practices (The Mom Test)**:
- Bad question: "Would you use this app?" (social desirability bias)
- Good question: "How did you solve this problem last week?" (based on past behavior)
- Best question: "How much are you spending on that solution?" (confirms willingness to pay)

---

## 2. Competitive Analysis Best Practices

### How to Build a Competitive Analysis Matrix
| Evaluation Criteria | Us | Competitor A | Competitor B | Competitor C |
|-----------|------|----------|----------|----------|
| Core Feature 1 | | | | |
| Core Feature 2 | | | | |
| Pricing | | | | |
| UX/UI | | | | |
| Target Segment | | | | |
| Strengths | | | | |
| Weaknesses | | | | |

### Competitor Type Classification
1. **Direct Competitors**: Solve the same problem in the same way
2. **Indirect Competitors**: Solve the same problem in a different way
3. **Potential Competitors**: Large companies not yet in this market but capable of entering
4. **Substitutes**: Imperfect solutions customers currently use (spreadsheets, manual processes, etc.)

### Competitive Advantage (Moat) Assessment
- **Network Effects**: Value increases as more users join (strong)
- **Switching Costs**: Difficulty of switching to another product (moderate)
- **Data Advantage**: Product improves as usage data accumulates (strong)
- **Brand**: Becoming the category synonym (e.g., "Googling") (strong but hard to build)
- **Economies of Scale**: More users → lower unit cost (moderate)
- **Regulation/Licensing**: Legal barriers to entry (strong but limited)

---

## 3. Financial Analysis & Unit Economics

### Key Financial Metrics
- **CAC (Customer Acquisition Cost)**: Cost to acquire one customer
  - Calculation: Marketing/Sales costs ÷ Number of new customers
  - Benchmark: Average B2B SaaS CAC = 6-12x monthly subscription fee

- **LTV (Lifetime Value)**: Lifetime value of one customer
  - Calculation: ARPU × Average customer lifespan (1/Churn Rate)
  - **LTV/CAC Ratio**: 3:1 or above is healthy, 1:1 is dangerous

- **ARPU (Average Revenue Per User)**: Average revenue per user
- **MRR (Monthly Recurring Revenue)**: Monthly recurring revenue
- **Churn Rate**: Attrition rate (above 5% monthly is a warning sign)
- **Payback Period**: Time to recover CAC (target within 12 months)

### Monetization Validation Checklist
- [ ] Are customers currently spending money to solve this problem?
- [ ] Is our expected price within the customer's willingness-to-pay range?
- [ ] Do the unit economics improve at scale?
- [ ] Is the CAC payback period reasonable? (within 12 months)
- [ ] Can we achieve an LTV/CAC ratio of 3:1 or above?

---

## 4. Data Tracking Design Best Practices

### Event Tracking Design Principles

**Event Design Based on AARRR (Pirate Metrics)**:
1. **Acquisition**: Where did they come from?
   - `page_view`, `signup_started`, `signup_completed`
   - Required properties: utm_source, utm_medium, utm_campaign, referrer

2. **Activation**: Did they experience the core value?
   - `onboarding_completed`, `first_[core_feature]_used`, `aha_moment_reached`
   - **Defining the Aha Moment is key**: "Which behaviors correlate with long-term retention?"

3. **Retention**: Do they come back?
   - `session_started`, `[core_feature]_used`, `day_n_return`
   - **Retention Curve Analysis**: Day 1, Day 7, Day 30 retention

4. **Revenue**: Do they pay?
   - `subscription_started`, `payment_completed`, `plan_upgraded`
   - Required properties: plan_type, amount, currency

5. **Referral**: Do they bring others?
   - `invite_sent`, `invite_accepted`, `share_clicked`

### KPI Definition Framework

**North Star Metric (NSM) Selection Criteria**:
- Does it represent the core value of the product?
- Is it measurable?
- Can the entire team understand it?
- Does improving this metric lead to business growth?

| Product Type | North Star Metric Examples |
|-----------|----------------------|
| SaaS B2B | Weekly Active Users (WAU), Features Used per Week |
| Marketplace | Transactions per Month, GMV |
| Social/Community | Daily Active Users (DAU), Content Created per Day |
| Subscription | Monthly Retention Rate, MRR Growth |

**Dashboard Design Principles**:
- **5-Second Rule**: You should be able to determine "is the status good or bad" within 5 seconds of looking at the dashboard
- **Hierarchy**: Executive (3-5 key metrics) → Operational (10-15) → Deep-dive (as needed)
- **Include Comparisons**: Week-over-week, month-over-month, vs. targets
- **Alert Settings**: Automatic alerts for anomalies (e.g., retention suddenly drops more than 10%)

---

## 5. Data Analysis Best Practices

### Cohort Analysis
- **Time-Based Cohorts**: Group by signup week/month → compare retention trends
- **Behavior-Based Cohorts**: Group by specific behavior (e.g., onboarding completed vs. not completed)
- **Key Insight**: Is the retention gap between cohorts narrowing? (confirms product improvement effectiveness)

### Funnel Analysis
- Measure conversion rate at each stage → identify the biggest drop-off point
- **Key Insight**: Improving the bottom of the funnel (just before conversion) yields higher ROI than improving the top (awareness)

### A/B Test Design
- **Minimum Sample Size Calculation**: Based on MDE (Minimum Detectable Effect)
- **Statistical Significance**: p-value < 0.05 (95% confidence interval)
- **Duration**: At least 1-2 weeks (to eliminate weekly cycle effects)
- **Key Insight**: Test only one thing at a time. If you change two things simultaneously, you cannot determine which one caused the effect.

### Segment Analysis
- **Power User Definition**: Behavior patterns of the top 10% of users → guide all users toward these patterns
- **Churn Prediction**: Declining activity patterns → intervene before churn (email, notifications, incentives)
- **Value-Based Segments**: Distinguish high-value/low-value users → focus resources accordingly

### Analysis Report Writing Principles
1. **So What?**: For every data point, add "so what should we do about it?"
2. **Actionable**: An insight that does not lead to action is not an insight
3. **Context**: Present numbers alongside comparisons (week-over-week, month-over-month, benchmarks), not in isolation
4. **Honest**: Be candid about bad news, not just good news

---

## 6. Feasibility Assessment Best Practices

### Technical Feasibility Checklist
- [ ] Does the core technology already exist? (libraries, APIs, etc.)
- [ ] Can a prototype be built within 2-4 weeks?
- [ ] Are there known technical limitations for scaling?
- [ ] What are the data/infrastructure dependencies?
- [ ] Can regulatory technical requirements (e.g., security, privacy) be met?

### Market Entry Feasibility Checklist
- [ ] How will you acquire the first 10 users? (specific plan)
- [ ] Is customer acquisition possible without an initial marketing budget?
- [ ] How long is the sales cycle?
- [ ] Is there a strategy for acquiring reference customers?

### Risk Matrix
| | Low Impact | Medium Impact | High Impact |
|--|---------|---------|---------|
| **High Probability** | Monitor | Develop mitigation plan | Immediate response required |
| **Medium Probability** | Acceptable | Monitor | Develop mitigation plan |
| **Low Probability** | Accept | Acceptable | Monitor |
