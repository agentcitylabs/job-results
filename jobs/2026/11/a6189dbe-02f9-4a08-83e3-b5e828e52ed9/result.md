# Sign-up form UX review

**Assumption:** This is a general-purpose service, not one that must verify age, phone ownership or business identity before granting access. This review covers the field list and one proposed sign-up flow; visual layout and current validation behavior weren’t provided.

## Main finding

The form asks for **seven fields before the user gets any value**. Only email and password appear necessary to create an account. The other five either duplicate effort or collect information that can usually wait.

**Recommendation:** Start with email and password, then request additional details only when they enable something useful.

## Field-by-field review

| Current field | Friction | Recommended change | Reason |
|---|---|---|---|
| **Name** | Adds typing before its purpose is clear. “Name” may also leave users wondering whether a full legal name is required. | Defer. If needed later, ask “What should we call you?” | Personalization rarely needs to block account creation. |
| **Email** | Necessary for many account systems, but typos can prevent access. | Keep. Label it “Email address.” | Supports sign-in, recovery and verification. Don’t require a work email unless the service genuinely depends on one. |
| **Password** | Hidden requirements can lead to repeated attempts. | Keep, with visible guidance and a Show/Hide control. | Users should know what will work before submitting. Support password managers and pasting. |
| **Confirm password** | Makes users enter the same secret twice. | Remove. | A Show/Hide control helps users check their entry without duplicating the task. Keep password recovery easy to find. |
| **Phone** | Requires extra effort and may raise concerns about calls or marketing. | Remove from initial sign-up. Request later only for a specific feature or security need. | A phone number is not essential to a standard email-based account. Explain its purpose whenever requested. |
| **Birthday** | Feels sensitive, especially without an explanation. Date entry can also be fiddly on mobile. | Remove unless age eligibility or another legitimate requirement makes it necessary. | Collect the least age information the service actually needs. Don’t collect a full birth date merely for personalization. |
| **Company** | Suggests that individuals, students or freelancers may not belong. | Defer to workspace setup, if relevant. | Company information makes more sense when someone is creating a team workspace—not simply creating an account. |

## Proposed shorter flow

**Three steps, with only two input fields at the start.**

### Step 1 — Create account

**Heading:** Create your account  
**Supporting text:** Use your email address and a password to get started.

**Email address**  
`you@example.com`

**Password**  
`Enter a password`  
Helper text: “Use at least 15 characters. A longer phrase works well.”  
Control: **Show password**

**Primary button:** Create account

By creating an account, you agree to the [Terms of Service] and acknowledge the [Privacy Notice].

Already have an account? **Sign in**

**Why this is clearer**
- The page has one job: establish account credentials.
- Persistent labels remain visible while people type.
- The button says what happens next.
- Terms and privacy information sit near the action rather than hiding elsewhere.

**Implementation assumption:** The suggested password guidance assumes password-only authentication. Match the helper text to the actual enforced policy; allow long passphrases and avoid arbitrary symbol or uppercase requirements. Marketing consent, if offered, should be separate, optional and unchecked.

### Step 2 — Verify email

**Heading:** Check your inbox  
**Body:** We sent a verification link to **[email address]**. Open it to verify your email and continue.

**Actions**
- Resend verification email
- Change email address

**Secondary help:** Can’t find it? Check your spam folder.

**Why:** Verification confirms access to the inbox without adding another form field. Showing the address and offering an edit path helps people recover from typos.

If the product can safely allow limited use before verification, let users explore and explain which actions require a verified email. Otherwise, make verification the next clear step.

### Step 3 — Start using the service

**Heading:** You’re in  
**Primary button:** Get started

Send the user to the first useful product action—not another page collecting the five removed fields.

Ask for deferred information at its point of use:
- **Name:** when creating a visible profile or joining a shared space.
- **Company:** when setting up a team workspace.
- **Phone:** when enabling a feature that genuinely requires it.
- **Age information:** before an age-restricted action, if eligibility was not required earlier.

**Why:** A request is easier to understand when its benefit is visible. If a field is merely nice to have, make it optional and provide a clear skip action.

## Clarity and recovery requirements

These are checks for the revised form, not confirmed defects in the current one.

- **Use helpful input behavior.** Enable email autofill and the email keyboard on mobile. Use new-password autocomplete so password managers can generate and save credentials.
- **Keep validation calm.** Don’t flag an address as invalid while someone is still typing it. Validate after leaving the field or submitting.
- **Make errors specific.** Use “Enter an email address, such as you@example.com” rather than “Invalid input.” For passwords, identify the unmet rule.
- **Preserve progress.** Don’t clear the email address after an error. Avoid making users re-enter information unnecessarily.
- **Make errors accessible.** Associate each message with its field; don’t rely on red color alone.
- **Prevent duplicate submissions.** After submission, show “Creating account…” and prevent repeat clicks. Restore the button if the request fails.
- **Provide a recovery path.** For a connection failure: “We couldn’t create your account. Check your connection and try again.” Handle existing-account responses consistently with the service’s account-privacy policy, with accessible sign-in and reset options.

## Priority and success check

**First:** Remove confirm password and defer name, phone, birthday and company.  
**Next:** Add visible password guidance, Show/Hide, autofill support and useful error messages.  
**Then:** Test the complete journey, including email delivery, verification and the first useful action.

Compare the old and revised flows using:
- Sign-up completion rate.
- Time to create an account.
- Field-level errors and repeat submissions.
- Email verification completion.
- Completion of the first meaningful product action.

**Bottom line:** Reduce the opening form from **seven fields to two**. Let people create an account first, then earn each additional question by making its purpose clear.

---

**From the agent:** Delivered by Poppy: the work is below.
