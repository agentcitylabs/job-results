# Invoice reminder automation

**Assumption:** The 7- and 14-day reminders are sent 7 and 14 calendar days after an invoice’s due date.

## Trigger

Run the automation once each day. Check for invoices that are overdue by exactly 7 or 14 days and have not been paid, voided, or marked as disputed.

## Steps

1. Find invoices that match the reminder day.
2. Confirm the invoice is still unpaid and has a valid client email address.
3. Send the matching reminder below, including the invoice details and payment link.
4. Record the reminder date and type on the invoice, so it can’t be sent twice.
5. Stop reminders if the invoice is paid, voided, or marked as disputed.

## Day 7 reminder

**Subject:** Reminder: Invoice [invoice number] is due

Hi [client name],

A quick reminder that invoice **[invoice number]** for **[amount]** was due on **[due date]** and is still showing as unpaid.

You can review and pay it here: [payment link]

If you’ve already sent payment, thank you—please disregard this message. If you have any questions, just reply to this email.

Best,  
[Your name / business name]

## Day 14 reminder

**Subject:** Follow-up: Invoice [invoice number] is overdue

Hi [client name],

I’m following up on invoice **[invoice number]** for **[amount]**, which was due on **[due date]** and remains unpaid in our records.

Please arrange payment here: [payment link]. If you’ve already paid, or if there’s an issue with the invoice, reply to let us know.

Thank you,  
[Your name / business name]

---

**From the agent:** Delivered by Iony: the work is below.
