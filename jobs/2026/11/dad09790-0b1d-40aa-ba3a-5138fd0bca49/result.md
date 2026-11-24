# Yoga Studio Booking Page — Wireframe

**Assumption:** This is a single-page site where visitors choose a class, select a time, enter their details, and confirm a booking. The wireframe is organized into three vertical screens on desktop; on mobile, the same sections stack in order.

## Screen 1 — Choose a class

### 1. Header
- **Left:** Studio logo/name
- **Right:** “Classes,” “About,” and “Contact” links, plus a clear **Book a class** button
- Keep the header compact; make it sticky after scrolling.

**Why:** Visitors can orient themselves quickly, while the booking button stays easy to reach.

### 2. Hero
- **Headline:** “Make space for your practice.”
- **Supporting line:** A short description of the studio and who its classes suit.
- **Primary button:** “Choose a class” — scrolls to the class list
- **Secondary detail:** Studio location or neighborhood
- **Image area:** Calm, welcoming studio or class photo

**Why:** Establishes the studio’s feel and gives first-time visitors an immediate next step without delaying the booking flow.

### 3. Class selector
- **Section heading:** “Find your class”
- **Filter controls:** Day, time of day, class style, and level
- **Class cards or rows:** Each includes:
  - Class name and style
  - Level and duration
  - Instructor
  - Date and start time
  - Price
  - **Select** button
- If no results match, show a helpful empty state with a **Clear filters** option.

**Why:** Visitors can compare practical details before committing. Filters help them find a suitable class without searching through unrelated options.

### 4. Selected-class summary
- Show the chosen class, date, time, instructor, duration, and price.
- Include **Change class** and a prominent **Continue** button.

**Why:** Confirms the visitor’s selection before asking for personal information, reducing mistakes and uncertainty.

---

## Screen 2 — Enter booking details

### 5. Booking progress indicator
- **1. Class → 2. Your details → 3. Confirm**
- Mark the current step clearly.

**Why:** Makes the process feel manageable and shows visitors how much remains.

### 6. Attendee details
- Fields: Full name, email, and phone number
- Optional checkbox: “I’m new to this studio”
- If selected, show a short note about what to expect or what to bring.
- Keep labels visible above fields; show inline validation messages.

**Why:** Collects only what the studio needs to manage the booking. The new-student option allows useful guidance without adding clutter for returning visitors.

### 7. Booking preferences
- Optional field: “Anything we should know?” for access needs or relevant notes
- Optional checkbox for booking studio updates, left unchecked by default

**Why:** Gives attendees room to share useful information while keeping optional requests separate from required booking details.

### 8. Price and policy summary
- Show class price and any applicable fees
- Brief cancellation or rescheduling policy, with a link to full terms
- **Continue to review** button

**Why:** Makes the cost and key terms clear before confirmation, so there are no surprises at the end.

---

## Screen 3 — Review and confirm

### 9. Booking summary
- Class name, date, time, instructor, and location
- Attendee name and email
- Total price
- **Edit class** and **Edit details** links

**Why:** Gives visitors one final chance to catch an error before placing the booking.

### 10. Payment or reservation action
- If payment is required: show secure payment fields and the total due.
- If the studio allows pay-in-person: show that option and state when payment is due.
- Required checkbox to accept the cancellation policy
- Primary button: **Confirm booking** or **Pay and book**

**Why:** Keeps the final action explicit and ties it clearly to the payment or reservation outcome.

### 11. Confirmation state
After submission, replace the final form with:
- Clear success message: “You’re booked.”
- Class, date, time, and studio address
- Confirmation sent to the attendee’s email
- **Add to calendar** button
- Links for directions and contacting the studio
- Brief arrival guidance, such as when to arrive and what to bring

**Why:** Reassures the attendee that the booking succeeded and gives them the next useful details in one place.

## Persistent mobile behavior
Use a compact sticky bottom bar showing the selected class and **Continue** button once a class is chosen. Keep form fields full-width and preserve the same section order.

**Why:** On a small screen, the main action remains accessible without covering the booking information.

---

**From the agent:** Delivered by Prism: the work is below.
