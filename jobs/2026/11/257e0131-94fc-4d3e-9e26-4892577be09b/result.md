# Sign-up form UX review

**Assumption:** This is a general-purpose product with email-and-password accounts. Phone, birthday and company are not essential to create an account. This review covers the field list rather than a visual design.

## Main finding

The form asks for **seven pieces of information before someone can get started**. The biggest improvement is to separate what creates an account from what fills out a profile.

**Recommendation: start with email and password only.** Ask for other details when they unlock something useful—and explain why you need them.

## 1. Current form: where the friction sits

| Field | Recommendation | Reason |
|---|---|---|
| **Name** | Defer | Usually unnecessary for account creation. Ask when someone creates a public profile, joins a team or needs a display name. Keep a single name field unless there is a real need to split it. |
| **Email** | Keep | Provides an account identifier and a route for verification and recovery. Explain its purpose rather than leaving people to wonder about marketing. |
| **Password** | Keep, with clear requirements | Hidden rules create avoidable retries. Show requirements before typing, support password managers and allow paste. |
| **Confirm password** | Remove | Makes people enter the same information twice. A **Show password** control helps them catch typing errors without another field. |
| **Phone** | Remove from initial sign-up | Can raise concerns about calls, texts and privacy. Request it when someone chooses a feature that actually requires it, with a clear explanation. |
| **Birthday** | Remove unless required | A full birth date is sensitive information. If eligibility or regulation requires an age check, collect only what is necessary and place it at the appropriate point. |
| **Company** | Defer or omit | May make independent users feel the product is not for them. Ask during workspace setup only if it affects the experience. |

Making five fields “optional” would still leave a long-looking form. **Remove them from this screen rather than keeping them as visual clutter.**

## 2. Suggested shorter version

### Screen 1 — Create account

The following assumes a minimum password length of 15 characters with no mandatory symbol or number rules. Match the helper text to the actual policy.

---

# Create your account

**Email address**  
`you@example.com`  
We’ll use this for sign-in and account recovery.

**Password**  
`Enter a password` **Show password**  
Use at least 15 characters. A longer phrase works well.

**Create account**

By creating an account, you agree to the [Terms] and acknowledge the [Privacy Policy].

Already have an account? **Sign in**

---

### Why this is clearer

- **Two fields instead of seven:** less typing and fewer decisions before entry.
- **A specific action:** “Create account” tells people what the button does; “Submit” does not.
- **Visible labels:** placeholders are examples, not replacements for labels.
- **Requirements up front:** no password guessing game after clicking the button.
- **A sign-in route:** returning users have an easy way out of the wrong flow.

Legal wording should match the product’s consent requirements. If marketing opt-in is offered, keep it separate, optional and unchecked.

### Screen 2 — Verify email, if required

---

# Check your inbox

We sent a verification link to **alex@example.com**. Open it to verify your email and continue.

Wrong address? **Change email**

No email yet? Check your spam folder or **Resend email**.

---

**Why this helps:** It states what happened, what to do next and how to recover from a typo. Keep the resend control in a predictable place; if it has a cooldown, show how long remains.

If verification is not needed before exploration, consider allowing limited access first. Require verification before sensitive actions or actions that could enable abuse.

### Screen 3 — Collect details at a useful moment

**Do not automatically follow sign-up with another profile questionnaire.** A shorter first screen is not much of a win if the same questions immediately reappear.

For a product that needs a workspace, a contextual screen could be:

---

# Name your workspace

Choose a name your teammates will recognize. You can change it later.

**Workspace name**  
`e.g., Paper Kite Studio`

**Create workspace**

---

A workspace name does not have to be a legal company name. For an individual-use product, skip this screen and take people straight to their first useful action.

Ask for phone or birthday only when genuinely necessary—not simply because those fields existed in the old form.

## 3. Interaction details that prevent snags

- **Use the right input support.** Set email to `type="email"` with `autocomplete="email"` and password to `autocomplete="new-password"`. Do not block paste or password managers.
- **Keep validation calm.** Validate after a field loses focus or on submission, rather than showing errors while someone is still typing.
- **Make errors actionable.** Use “Enter an email address like name@example.com” or “Use at least 15 characters,” not “Invalid input.”
- **Preserve work.** Keep entered values when validation fails. Do not make someone rebuild the form to fix one field.
- **Make errors accessible.** Associate each message with its field, announce submission errors to assistive technology and never rely on red alone.
- **Give the password toggle a clear state.** Use “Show password” and “Hide password,” with keyboard access and an accessible state.
- **Show submission progress.** Change the button to “Creating account…” while processing and prevent duplicate submissions. If a network request fails, provide a retry path without clearing the form.

## Priority and success checks

**Do first:** remove confirm password, defer nonessential personal details, and clarify password rules.

**Then check:** mobile keyboards, autofill, accessible errors, email correction and resend behavior.

Measure:
- Sign-up completion rate.
- Time to complete the form.
- Validation errors per attempt.
- Verification completion, if applicable.
- Completion of the first meaningful product action.

**Bottom line:** Ask for what opens the door—email and password. Gather the rest when there is a clear, useful reason to ask.

---

**From the agent:** Delivered by Poppy: the work is below.
