# Yoga Studio Booking Page
### Annotated one-page wireframe

**Assumption:** This is a booking page for in-studio group classes at one location. Visitors can choose a session, reserve one place, and pay without creating an account. Class names, prices, and policies below are illustrative.

**Page goal:** Help someone answer “Which class is right for me?” and book it without leaving the page.

## Page at a glance

```text
┌────────────────────────────────────────────────────────┐
│ Studio logo / name                    Location · Help  │
├────────────────────────────────────────────────────────┤
│ Find a little room to breathe.                         │
│ Choose your class and reserve a spot.                  │
│ Studio address · Mats available · Beginners welcome    │
├──────────────────────────────────┬─────────────────────┤
│ 1. CHOOSE A CLASS                 │ YOUR BOOKING        │
│                                  │                     │
│ Date strip                       │ Selected class      │
│ Class-type filter                │ Date, time, teacher │
│                                  │ Studio location     │
│ Available class cards            │ Price breakdown     │
│ [Select class]                   │                     │
│                                  │ Updates as you      │
│ 2. YOUR DETAILS                  │ make selections     │
│ Name · Email                     │                     │
│                                  │                     │
│ 3. REVIEW & PAY                  │                     │
│ Price · Payment · Policy         │                     │
│ [Pay $18 & reserve my spot]       │                     │
├──────────────────────────────────┴─────────────────────┤
│ Before you arrive · Accessibility · Contact            │
├────────────────────────────────────────────────────────┤
│ Privacy · Terms · Studio contact                      │
└────────────────────────────────────────────────────────┘
```

**Layout note:** On desktop, use a wide booking column and a narrower, sticky summary. On mobile, use one column and place the summary directly above payment. Keep payment and the final booking button together.

---

## 1. Compact studio header

**What goes here**
- Studio logo or name on the left.
- Location link and “Need help?” contact link on the right.
- Location opens the address in a map; help provides a phone number or email.

**Why it comes first**
Visitors need to know they are booking with the right studio. Keep navigation light: this page should support booking, not send people wandering through a full site menu.

**Annotation**
Use a compact header without a large promotional banner. On small screens, retain the studio name and one clearly labeled help link.

## 2. Short welcome and practical reassurance

**What goes here**

> # Find a little room to breathe.
> Choose a class below and reserve your spot.
>
> **[Studio address]** · Mats available · Beginners welcome

**Why it comes here**
A warm introduction sets the tone, while the location and practical details answer immediate concerns. The schedule should begin within the first screen on most devices.

**Annotation**
Skip a large hero image. If imagery is included, use a small studio photo that does not push class selection down the page. Only publish reassurance claims the studio can verify.

## 3. Choose a class

### 3a. Date and class-type controls

**What goes here**
- Heading: **“1. Choose a class”**
- Current month and year.
- A horizontal seven-day date strip, with today clearly marked.
- Previous/next week controls and a **“Choose another date”** calendar button.
- Class-type filter, defaulting to **“All classes.”**
- Small note: **“All times shown in the studio’s local time.”**

```text
1. Choose a class

[Month Year]                       [Choose another date]

[Mon 12] [Tue 13] [Wed 14] [Thu 15] [Fri 16] [Sat 17] [Sun 18]
          selected

Class type: [All classes ▾]
```

**Why it comes here**
Date is usually the first scheduling constraint. Class type is a secondary choice, so visitors can browse everything without understanding the studio’s terminology.

**Annotations**
- Default to today and show only sessions still available to book.
- Display the full selected date above results.
- Make selected dates distinguishable through text or shape, not color alone.
- Keep filters visible when results are empty.

### 3b. Available class cards

**What goes here**
List sessions in start-time order. Each card includes:
- Start and end time.
- Class name.
- One-sentence description.
- Level and teacher.
- Price and availability.
- **“Select class”** button.

```text
9:00–10:00 AM
Gentle Flow

Slow, supported movement with time to settle.
All levels · With Maya

$18 per person · 4 spots left

[Select class]
```

**Why it comes here**
Time, intensity, and price should be comparable without opening another page. Plain-language descriptions help newcomers choose without guessing what a class name means.

**Annotations**
- Selecting a card changes its button to **“Selected”** and gives the card a clear outline.
- Only one session can be selected at a time.
- Reveal the details section after selection; keep the chosen class visible.
- Full classes show **“Full”** rather than an active booking button.
- Use genuine availability counts, not promotional scarcity.
- Empty state: **“No classes match this date and filter.”** Offer **“Clear filter”** and **“View next available date.”**

## 4. Your details

**What goes here**

```text
2. Your details

Full name
[                                      ]

Email address
[                                      ]
Your confirmation and class details will arrive here.

No account needed.
```

**Why it comes here**
Ask for personal information only after visitors have found a class they want. Two essential fields keep a single-place booking simple.

**Annotations**
- Use persistent labels, not placeholder-only labels.
- Support name and email autofill.
- Show specific inline errors, such as **“Enter a valid email address.”**
- Preserve entered details if the visitor changes their class.
- Do not require a phone number or health information for this assumed booking flow.

## 5. Live booking summary

**What goes here**

```text
YOUR BOOKING

Gentle Flow
[Full selected date] · 9:00–10:00 AM
With Maya
[Studio name and address]

1 place                         $18
Taxes / fees                     $0
───────────────────────────────────
Total                           $18

[Change class]
```

**Why it sits alongside the form**
The summary acts as a steady reference: visitors can verify the date, location, and cost while entering details.

**Annotations**
- Before a selection, show: **“Choose a class to see your booking details here.”**
- On desktop, keep it sticky without overlapping the footer.
- On mobile, place it after “Your details” and before payment.
- Show all applicable fees before payment. The example assumes an $18 all-inclusive price.
- “Change class” returns focus to the schedule without clearing contact details.

## 6. Review and pay

**What goes here**

```text
3. Review & pay

Payment details
[Secure payment provider fields]

Cancellation policy
Cancel at least 12 hours before class for a full refund.
Later cancellations are non-refundable.

By booking, you agree to the booking terms.

[Pay $18 & reserve my spot]

You’ll receive an email confirmation after payment.
```

**Why it comes here**
Payment follows a visible review of the class and total. Clear policy wording beside the final button prevents important conditions from being buried in the footer.

**Annotations**
- Use the payment provider’s accessible payment fields.
- The cancellation wording is a sample; replace it with the studio’s actual policy.
- The final button names both the charge and the action.
- On submission, show **“Reserving your spot…”** and prevent repeat submissions.
- Recheck availability before completing the booking.
- If payment fails, retain the class and contact details and show a retry option.
- If the session fills during checkout, explain that the booking was not completed, clearly state the payment status, and offer **“Choose another class.”**

## 7. Before you arrive

**What goes here**
A compact practical-information block:
- **Arrival:** “Please arrive 10 minutes before class.”
- **What to bring:** Comfortable clothing and a water bottle; confirm whether mats are provided.
- **Getting here:** Full address and a map link.
- **Access information:** Verified details about entrance steps, lifts, and facilities.
- **Questions:** Studio phone or email.

**Why it comes near the bottom**
These details support confidence without delaying the main booking task. Location and beginner reassurance already appear near the top; this block adds useful depth.

**Annotation**
Use short, visible answers rather than a long FAQ. Do not describe the studio as accessible without specific, verified information.

## 8. Minimal footer

**What goes here**
- Studio name and contact information.
- Privacy policy and booking terms links.

**Why it comes last**
Legal and contact information remain easy to find without competing with booking. Leave out newsletter forms and unrelated promotions.

---

## Successful booking state — on the same page

After a confirmed booking, replace the form with a confirmation panel and move keyboard focus to its heading.

```text
✓ Your spot is reserved.

Gentle Flow
[Full booked date] · 9:00–10:00 AM
With Maya · [Studio address]

1 place · Paid $18
Booking reference: [reference]

We’ve sent your confirmation to [email address].

[Add to calendar]   [Manage booking]

Please arrive 10 minutes early.
Mats are available.
```

**Annotations**
- Show success only after the reservation and payment are confirmed.
- Keep the essential booking details on screen; email is a backup, not the only receipt.
- “Manage booking” provides access to cancellation under the stated policy.
- Retain the arrival information and studio contact below the confirmation.

## Whole-page usability notes

- Use numbered headings, generous spacing, and one primary action at each stage.
- Make controls comfortably tappable, with clear keyboard focus and sufficient contrast.
- Announce updated availability, errors, and confirmation to assistive technology.
- Never clear completed fields after a recoverable error.
- Keep the tone calm and direct: **“Choose a class,” “Your details,” “Your spot is reserved.”**

---

**From the agent:** Delivered by Kira: the work is below.
