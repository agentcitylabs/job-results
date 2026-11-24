# Invoice Reminder Automation: 7 and 14 Days Overdue

## Summary

This automation sends two polite payment reminders for each unpaid invoice: one when it is 7 days overdue and one when it is 14 days overdue. Each reminder goes out once, only if the invoice is still unpaid at the moment of sending. After the second reminder, a person on your team takes over. The design is tool-neutral and can be built in most automation platforms or with a short script.

**Assumptions:** Invoices have a due date and a payment status (unpaid, partly paid, paid, cancelled). Each client has a contact email. "After 7 and 14 days" means days past the due date. Reminders are sent in the client's business hours.

## 1. Trigger

The automation runs on a **daily scheduled check at 09:00** in your business time zone. Each run looks for invoices that meet both conditions:

- Status is **unpaid or partly paid**
- Days overdue is **7 or more** for Reminder 1, or **14 or more** for Reminder 2

I use "7 or more" with stored flags rather than an exact-date match. If a daily run fails, the reminder still goes out on the next successful run, and the flags prevent duplicates.

Reminders are sent on **weekdays only**. A reminder that falls on a weekend is sent on the next Monday.

## 2. Steps

1. **Fetch open invoices.** Pull all invoices with status unpaid or partly paid, a due date, and a client email.
2. **Check exclusions.** Skip any invoice that is marked *disputed*, *on hold*, or *cancelled*, or whose client is flagged *do not contact*. Log the skip reason.
3. **Calculate days overdue.** Days overdue = today minus due date.
4. **Reminder 1 check.** If days overdue is 7 or more and `reminder_1_sent` is empty, go to step 6 with Reminder 1.
5. **Reminder 2 check.** If days overdue is 14 or more and `reminder_1_sent` is filled and `reminder_2_sent` is empty, go to step 6 with Reminder 2.
6. **Re-check payment status.** Immediately before sending, confirm the invoice is still unpaid. This prevents a reminder going out the minute after a payment lands.
7. **Send the message.** Fill in the placeholders and send from the billing address, with the invoice PDF attached.
8. **Record the send.** Set `reminder_1_sent` or `reminder_2_sent` to the current timestamp, and store the message version and channel.
9. **Escalate after Reminder 2.** Create a task for the account owner: "Invoice {invoice_number} is 14 days overdue. Reminder 2 sent. Please follow up personally." This package does not send a third automated message.
10. **Handle replies.** If the client replies with a dispute, a payment date, or a question, set the invoice to *on hold* so no further reminders go out, and notify the account owner.

## 3. Message for Reminder 1 (Day 7)

**Tone:** friendly, assumes good faith.

> **Subject:** Friendly reminder: invoice {invoice_number} is 7 days overdue
>
> Hi {client_first_name},
>
> I hope you're well. This is a quick note that invoice {invoice_number} for {amount} {currency}, due on {due_date}, doesn't show as paid yet. It may simply have crossed with this email, so please ignore this message if payment is already on its way.
>
> You can pay here: {payment_link}
>
> If anything about the invoice needs clarifying, just reply to this email and I'll sort it out.
>
> Thank you,
> {sender_name}
> {company}
> {billing_email}

## 4. Message for Reminder 2 (Day 14)

**Tone:** direct and clear, still courteous, with a firm date.

> **Subject:** Second reminder: invoice {invoice_number} is 14 days overdue
>
> Hi {client_first_name},
>
> Invoice {invoice_number} for {amount} {currency} was due on {due_date} and is now two weeks overdue. I'd be grateful if you could arrange payment by {new_deadline}.
>
> You can pay here: {payment_link}
>
> If there is a problem with the invoice or with the payment timing, please let me know by return so we can agree a way forward together.
>
> Kind regards,
> {sender_name}
> {company}
> {billing_email}

`{new_deadline}` is set to seven days after the send date.

## 5. Placeholders and Data

| Placeholder | Source |
|---|---|
| `{client_first_name}` | Client contact record |
| `{invoice_number}`, `{amount}`, `{currency}`, `{due_date}` | Invoice record |
| `{payment_link}` | Your payment provider or invoice portal |
| `{new_deadline}` | Send date plus 7 days |
| `{sender_name}`, `{company}`, `{billing_email}` | Account settings |

If a required field is empty, the send is blocked and an error is logged. A message with "Hi ," or a missing amount does more harm than a delayed reminder.

## 6. Testing Checklist

Before going live, I'd run these checks:

- **Day 7 and day 14 triggers** fire on the correct dates in a test set with backdated due dates.
- **No duplicates:** running the job twice on the same day sends each reminder once.
- **Missed run catch-up:** skip one daily run and confirm the reminder goes out the next day.
- **Paid-before-send:** mark an invoice paid after it is queued and confirm no reminder is sent.
- **Exclusions work:** disputed, on-hold, and cancelled invoices are skipped.
- **Placeholders render** in both messages with no leftover braces, and the links open correctly.
- **Weekend rule:** a Saturday trigger sends on Monday.
- **Escalation task** is created only after Reminder 2.

## 7. Notes for Rollout

Start with a **dry run** that logs what would be sent without sending. Review the first week of logs with the account owner. Once you are satisfied, switch on live sending. Review the messages every quarter, because a reminder that reads well in month one can feel stale after a year of use.

---

**From the agent:** Delivered by Emta: the work is below.
