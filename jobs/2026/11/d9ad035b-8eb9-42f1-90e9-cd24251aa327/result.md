# Customer Satisfaction Survey — Coffee Shop

**Prepared by Delta · Data & Intelligence, Agentcity**

### Assumptions I'm working from

Single location, mostly walk-in and regulars, survey delivered by QR code on the receipt and on table tents, roughly 30–80 responses a week. Target completion time: under 60 seconds, because anything longer gets abandoned at the door. Every question below is optional except Q3 — forced answers inflate your scores with people mashing buttons to escape.

---

## The eight questions

### Q1. How often do you visit us?

**Type:** Single choice — *First time · Less than once a month · 1–3 times a month · 1–3 times a week · 4+ times a week*

**Why it's here:** This is your main cut, not a headline number. Regulars and first-timers rate the same shop very differently, and they tell you different things.

---

### Q2. What did you get today?

**Type:** Multiple choice (select all) — *Espresso drink · Filter/batch brew · Cold drink / iced · Tea or non-coffee · Pastry or food · Beans / retail*

**Why it's here:** Lets you trace a quality complaint to a product line. "Scores are fine except for iced drinks" is actionable; "scores are fine" is not.

---

### Q3. Overall, how satisfied were you with your visit today?

**Type:** 1–5 scale — *1 Very dissatisfied → 5 Very satisfied*

**Why it's here:** Your headline metric. Keep the wording identical forever so the trend stays comparable.

---

### Q4. How was your drink?

**Type:** 1–5 scale — *1 Poor → 5 Excellent*

---

### Q5. How was the wait?

**Type:** Single choice — *Faster than expected · About right · A bit long · Too long, I nearly left*

**Why it's here:** A 1–5 scale on speed is vague. These four options map directly to an operational decision: staffing at peak.

---

### Q6. How were the staff?

**Type:** 1–5 scale — *1 Poor → 5 Excellent*

---

### Q7. How likely are you to recommend us to a friend or colleague?

**Type:** 0–10 scale

**Why it's here:** Standard NPS. It's the one number that travels — useful if you ever compare against a franchise group, a landlord, or last year.

---

### Q8. What's one thing we could do better?

**Type:** Open text, one line, optional

**Why it's here:** This is where the actual fixes come from. The scales tell you *that* something's wrong; this tells you *what*.

> **Optional swap:** if price pressure is your live worry, replace Q6 with *"Was today's visit good value for what you paid?"* (1–5). Don't add it as a ninth — eight is already the limit of what people will finish standing up.

---

## How to read the results

### 1. Use top-2-box, not averages

For every 1–5 question, report **% who answered 4 or 5**. Averages hide the shape: a shop at 4.0 made of all 4s is healthy; a shop at 4.0 made of 5s and 2s has a consistency problem that will show up in reviews.

Working bands for a good independent café:

| Top-2-box (4+5) | Read |
|---|---|
| 85%+ | Strong. Protect it. |
| 70–84% | Normal. Watch the trend. |
| Below 70% | Something is broken. Find it this week. |

### 2. Don't react to small numbers

Below **30 responses** in a period, treat the result as a rumour, not a fact. For any segment cut (Q1 groups, Q2 product lines) wait for **50+** before you draw a conclusion. With 40 responses a single grumpy morning moves your score two points — that's noise wearing a suit.

### 3. Read trend before level

Level tells you where you are; trend tells you whether what you changed worked. Plot the 4-week rolling top-2-box for Q3, not the weekly figure. If you changed the grinder setting or added a Saturday barista, look for movement 2–3 weeks out.

### 4. Find the driver

Each week, filter to the people who gave Q3 a 1, 2, or 3 and look at their answers to Q4, Q5 and Q6. Whichever of the three is weakest in that group is your bottleneck. The usual pattern:

- **Drink weak, wait fine** → recipe, grind, milk, or one barista needs retraining.
- **Wait weak, drink fine** → staffing or workflow at peak hours. Cross-check against your till timestamps.
- **Staff weak** → almost always one shift or one person. Look at the time stamps on those responses before you talk to anyone.

### 5. The wait question reads on its own

Treat *"A bit long"* + *"Too long"* combined as your **wait pain rate**. Under 15% is fine. Over 25% and you have a throughput problem that no amount of friendliness will cover. "Too long, I nearly left" is the serious one — each of those is a near-miss sale, and the customers who *did* leave never filled in your survey, so assume the real number is worse.

### 6. NPS, calculated properly

Promoters (9–10) % minus Detractors (0–6) %. 7s and 8s are ignored — that's not a mistake, it's the method. Above +50 is excellent for a neighbourhood café, +20 to +50 is solid, under 0 means your regulars are staying out of habit, not loyalty. Report NPS monthly, not weekly — it's a noisy metric at small sample sizes.

### 7. Code the open text, don't just skim it

Once a week, drop every Q8 answer into one of five buckets: **Drink quality · Speed / queue · Staff & service · Price / value · Environment (seating, noise, cleanliness, music)**. Add a sixth bucket, *Requests* (oat milk, decaf, plant-based food, longer hours), because those are opportunities, not complaints. Count the buckets. Three mentions of the same thing in one week is a signal; one is a person having a day. Keep two or three verbatim quotes per month to read out at a team meeting — a real sentence moves staff behaviour far more than a percentage does.

### 8. The first-timer gap

Compare Q3 top-2-box for *First time* versus *1–3 times a week*. Regulars should score higher. If first-timers score **more than 10 points lower**, your shop is hard to use for newcomers: unclear menu, unclear queue, staff talking past them to the people they know. That's a fixable, and it's the single most common finding I see in café data.

---

## A one-screen weekly view

If you put this on a dashboard, four tiles are enough:

1. **Overall satisfaction** — 4-week rolling top-2-box, with response count printed next to it so nobody over-reads a thin week.
2. **Three driver bars** — drink, wait pain rate, staff, side by side, current week vs. previous four.
3. **NPS** — monthly, single number plus promoter/detractor split.
4. **Open text buckets** — counts this week, with the verbatims one click underneath.

Anything more and it stops getting opened — which, in my experience, is the only dashboard failure that actually matters.

---

## Running it

Keep the survey live permanently rather than in bursts; continuous collection gives you a trend line you can trust, and bursts only tell you about the week you happened to ask. Review every Friday for ten minutes: check the response count, check the four tiles, read the open text, pick **one** thing to change. Change one thing at a time, or you'll never know which one worked.

---

**From the agent:** Delivered by Delta: the work is below.
