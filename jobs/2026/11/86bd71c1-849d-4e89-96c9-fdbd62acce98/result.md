# Automation: Unpaid Invoice Reminders

**Assumption:** Reminders are sent 7 and 14 days after an invoice’s due date. If “after 7 and 14 days” means after the invoice is issued instead, change the timing accordingly.

## Trigger

Run once daily. Find invoices that are **overdue and unpaid**.

## Steps

1. Check each invoice’s status and due date.
2. If it is unpaid and 7 days overdue, send the first reminder—provided it hasn’t already been sent.
3. If it is still unpaid and 14 days overdue, send the second reminder—provided it hasn’t already been sent.
4. Stop reminders as soon as the invoice is paid, voided, or disputed. Record each reminder’s sent date to prevent duplicates.

## Reminder at 7 days overdue

**Subject:** Reminder: Invoice [Invoice number] is past due

Hi [Client name],

A quick reminder that invoice **[Invoice number]** for **[Amount]** was due on **[Due date]** and is still showing as unpaid.

You can view and pay it here: [Payment link]

If you’ve already sent payment, please disregard this message. Let us know if you have any questions.

Thanks,  
[Your name / Company]

## Reminder at 14 days overdue

**Subject:** Second reminder: Invoice [Invoice number] is past due

Hi [Client name],

Our records show that invoice **[Invoice number]** for **[Amount]** is still unpaid, 14 days after its due date of **[Due date]**.

Please arrange payment here: [Payment link]. If you’ve already paid, or need to discuss the invoice, reply to this email and we’ll be happy to help.

Thanks,  
[Your name / Company]

---

**From the agent:** Delivered by Ulro: the work is below.
