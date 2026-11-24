## Summary

This workflow sends two automatic reminders for unpaid invoices: one 7 days after the due date and another 14 days after the due date.  
It checks the invoice status before each message, so paid invoices are excluded.  
Use the trigger and steps below in your invoicing or automation platform, replacing the bracketed fields with invoice data.

## Automation Workflow

### Trigger

**Event:** An invoice reaches its due date and remains unpaid.

**Schedule:** Run the workflow daily, checking all open or overdue invoices.

For each invoice, calculate the number of days since the due date:

- If the invoice is **7 days overdue**, send Reminder 1.
- If the invoice is **14 days overdue**, send Reminder 2.
- If the invoice has been paid, stop the workflow and send nothing.

### Step 1: Find overdue invoices

Retrieve invoices that meet all of these conditions:

- Status is `Unpaid`, `Open`, or `Overdue`.
- Due date was exactly 7 or 14 days ago.
- The invoice has not already received the relevant reminder.
- The client has a valid email address.

Store or use these invoice details:

- Client name
- Client email
- Invoice number
- Invoice amount
- Original due date
- Payment link
- Business name and contact details

### Step 2: Check the current payment status

Before sending either reminder, check the invoice again in the billing system.

- If status is `Paid`, mark the reminder as skipped and end the workflow.
- If status is still unpaid, continue.
- If status is `Partially paid`, use the same reminder but show the remaining balance, if available.
- If the invoice is disputed, cancelled, or under review, stop the workflow and flag it for manual handling.

### Step 3: Send the 7-day reminder

**Timing:** 7 calendar days after the invoice due date.

**Subject:** Reminder: Invoice [Invoice Number] is overdue

**Message:**

> Hi [Client Name],  
>
> This is a friendly reminder that invoice **[Invoice Number]** for **[Currency][Amount Due]** was due on **[Due Date]** and is still showing as unpaid.  
>
> You can make the payment here: [Payment Link]  
>
> If you have already paid, please disregard this message or send us the payment details so we can update our records. If there is a problem with the invoice, reply to this email and we will be happy to help.  
>
> Thank you,  
> [Business Name]  
> [Contact Details]

After sending, record:

- Reminder type: `7-day reminder`
- Date and time sent
- Delivery status
- Invoice number

### Step 4: Send the 14-day reminder

**Timing:** 14 calendar days after the invoice due date.

Before sending, repeat the payment-status check from Step 2.

**Subject:** Second reminder: Invoice [Invoice Number] is [14] days overdue

**Message:**

> Hi [Client Name],  
>
> We are following up again regarding invoice **[Invoice Number]** for **[Currency][Amount Due]**, which was due on **[Due Date]** and remains unpaid.  
>
> Please arrange payment here: [Payment Link]  
>
> If payment has already been made, please reply with the payment date or reference so we can check and update our records. If you need to discuss the invoice or payment timing, contact us at [Contact Details].  
>
> Thank you for your prompt attention.  
>
> Best,  
> [Business Name]  
> [Contact Details]

After sending, record:

- Reminder type: `14-day reminder`
- Date and time sent
- Delivery status
- Invoice number

## Recommended Workflow Logic

```text
Every day:
  Find invoices due 7 or 14 days ago
  For each invoice:
    Check payment status

    If paid, cancelled, or disputed:
      Stop

    If 7 days overdue and 7-day reminder not sent:
      Send 7-day reminder
      Record reminder

    If 14 days overdue and 14-day reminder not sent:
      Send 14-day reminder
      Record reminder
```

## Notes

- This design assumes reminders are scheduled from the **invoice due date**, not the invoice creation date.
- “7 days” and “14 days” mean calendar days. Set the sending time and timezone for the business.
- The automation should use a stored reminder flag or message log to prevent duplicate emails if it runs more than once.
- The client should confirm the payment statuses, field names, payment-link format, and preferred sender address in the invoicing platform before activating the workflow.
- No reminder is included after 14 days; invoices still unpaid after that point should be reviewed under the business’s normal collections process.

---

**From the agent:** Delivered by Alve: the work is below.
