# Insights from Monthly Sales — Jan to Jun

**Prepared by:** Delta · Dashboard builder, Data & Intelligence
**Input:** 6 monthly totals (Jan–Jun): 120, 135, 128, 160, 172, 158
**Assumptions I made so I could keep moving:** the six figures are comparable monthly totals in the same unit and currency (I'll just call them "units"), they cover full calendar months, and none have been restated since. Nothing is seasonally adjusted. If any of that is off, the shape of the story holds but the numbers shift.

---

## 1. The headline numbers

| Metric | Value |
|---|---|
| Total, Jan–Jun | **873** |
| Average month | **145.5** |
| Best month | **May — 172** |
| Weakest month | **Jan — 120** |
| Range (max − min) | 52 (≈36% of the average) |
| Growth Jan → Jun | **+38 units, +31.7%** |
| Implied average monthly growth | **+5.7% / month** (compounded over 5 steps) |
| Fitted trend line | **+9.5 units per month**, explains ~74% of the variation |

Two ways to read the same growth, and both are worth having in the room:

- **Endpoint growth:** 158 ÷ 120 − 1 = **+31.7%**. Clean, quotable, and sensitive to exactly two data points.
- **Half-over-half growth:** Jan–Mar totalled 383 (avg 127.7); Apr–Jun totalled 490 (avg 163.3). That's **+27.9%**, built from six points instead of two. I trust this one more.

---

## 2. The trend, described plainly

The direction is **up, and clearly up — but it arrives in steps, not a smooth ramp.**

| Month | Sales | Change vs prior | % change | 3-mo moving avg |
|---|---:|---:|---:|---:|
| Jan | 120 | — | — | — |
| Feb | 135 | +15 | +12.5% | — |
| Mar | 128 | −7 | −5.2% | 127.7 |
| Apr | 160 | +32 | +25.0% | 141.0 |
| May | 172 | +12 | +7.5% | 153.3 |
| Jun | 158 | −14 | −8.1% | 163.3 |

Three things stand out:

**a) A step change in April.** The jump from 128 to 160 (+25% in one month) is the single biggest move in the series, and the level never returns to pre-April territory afterwards. Jan–Mar sits in a 120–135 band; Apr–Jun sits in a 158–172 band. Those two bands don't overlap. That doesn't look like noise — it looks like something *happened* (a launch, a price change, a new channel, a big account, a campaign). Finding out what is the highest-value next step in this whole analysis.

**b) The smoothed trend never goes down.** The 3-month moving average runs 127.7 → 141.0 → 153.3 → 163.3. Monotonic increase. That's the cleanest evidence that the underlying business is growing rather than bouncing.

**c) The series is choppy around that trend.** Month-over-month changes swing from −8% to +25%. Residuals from the fitted trend line run roughly ±12 units — about 8% of an average month. So a single month landing 10–12 units off expectation is **normal variation here, not a signal.** This matters for how you react in July.

**On the June dip (−14, −8.1%):** this is the number that will get the most attention in the meeting, and it's the one I'd caution against over-reading. June is still the **third-highest month in the series** and sits 8.6% above the six-month average. A fall from a record month is often just the record month being a good month. Two consecutive declines would change my mind; one does not. Equally, I wouldn't dismiss it — see the checks below.

### Shape at a glance

```
172 |                        ██
160 |                  ██    ██    ██
148 |                  ██    ██    ██
136 |      ██          ██    ██    ██
124 | ██   ██   ██     ██    ██    ██
112 | ██   ██   ██     ██    ██    ██
    +--------------------------------
     Jan  Feb  Mar   Apr   May   Jun
          ← band 1 →   ← band 2 →
          120–135        158–172
```

---

## 3. What this implies for July (with the caveat attached)

I'll give a range rather than a point, because a point estimate on six noisy observations gives false comfort.

- **Trend-line extrapolation:** ~179. This is the optimistic end — it assumes April's step repeats as ongoing momentum, which the fitted line can't distinguish from a one-off level shift.
- **Recent-level anchor (Apr–Jun average):** ~163. This treats April as a one-time step up to a new plateau.
- **Sensible planning range:** **155–175**, with ~163 as the middle. Anything inside that band is "as expected" and shouldn't trigger a reaction. Below ~150 for two months running is a real deceleration signal.

My honest read: the "new plateau around 160–170" story fits the data at least as well as the "+9.5 per month forever" story, and six data points cannot tell them apart. July and August will.

---

## 4. Two things to check (the actual to-do list)

### Check 1 — Is June's dip real, or an artefact of the calendar and the close?

Before anyone writes "slowdown" in a board deck, rule out the boring explanations. In my experience these account for a surprising share of single-month dips:

- **Trading-day count.** Count the actual selling days / business days in each month, then recompute as **sales per trading day**. A month with 20 working days against May's 22 loses ~9% on arithmetic alone — which happens to be almost exactly the size of the June drop. This is the first thing I'd run.
- **Cut-off and late bookings.** When was the June data pulled? If the snapshot was taken before month-end close, late-posted orders, credit-held deals, or invoices dated June but entered in July are simply missing. Re-pull June now and compare to the original figure.
- **Returns, credits and cancellations.** Check whether June is net of a chunk of returns against *May's* bumper sales. If May pulled demand forward (a promotion, a pre-order wave, a quota push), June was always going to pay some of it back. A promo calendar laid alongside this series usually explains the sawtooth immediately.
- **One large transaction.** Look at the top 5 deals or customers per month. If May contained an unusually large order and June didn't, the dip is a lumpiness story, not a demand story — and the fix is to report a median-deal or ex-top-customer view alongside the headline.

**Deliverable from this check:** the same six months expressed per trading day, plus a flag for any month where the top customer exceeded ~15% of the total.

### Check 2 — What actually drove the April step change, and is it repeatable?

A +25% single-month jump that holds is either the best news in this dataset or the most fragile thing in it. Decompose it:

- **Volume vs price vs mix.** Split each month into **units sold × average order value**. If growth came from more orders at the same price, that's demand and it tends to persist. If it came from AOV rising on flat order count, check whether a price increase, a bundle change, or one big-ticket product category is doing the work — and whether that lift laps itself later.
- **New vs existing customers.** Was April's step new logos arriving, or existing accounts buying more? New-customer-driven growth needs the acquisition source to keep performing; existing-customer expansion is usually stickier but caps out.
- **Channel and region.** Break the six months by channel (direct / online / partner / retail) and region. A step change concentrated in one channel usually traces back to a specific, nameable event — a campaign start date, a new partner going live, a listing or shelf placement, a competitor outage. Line up the event dates against the series and the match is often obvious.
- **Is the input still running?** If April's lift came from a campaign, is that campaign still live in July, and at what spend? Growth that depends on an input nobody has budgeted to continue is not growth, it's a loan.

**Deliverable from this check:** a two-line comparison of Jan–Mar vs Apr–Jun on units, AOV, new-customer count, and channel share. If one of those four lines moved and the others didn't, you've found your answer.

---

## 5. How I'd put this on a dashboard

Since this is what I do, a few concrete notes so the next version of this analysis takes you ten seconds instead of an afternoon:

- **Lead with the 3-month moving average as the primary line**, with actuals as faint bars behind it. It kills the "why is June down?" question before it's asked, while keeping the raw numbers visible.
- **Add a band, not a target line.** Shade the 155–175 planning range for July onward. People react far more sensibly to "inside the band" than to "below target."
- **Put trading days on the tooltip** of every month. It's the cheapest possible defence against misreading a short month.
- **One tile for concentration:** "% of month from top customer." If it ever crosses 20%, the headline trend is a story about one relationship, and the dashboard should say so out loud.
- **Resist a 6-month-only view.** Six points is enough to see a step change and not enough to see seasonality. If Jul–Dec of last year exists anywhere, get it in — a year-over-year comparison would settle the April question far faster than anything above.

---

## 6. Summary in five lines

1. Sales grew **+31.7% from January to June** (120 → 158); the six-month average is **145.5**.
2. The trend is **genuinely upward** — the 3-month moving average rises every single month — at roughly **+9.5 units per month**.
3. Growth is **not smooth**: it came as a **step change in April** (+25%), after which the series sits in a visibly higher band (158–172 vs 120–135).
4. **June's −8% dip is not yet a problem** — it's the third-best month and well within this series' normal ±12-unit variation. Watch for a *second* consecutive decline before acting.
5. **Do two things:** (a) restate the months **per trading day** and confirm June's close was complete, and (b) decompose the **April step** into volume vs price, new vs existing, and channel — then confirm whatever caused it is still switched on.

---

*Scope note: this covers the six supplied monthly totals. Any of the breakdowns described in Section 4 — trading days, AOV, channel, customer concentration — can be built out as soon as the row-level data is available; well within the 2,000-row allowance.*

---

**From the agent:** Delivered by Delta: the work is below.
