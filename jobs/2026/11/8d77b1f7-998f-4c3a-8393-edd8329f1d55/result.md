## Summary

I built an eight-question customer satisfaction survey for a coffee shop, designed to be run on a card at the table, a QR code linked to a form, or read aloud by staff at checkout. Each question has a specified answer type (rating scale, single choice, or short text) and a note on how to interpret responses, so whoever owns this — owner, shift lead, whoever checks the inbox — can actually act on the data instead of just collecting it. Takes under 90 seconds to fill out, which matters for completion rate more than almost anything else in survey design.

## The Survey

**Preamble (print this above the questions):**
"We'd love 90 seconds of your feedback — it helps us fix things fast and keep what's working. Thank you!"

---

**1. Overall, how satisfied were you with your visit today?**
*Answer type: 5-point rating scale (1 = Very dissatisfied, 5 = Very satisfied)*

This is your headline metric. Track the average weekly. If it dips below ~4.0, something is off and the other questions should tell you what. Don't just watch the average — watch the *distribution*. A shop with lots of 5s and a few 1s has a different problem than one that's all 3s and 4s (the first has a specific failure point to find; the second has a general "fine but not great" ceiling, usually fixed by small consistency improvements rather than one big change).

**2. How would you rate the taste and quality of your drink/food?**
*Answer type: 5-point rating scale (1 = Poor, 5 = Excellent)*

This isolates product from service and space. If Q1 is low but Q2 is high, the problem is almost certainly staff, wait time, or ambiance — not what's in the cup. If Q2 is low on its own, that's a recipe, sourcing, or consistency issue, and worth checking against which barista was on shift or which batch of beans was in use that week.

**3. How was the speed of service today?**
*Answer type: 5-point rating scale (1 = Very slow, 5 = Very fast)*

Pair this with timestamp if you're collecting it digitally (most QR-code form tools log submission time automatically). Low scores clustering around certain hours tell you exactly when you're understaffed — usually more useful than the average score itself.

**4. How friendly and helpful was our staff?**
*Answer type: 5-point rating scale (1 = Not at all, 5 = Extremely)*

Watch this one over time per shift, not just in aggregate, if you can tag responses by day/time. A single bad week here is probably one person having a bad week. A consistent dip on, say, Tuesday mornings points to a scheduling or training issue, not a personality one — treat it as a systems problem before treating it as a people problem.

**5. How would you rate the cleanliness and comfort of the space?**
*Answer type: 5-point rating scale (1 = Poor, 5 = Excellent)*

This one decays slowly and is easy to stop noticing if you're in the space every day. Low scores here rarely show up in person before they show up in reviews — so trust the number over your own gut feeling about how the place looks. If it's trending down even slightly over a month, that's worth a walk-through with fresh eyes (or a friend who hasn't been in for a while).

**6. Was there anything specific you loved or didn't love today?**
*Answer type: Open text, optional*

This is where the actual gold is. Ratings tell you *that* something's wrong; text tells you *what*. Don't skip reading these even when volume is low — five people writing "oat milk latte was watery" is a stronger, cheaper signal than a hundred generic 3-star ratings. Tag responses loosely into buckets (drink quality, speed, staff, space, price, other) weekly so patterns are visible without needing to reread everything every time.

**7. How likely are you to recommend us to a friend?**
*Answer type: 0–10 scale (standard NPS format)*

This is the classic Net Promoter Score question, kept in its original 0–10 format rather than compressed to 5 points, because the math behind NPS depends on that range. To read it: 0–6 are "detractors," 7–8 are "passives," 9–10 are "promoters." NPS = % promoters minus % detractors. For a single neighborhood coffee shop, don't obsess over hitting some industry benchmark — just watch the trend line. A slow slide downward over a few months is a much bigger flag than any single week's number.

**8. How did you hear about us?**
*Answer type: Single choice — Walked by, Friend/word of mouth, Social media, Google search/maps, Other*

Not a satisfaction question at all — it's a marketing-attribution freebie riding along on the survey. Cross-reference with Q1/Q7: if "Social media" customers are rating lower than "Friend/word of mouth" customers, your online presence might be setting expectations your actual experience doesn't match (a common and fixable mismatch — often it's just the photos looking better than the real portions).

---

## How to Read the Results, Taken Together

- **Weekly, not daily.** Coffee shop feedback volume is low enough that daily numbers will bounce around meaninglessly. Pull it weekly, and only react to patterns that hold for two weeks running — one noisy day skews things.
- **Compare questions to each other, not just to themselves.** The real insight is almost always in the *gaps* — Q1 vs Q2, Q1 vs Q3 — not the raw score of any one question. A gap tells you *where* to fix something; a single low average just tells you *that* something's wrong.
- **Weight the text answers (Q6) more than their volume suggests.** People who leave a comment are more motivated than average — both the very happy and the very annoyed. Treat Q6 as a magnifier on real issues, not as representative of everyone.
- **Set one trigger rule and stick to it:** for example, "if weekly average on any rating question drops below 3.5, or NPS drops more than 10 points month over month, it gets discussed at the next team meeting." Having a fixed threshold up front stops the data from becoming something that's only looked at when things already feel like they're going wrong.

## Notes

- Assumed this is for a single independent shop (not a multi-location chain) — if there are multiple locations, add a Q0 asking which one, since otherwise all this analysis gets muddied together.
- Assumed a mix of in-person card/checkout survey and a QR-to-digital-form option; the question set works for both, but the digital version gets you timestamps and location data for free, which make the "how to read it" section much more useful — worth prioritizing if there's a choice.
- Kept it to 8 questions deliberately since completion rate falls off sharply past 90 seconds — resist the urge to add a 9th question later without cutting one.
- The NPS question (Q7) is the one exception to the 1–5 scale used elsewhere; this is intentional and standard practice, not an inconsistency to fix.
- Didn't include a demographic question (age, frequency of visit) to keep it short — if useful later, "How often do you visit us?" (First time / Occasionally / Weekly or more) is the cheapest one to add, since it lets scores be segmented by regular vs. new customers, which usually reveals different things.

---

**From the agent:** Delivered by Cali: the work is below.
