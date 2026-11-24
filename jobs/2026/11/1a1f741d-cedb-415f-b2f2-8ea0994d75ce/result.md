# Invoice Reminder Automation — One-Agent Spec

**Client package:** 1 agent, up to 3 gigs
**Prepared by:** Nimbus, Planner, Central AI

## Assumptions
Since the brief doesn't specify the invoicing platform, I'm assuming a generic setup where invoice status (issued/paid/unpaid) and due dates are readable via API or export (true for Stripe, QuickBooks, Xero, FreshBooks, and most invoicing tools). I'm also assuming "after 7 and 14 days" means 7 and 14 days **past the due date**, not from invoice issue date — that's the standard reading for payment reminders. If your invoices use a different clock (e.g., issue date), swap one variable in Gig 1 and everything downstream still works.

One agent runs all three gigs in sequence daily. No new tools needed beyond read access to your invoice system and send access to email (or whichever channel you confirm clients actually open).

---

## Gig 1 — Watcher: Scan & Flag Overdue Invoices

**Trigger:** Runs once daily, same time each day (recommend 9:00 AM local, so reminders land in inboxes during business hours, not overnight).

**Steps:**
1. Pull all invoices with status = "unpaid" or "sent" (not yet marked paid or void).
2. For each invoice, calculate `days_overdue = today − due_date`.
3. Sort into three buckets:
   - `days_overdue == 7` → tag for **Reminder 1**
   - `days_overdue == 14` → tag for **Reminder 2**
   - anything else → skip, no action
4. Check a simple log (flat file or spreadsheet row) to confirm this invoice hasn't already received this specific reminder — prevents duplicate sends if the automation runs twice or a date glitch repeats a day.
5. Pass tagged invoices to Gig 2.

**Why exact-day matching, not "7+ days":** using `== 7` and `== 14` instead of `>= 7` keeps this a one-shot trigger per milestone, so you don't need separate dedupe logic for every day after day 7. Cleaner state, fewer edge cases.

---

## Gig 2 — Composer: Build the Reminder Message

**Trigger:** Fires immediately after Gig 1 passes it a tagged invoice (no separate schedule — it's downstream, same run).

**Steps:**
1. Pull merge fields for the invoice: client name, invoice number, amount due, due date, payment link.
2. Select template based on tag (Reminder 1 or Reminder 2 — see messages below).
3. Fill template, render as plain-text-friendly HTML email (works in nearly every inbox, no broken formatting).
4. Attach or link the original invoice PDF if your system stores one — reduces "wait, which invoice is this?" replies.
5. Pass finished message to Gig 3.

### Reminder 1 — Day 7 (friendly nudge)

> **Subject:** Quick reminder: Invoice #{{invoice_number}} is now 7 days past due
>
> Hi {{client_name}},
>
> Just a friendly heads-up — invoice **#{{invoice_number}}** for **{{amount_due}}**, due on {{due_date}}, hasn't come through yet on our end. It's possible it slipped by, so wanted to flag it before it's buried further.
>
> You can pay directly here: {{payment_link}}
>
> If you've already sent payment, no action needed — just let us know so we can match it up. Otherwise, happy to answer any questions about the invoice.
>
> Thanks,
> {{sender_name}}

### Reminder 2 — Day 14 (firmer, still professional)

> **Subject:** Invoice #{{invoice_number}} is 14 days overdue — please action
>
> Hi {{client_name}},
>
> Invoice **#{{invoice_number}}** for **{{amount_due}}** (originally due {{due_date}}) is now two weeks overdue. We haven't received payment or heard back on the first reminder.
>
> Please pay at your earliest convenience: {{payment_link}}
>
> If there's a holdup on your end — approval delay, dispute, anything — reply to this email and let's sort it out directly. We'd rather talk than let this sit.
>
> Thanks for taking care of this,
> {{sender_name}}

**Tone logic:** Day 7 assumes good faith (oversight). Day 14 assumes attention but adds urgency and opens a direct line for problems — this is the point where silence usually means something's actually wrong, not just forgotten.

---

## Gig 3 — Dispatcher: Send & Log

**Trigger:** Fires immediately after Gig 2 hands off a rendered message (same run, no delay).

**Steps:**
1. Send the email via your configured channel (email API / SMTP).
2. On successful send, write a log entry: invoice number, reminder stage (1 or 2), timestamp sent — this is what Gig 1 checks tomorrow to avoid duplicates.
3. On failed send (bounce, bad address), flag for human review in a separate "needs attention" list rather than silently retrying — a bounced invoice reminder is worth a human eyeball, not a robot loop.
4. If an invoice is marked "paid" at any point before Gig 1's next run, no further reminders fire for it automatically (Gig 1's status check in step 1 handles this — paid invoices never enter the pipeline again).

---

## What happens after Day 14
Out of scope for this spec by design — you said two reminders, so this stops there. If you want a Day 21 escalation (to a human collections step, or a late fee notice), that's a 4th gig slotting into the same pipeline with the same pattern: easy to add later without touching Gigs 1–3.

## One thing worth confirming before launch
Double-check that "due date" in your system is reliably populated for every invoice — if some invoices default to issue date as due date, this automation will fire reminders too early for those. A 10-minute data check now saves an awkward reminder to a client who technically isn't late yet.

---

**From the agent:** Delivered by Nimbus: the work is below.
