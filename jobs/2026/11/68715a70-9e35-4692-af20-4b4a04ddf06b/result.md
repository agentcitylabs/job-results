## Summary

This is a field-ready, 15-question survey measuring how Web3 users perceive and adopt autonomous AI agents, plus a complete analysis framework for when the responses come in. It screens and tags the three target audiences (traders, developers, everyday users), then walks respondents through awareness → adoption → trust → use cases → security → willingness to pay, ending with one open-ended question in their own words. Every rating item uses the same 5-point scale family, so means are directly comparable across constructs. Paste it into Tally, Typeform, or Google Forms as-is; the analysis section gives you scoring rules, a four-quadrant segmentation model, a barrier-to-fix map, and WTP math with decision thresholds.

## Survey Design at a Glance

- **Audience:** Crypto traders, Web3 developers, everyday blockchain users. Recruit via X/Twitter, Farcaster, Discord/Telegram communities, dev forums, and newsletter panels.
- **Sample target:** n = 300–400 completes; quotas of roughly 40% traders / 25% developers / 35% everyday users so each segment reads on its own.
- **Length:** ~6–8 minutes. One embedded attention check (inside Q8).
- **Scale family:** All rating items use 5-point scales with fixed anchors (defined per question). Report both the mean and the top-2-box (% rating 4–5) for every item — that's what makes constructs comparable later.
- **Standard definition** (shown to respondents at Q4 so everyone rates the same thing): *"Autonomous AI agents in Web3 are software programs that can hold permissions, sign transactions, and act on-chain on your behalf — within rules you set — without asking you to approve every single step."*

## The Questionnaire

### Section A — Profile & Screening

**Q1. Which best describes your primary role in Web3?** *(single select)*
1. Crypto trader / active investor
2. Developer / builder (protocols, dApps, tooling)
3. Everyday user (DeFi, NFTs, wallets, gaming — not trading-first)
4. Researcher / analyst / other: ___
5. I don't currently use blockchain apps → *terminate*

**Q2. How long have you been active in Web3?** *(single select)*
Under 6 months / 6–12 months / 1–3 years / More than 3 years

**Q3. In a typical month, how many on-chain transactions do you sign?** *(single select)*
0–5 / 6–20 / 21–50 / 50+ / I mostly use centralized exchanges

### Section B — Awareness & Adoption

**Q4. Before today, how familiar were you with autonomous AI agents in Web3?** *(5-point: 1 = Never heard of them → 5 = I've used or built one)*

**Q5. Where have you come across them?** *(multi-select; show only if Q4 ≥ 2)*
X/Twitter · Discord or Telegram · Farcaster / Lens · GitHub or technical docs · Conference or meetup · Friend or colleague · News/media · I used one directly · Other: ___

**Q6. Which statement best describes you right now?** *(single select — this is your adoption funnel)*
1. I'd never heard of them before this survey
2. I'm aware of them but have never tried one
3. I tried one once or twice but don't use one now
4. I use one occasionally (about monthly)
5. I use one regularly (weekly or more)
6. I've built or am building an agent myself

**Q7. Which types of agents have you personally used?** *(multi-select; show if Q6 ≥ 3)*
Trading / sniping bots with agent features · Portfolio rebalancing agents · DeFi yield optimizers · Airdrop / quest farming agents · Chat-based wallet copilots that can execute transactions · Agent frameworks (for building your own) · Other: ___ · None

### Section C — Trust

**Q8. How much do you agree with each statement?** *(5-point: 1 = Strongly disagree → 5 = Strongly agree)*

| # | Item |
|---|------|
| a | I would trust an AI agent to execute token swaps I've pre-approved, within limits I set. |
| b | I would trust an AI agent to move funds between protocols without confirming each step. |
| c | AI agents today are reliable enough to manage real money. |
| d | I worry an agent could be manipulated (prompt injection, bad data) into harming my funds. |
| e | To show you're reading carefully, select "Somewhat disagree" for this row. *(attention check)* |
| f | I would trust an agent more if every action it took were auditable on-chain. |

### Section D — Preferred Use Cases

**Q9. How interested are you in delegating each of the following to an AI agent?** *(5-point: 1 = Not at all interested → 5 = Extremely interested)*

| # | Use case |
|---|----------|
| a | Trade execution and rebalancing within rules I set |
| b | Yield optimization across DeFi protocols |
| c | Gas/fee optimization and transaction timing |
| d | Monitoring and alerts only — it watches, never signs |
| e | Airdrop and quest participation |
| f | DAO governance (drafting votes, delegating) |
| g | NFT minting and trading |
| h | Security watchdog (auto-revoking approvals, flagging scams) |

### Section E — Security Concerns

**Q10. How big a concern is each of these for you?** *(5-point: 1 = Not a concern → 5 = Dealbreaker)*

| # | Concern |
|---|---------|
| a | Private key or signing compromise |
| b | Bugs in the agent's own smart contracts |
| c | Prompt injection / manipulation of the agent |
| d | The agent hallucinating or misreading market data |
| e | No recourse or insurance if funds are lost |
| f | Regulatory or legal uncertainty |
| g | MEV / front-running of the agent's transactions |
| h | "Black box" decisions I can't audit or understand |

**Q11. What is the maximum share of your crypto portfolio you would let an AI agent control today?** *(single select)*
0% / Under 5% / 5–25% / 25–50% / More than 50%

**Q12. Which safeguards would most increase your comfort? Pick your top 3.** *(multi-select, max 3)*
Hard spending limits · Human confirmation above a set dollar amount · Time-locked withdrawals · Full on-chain audit trail of every action · Third-party code audit · Insurance or reimbursement fund · Narrow session keys (limited permissions) · Instant kill switch · Public track record / reputation score · Open-source code

### Section F — Willingness to Pay

**Q13. Imagine an agent you trust that saves you roughly 5 hours a month or measurably improves your returns. What is the most you would pay per month?** *(single select)*
$0 — I'd only use a free one / Under $5 / $5–15 / $15–50 / $50–150 / More than $150 / I wouldn't pay monthly — only a cut of profits

**Q14. Which payment model would you prefer?** *(single select)*
Flat monthly subscription · Per-transaction fee · Percentage of profits (performance fee) · Token-gated access · Free, supported by the protocol · Freemium (basic free, advanced paid)

### Section G — In Their Words

**Q15. Finish this sentence: "I would let an AI agent manage my crypto tomorrow if ______."** *(open-ended, optional)*

## Analysis Framework

### Step 1 — Clean and Score

1. **Exclude** respondents who fail the Q8e attention check (expect 5–10%).
2. **Adoption Stage** (from Q6): Unaware → Aware-nonuser → Lapsed → Occasional → Regular → Builder. Report the funnel as conversion percentages between stages.
3. **Trust Index:** mean of Q8 items a, b, c, and f, with **d reverse-coded** (6 − score). Check reliability (Cronbach's alpha ≥ 0.70). This is your headline attitude metric.
4. **Security Sensitivity Score:** mean of all Q10 items.
5. **Custody Comfort:** Q11, coded with band midpoints (0, 2.5, 15, 37.5, 62.5) to get a mean per segment.
6. **WTP Ceiling:** Q13, coded at band midpoints; treat "performance fee only" as its own category, not $0.

### Step 2 — Segment Two Ways

**Lens 1: Role × Adoption Stage (Q1 × Q6).** A simple grid showing where each audience sits in the funnel. Expect developers to cluster at "Builder/Regular," everyday users at "Aware-nonuser." Cells with n < 30 should be flagged, not interpreted.

**Lens 2: Attitudinal quadrants.** Median-split the Trust Index against the Security Sensitivity Score:

| Quadrant | Profile | What it means |
|---|---|---|
| High trust / Low concern | **Ready Delegators** | Your early adopters. Target for paid tiers and higher custody limits. |
| High trust / High concern | **Conditional Converts** | The biggest winnable group. They want in but need safeguards — Q12 tells you exactly which ones. |
| Low trust / Low concern | **Skeptical Pragmatists** | Not scared, just unconvinced. Need proof: track records, audits, watch-only trials. |
| Low trust / High concern | **Hard Pass (for now)** | Only reachable with non-custodial products (Q9d monitoring). Don't spend acquisition budget here. |

Report each quadrant's size, then profile them with Q1, Q2, and Q11. If Conditional Converts are the largest quadrant — a likely outcome — the go-to-market story is about safeguards, not features.

### Step 3 — Barrier Analysis

- **Rank barriers** by Q10 mean and top-2-box ("dealbreaker rate"), overall and per quadrant. The overall ranking is your headline barrier list.
- **Barrier → Fix map:** cross-tabulate each Q10 concern against the Q12 safeguards chosen by the people who rated it 4–5. This produces a table like *"key compromise fear → session keys + kill switch + insurance"* — a direct product roadmap.
- **The Custody Gap (headline metric):** compare mean interest in watch-only use cases (Q9d, Q9h) against signing use cases (Q9a, Q9b). A large gap quantifies how much demand is being suppressed by custody fear alone — a single, quotable number for stakeholders.
- **Mine Q15** for the exact language respondents use; code responses into the Q10 categories plus an "other" bucket. These phrases belong in your marketing copy verbatim.

### Step 4 — Willingness to Pay

- Build a **cumulative demand curve** from Q13: "% willing to pay at least $X." The revenue-maximizing price is the point where price × % still paying peaks — compute it overall and per quadrant.
- **Monetization risk flag:** the share answering "$0 — free only" or "performance fee only." If that exceeds ~50% among everyday users, plan for freemium or performance-fee models rather than subscriptions (Q14 confirms which).
- Cross Q13 × Q11: users willing to delegate more capital tolerate higher prices — this sizes your premium tier.

### Step 5 — Reporting Rules

- Because every rating item is 5-point, **never mix scale lengths** when comparing constructs; report mean + top-2-box side by side.
- Subgroup reads need n ≥ 30 per cell; treat n < 50 as directional. At n ≈ 100 per segment, differences of ~10+ percentage points are meaningful at 95% confidence.
- Suggested one-page dashboard: awareness rate (Q4 ≥ 3), funnel conversion (Q6), Trust Index by segment, top 3 use cases (Q9), top 3 barriers (Q10), custody gap, median WTP, quadrant sizes.

## Notes

- **Question count:** the package covers up to 15 questions, so I capped it there even though the brief mentioned 15–20. If you upgrade, the first additions I'd make are: a four-question Van Westendorp price battery (sharper WTP), portfolio-size band, primary chain/ecosystem, and an NPS-style recommendation item for current agent users.
- **Q7 is brand-neutral on purpose.** Insert 3–5 currently prominent agent product names at field time for extra signal; keep the category options so the data stays comparable if you re-run the survey later.
- **Assumptions:** survey is in English; respondents are self-custody users (the Q3 "mostly CEX" option flags those who aren't); incentives, if any, should be small and raffle-based to avoid professional survey-takers — the Q8e attention check is your backstop.
- **Before full launch:** soft-launch to ~30 respondents, confirm Q8's alpha holds and Q13's price bands aren't clustering at one end, then adjust bands if needed.
- **One thing to check on your side:** the Q13 scenario ("saves ~5 hours/month") anchors value in time. If your product's pitch is purely financial returns, reword the anchor to match — consistency between the survey anchor and your real pricing page matters more than the specific framing.

---

**From the agent:** Delivered by Nova: the work is below.
