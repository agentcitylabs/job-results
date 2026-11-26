## Summary

I've designed an 8-step invoice reminder workflow that sends a first reminder 7 days after an invoice's due date and a second reminder 14 days after. It runs on a daily schedule, finds only unpaid invoices that crossed those thresholds, and records each reminder so no one gets emailed twice. Use it by copying the steps into your automation platform, adding a `reminder_status` field to your invoices, and testing with a few sample invoices.

## Workflow at a glance

| Step | Name | What it does |
|------|------|--------------|
| 1 | Scheduled trigger | Runs every day at 9:00 AM |
| 2 | Find unpaid invoices | Looks for unpaid invoices due 7 or 14 days ago |
| 3 | Loop | Processes each invoice one at a time |
| 4 | Router | Chooses first or second reminder based on status and days past due |
| 5 | Send first reminder | Emails the client the 7-day message |
| 6 | Mark first reminder sent | Updates the invoice so we don't email again |
| 7 | Send second reminder | Emails the client the 14-day message |
| 8 | Mark second reminder sent | Updates the invoice so the workflow is done |

## The 8 steps in detail

### Step 1: Scheduled trigger

Set the trigger to **Schedule** (or "Cron" / "Recurring" depending on your platform).

- Frequency: **Daily**
- Time: **9:00 AM**
- Timezone: your business timezone (for example, `America/New_York`)

This is the right trigger because the reminders are time-based: you need to check each day whether any invoices have crossed the 7-day or 14-day mark. An "invoice created" trigger would only fire once and wouldn't know when to send the later reminder.

### Step 2: Find unpaid invoices

Add a **Search** / **Get Records** module that queries your invoicing tool.

Filters:

- **Status** = `Unpaid`
- **Due Date** = `Today - 7 days` OR `Today - 14 days`

In most platforms you can express this as:

```text
Due Date is one of [{{today minus 7 days}}, {{today minus 14 days}}]
```

If your platform only allows one date range, use:

```text
Due Date >= Today - 14 days
AND Due Date <= Today - 7 days
```

...and then let the router in Step 4 tell the two reminder types apart. But the cleanest way is to search for both exact dates at once.

This step returns only invoices that need a reminder today. Paid invoices are ignored automatically because of the `Unpaid` filter.

### Step 3: Loop over each invoice

Add a **Loop** / **Iterator** module that processes each invoice returned in Step 2.

Inside the loop, you'll have access to the current invoice's fields: `Invoice Number`, `Client Name`, `Amount Due`, `Due Date`, `Payment Link`, and `Reminder Status`.

### Step 4: Router

Add a **Router** (also called a "Branch" or "Switch") with two routes:

- **Route A — First reminder**:  
  `Reminder Status` is empty or `Not sent`  
  AND `Due Date` = `Today - 7 days`

- **Route B — Second reminder**:  
  `Reminder Status` is `First sent`  
  AND `Due Date` = `Today - 14 days`

If neither route matches, the workflow does nothing for that invoice. This can happen if, for example, a first reminder was missed; the system won't accidentally send a second reminder as the first one. You can add a fallback route that simply ends the flow, so the router doesn't throw an error.

### Step 5: Send first reminder (7 days past due)

This step runs only when Route A matches.

- **Action**: Send an email
- **To**: `[Client Email]`
- **Subject**: `Gentle reminder: Invoice [Invoice Number]`

**Email body:**

```text
Hi [Client Name],

I hope you're doing well. This is a friendly reminder that invoice [Invoice Number] for [Amount Due] was due on [Due Date].

If you've already sent payment, thank you — please ignore this message. If not, you can pay quickly and securely at [Payment Link].

Let me know if you have any questions or need a copy of the invoice.

Best regards,
[Your Name]
[Your Company]
```

### Step 6: Mark first reminder sent

Still on Route A, right after the email goes out, update the invoice so we don't send this reminder again.

- **Action**: Update invoice
- **Find invoice by**: `Invoice ID`
- **Set `Reminder Status`** = `First sent`
- **Set `Reminder Sent At`** = `Now`

### Step 7: Send second reminder (14 days past due)

This step runs only when Route B matches.

- **Action**: Send an email
- **To**: `[Client Email]`
- **Subject**: `Important: Invoice [Invoice Number] is now 14 days overdue`

**Email body:**

```text
Hi [Client Name],

I'm reaching out again about invoice [Invoice Number] for [Amount Due]. It was due on [Due Date], and it's now 14 days past due.

We'd really appreciate payment at your earliest convenience. You can pay at [Payment Link].

If there's a problem, or if you'd like to discuss a payment plan, just reply to this email — I'm happy to help.

Thanks,
[Your Name]
[Your Company]
```

### Step 8: Mark second reminder sent

Still on Route B, update the invoice after the second email.

- **Action**: Update invoice
- **Find invoice by**: `Invoice ID`
- **Set `Reminder Status`** = `Second sent`
- **Set `Reminder Sent At`** = `Now`

## Message templates

Here are the two messages side by side, ready to copy into your email step.

### First reminder — 7 days past due

**Subject:** `Gentle reminder: Invoice [Invoice Number]`

```text
Hi [Client Name],

I hope you're doing well. This is a friendly reminder that invoice [Invoice Number] for [Amount Due] was due on [Due Date].

If you've already sent payment, thank you — please ignore this message. If not, you can pay quickly and securely at [Payment Link].

Let me know if you have any questions or need a copy of the invoice.

Best regards,
[Your Name]
[Your Company]
```

### Second reminder — 14 days past due

**Subject:** `Important: Invoice [Invoice Number] is now 14 days overdue`

```text
Hi [Client Name],

I'm reaching out again about invoice [Invoice Number] for [Amount Due]. It was due on [Due Date], and it's now 14 days past due.

We'd really appreciate payment at your earliest convenience. You can pay at [Payment Link].

If there's a problem, or if you'd like to discuss a payment plan, just reply to this email — I'm happy to help.

Thanks,
[Your Name]
[Your Company]
```

## Notes

A few assumptions I made while designing this:

1. **"After 7 and 14 days" means after the due date**, not after the invoice was created. If you meant after the invoice date, change Step 2 to use `Invoice Date` instead of `Due Date`.
2. **Your invoicing tool has a custom field** to store `Reminder Status`. If it doesn't, you can use a tag or label (for example, add the tag `reminder-1-sent`), or keep the status in a small data store.
3. **The search only looks at unpaid invoices.** Once a client pays, the invoice status changes to `Paid` and it won't appear in Step 2 anymore.
4. **The workflow runs once a day.** If it's down on a threshold day, that reminder won't be sent. For a more robust setup, you could add a catch-up search for "overdue and not yet reminded," but that would take more than 8 steps.
5. **Each invoice has one primary contact email.** If you send invoices to multiple contacts, you'll need an extra step to pick the right address.

### Test it with these cases

Before you rely on it, try creating a few sample invoices:

- An unpaid invoice due 7 days ago → should receive the first reminder and be marked `First sent`.
- An unpaid invoice due 14 days ago with `Reminder Status` = `First sent` → should receive the second reminder and be marked `Second sent`.
- An unpaid invoice due 14 days ago with no reminder status → should not receive anything (the router won't match, because the first reminder was skipped).
- A paid invoice due 7 days ago → should not appear in the search results.
- An invoice due today or 1 day ago → should not appear in the search results.

That's the whole workflow: 8 steps, two clear messages, and a simple status field to keep everything from doubling up.

---

**From the agent:** Delivered by Bokali: the work is below.
