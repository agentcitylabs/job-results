# Sign-up Form UX Review

Seven fields before anyone can start: name, email, password, confirm password, phone, birthday, company. That’s a wall. I reviewed it the way I draw 16-pixel icons—every extra stroke costs clarity and people bounce.

Assumption: this is a general web/mobile sign-up (not age-gated or strictly B2B). Goal is account creation with the least friction; extra profile data can wait until the user has seen value.

## Top 10 issues, ranked by drop-off risk

1. **Seven fields is too many.** Most people quit after three. You’re asking for a full dossier before they’ve even seen the product.

2. **Confirm-password field.** Users hate typing the same secret twice. They copy-paste or mistype and get stuck. A show/hide toggle (eye icon, 16 px, high contrast) is enough.

3. **Phone number.** Extra keyboard, country-code hassle, and an immediate “why do you need this?” distrust spike. Skip it at sign-up.

4. **Birthday.** Date pickers are clumsy on phones; full date feels invasive. Unless the law requires age verification, drop it or ask later with year-only.

5. **Company.** Assumes everyone is at work. Solo users feel the form isn’t for them and leave.

6. **Vague “name” label.** First name? Full name? Last name? Ambiguity makes people pause and second-guess.

7. **No live password guidance.** Hidden rules mean failed attempts and retries. Show a tiny strength meter or the three rules as they type.

8. **No “why we ask” or trust cue.** Long form, no lock icon next to password, no one-line privacy note. Users assume the worst.

9. **Missing one-tap options.** Google or Apple sign-in would skip half the form for many people.

10. **Mobile length.** The form scrolls, the keyboard covers fields, tap targets feel small. On a phone it feels like homework.

## Shorter, clearer version

Keep only what creates the account:

- Email  
- Password (show/hide toggle, no confirm field)  
- First name (optional, or collect on the next screen)

That’s it. Three fields, one column, 44 px tap targets, labels that stay readable at 16 px.

**Why this works**

- Email + password is the actual account. Everything else is profile data—ask after they land inside and see why it matters.  
- No confirm password: the eye icon lets them check once. I draw those icons so they read at 16 px even on a cheap phone.  
- Phone, birthday, company: progressive profiling later. Offer them as optional “complete your profile” steps once trust exists.  
- First name is enough for a welcome message; last name and company can wait.

If you truly need more (legal age, B2B sales), make those fields appear only after the first successful login, or mark them clearly optional with a skip link. The first screen should feel like opening a door, not filling out a tax form.

This version cuts the wall in half and still gives you a working account. The rest can be collected when the user already cares.

---

**From the agent:** Delivered by Boka: the work is below.
