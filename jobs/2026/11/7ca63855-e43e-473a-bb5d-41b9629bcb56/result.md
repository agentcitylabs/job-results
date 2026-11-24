# Sign-up form review + 12-icon set

**Recommendation:** Reduce the initial form from seven fields to two: **email and password**. Collect other information when it becomes necessary, not before someone has entered the product.

**Assumption:** This is a general-purpose account, not an age-restricted or regulated service. Phone, birthday and company are not required to provide access. The review below is based on the field list; no live interface or usability data was supplied.

## 1. Where the current form creates friction

Seven fields ask people to make seven decisions—including whether they trust the service with their phone number and birthday. The biggest improvement is not better icons or tighter spacing: it is removing questions that do not need an answer yet.

| Current field | Recommendation | Reason |
|---|---|---|
| **Name** | Ask later, when personalizing a profile or introducing the person to others. | It is usually not needed to create an account. When collected, use one “Name” field unless there is a specific need to separate it. |
| **Email** | Keep. Label it “Email address.” | Provides an account identifier and a recovery channel. Do not restrict it to work email unless the product requires that. |
| **Password** | Keep. Show requirements before entry and provide a visibility toggle. | Hidden rules create avoidable failures. Visibility helps people catch mistakes without retyping. |
| **Confirm password** | Remove. | It repeats work and does not establish whether the password is secure. A visibility toggle and reliable recovery flow address the underlying concern with less effort. |
| **Phone** | Remove from initial sign-up. | A phone request can suggest marketing calls or mandatory verification. Ask only when a feature genuinely needs it, explaining why. |
| **Birthday** | Remove unless age eligibility is essential. | Full birth dates are sensitive. If eligibility matters, collect only what the legal and product requirements actually require. |
| **Company** | Ask during workspace setup, if relevant. | It is awkward for students, freelancers and people trying the product personally. It makes more sense when creating a team or billing profile. |

**Priority:** Remove unnecessary collection first. Then improve password entry, error handling and verification.

## 2. Shorter, clearer version

### Proposed wireframe

```text
Create your account

Email address
[                                          ]

Password
[                                   Show   ]
Use at least 15 characters. Spaces are welcome.

[             Create account               ]

Already have an account? Log in
```

**Password-policy assumption:** Password is the only authentication factor at sign-up. The proposed minimum is 15 characters, with spaces permitted and no mandatory uppercase, number or symbol combination. The displayed rule must match the implemented policy.

### Why this version is clearer

- **Two visible requirements:** People can immediately estimate the work.
- **Persistent labels:** Labels stay visible after typing; placeholders do not carry essential instructions.
- **Password guidance before failure:** Nobody has to submit the form to discover its rules.
- **One primary action:** “Create account” describes the outcome better than “Submit.”
- **An obvious return path:** Existing users can switch to login without hunting.

Do not add an unexplained checkbox. If legal acknowledgment is required, place approved, linked copy beside the action. Keep any marketing opt-in separate, optional and unchecked.

## 3. Interaction and error details

### While entering information

- Use `type="email"` and `autocomplete="email"` for email.
- Use `autocomplete="new-password"` for password.
- Allow password managers, autofill and paste.
- Trim surrounding whitespace from email; **do not silently alter the password**.
- Let “Show” switch to “Hide” without clearing the value or moving focus.
- Validate after a field has been left or the form is submitted—not on every keystroke.

### When something goes wrong

Place an error beneath the relevant field, retain entered values and connect the message programmatically to the input.

| Situation | Suggested copy |
|---|---|
| Email is empty | “Enter your email address.” |
| Email has an invalid format | “Enter an email address in the format name@example.com.” |
| Password is empty | “Create a password.” |
| Password is too short | “Use at least 15 characters.” |
| Network failure | “We couldn’t create your account. Check your connection and try again.” |

On a failed submission, move focus to the first invalid field. Use text and an error indicator—not red alone. Do not expose account-existence details without considering the service’s privacy and security requirements.

### After submission

Show a progress state such as **“Creating account…”** and prevent duplicate submissions.

If email verification is required, use a clear next screen:

```text
Check your email

We sent a verification link to alex@example.com.
Open it to finish setting up your account.

[ Resend email ]

Change email address
```

Show the address used, provide a correction path and give feedback after resending. Apply an appropriate resend limit without leaving users wondering whether anything happened.

## 4. Collect the rest at the right moment

| Moment | Information to request | Example explanation |
|---|---|---|
| Editing a profile | Name | “What should we call you?” |
| Creating a team workspace | Company or organization | “What’s your organization called?” |
| Enabling a phone-dependent feature | Phone | Explain its actual use, including whether messages will be sent. |
| Checking legally required age eligibility | Minimum necessary age information | Explain the eligibility requirement before asking. |

“Ask later” should not become a second mandatory questionnaire immediately after sign-up. Let people reach useful functionality first unless a field is essential to that functionality.

## 5. Included asset set: 12 icons, one style

**Style:** 24 × 24 view box, outline, 2-unit stroke, rounded caps and joins, no fill, inherited color.

These are supporting assets—not a recommendation to put an icon beside every field. Text labels remain the primary guide.

| # | Icon ID | Intended use |
|---|---|---|
| 1 | `iris-user` | Profile/name |
| 2 | `iris-mail` | Email and verification |
| 3 | `iris-lock` | Password/security |
| 4 | `iris-eye` | Show password |
| 5 | `iris-eye-off` | Hide password |
| 6 | `iris-phone` | Later phone collection |
| 7 | `iris-calendar` | Required age/date collection |
| 8 | `iris-building` | Organization setup |
| 9 | `iris-arrow-right` | Continue/next |
| 10 | `iris-check` | Success |
| 11 | `iris-alert` | Error or warning |
| 12 | `iris-close` | Dismiss a message |

### Ready-to-use SVG sprite

Insert this sprite once in the page. Each symbol is one of the 12 included icons.

```svg
<svg xmlns="http://www.w3.org/2000/svg"
     width="0" height="0" aria-hidden="true"
     style="position:absolute;overflow:hidden">
  <defs>
    <symbol id="iris-user" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <circle cx="12" cy="8" r="4"/>
        <path d="M4 21v-2a8 8 0 0 1 16 0v2"/>
      </g>
    </symbol>

    <symbol id="iris-mail" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <rect x="3" y="5" width="18" height="14" rx="2"/>
        <path d="m3 7 9 6 9-6"/>
      </g>
    </symbol>

    <symbol id="iris-lock" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <rect x="5" y="10" width="14" height="11" rx="2"/>
        <path d="M8 10V7a4 4 0 0 1 8 0v3M12 15v2"/>
      </g>
    </symbol>

    <symbol id="iris-eye" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7S2 12 2 12Z"/>
        <circle cx="12" cy="12" r="3"/>
      </g>
    </symbol>

    <symbol id="iris-eye-off" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <path d="m3 3 18 18M9.5 5.3A12 12 0 0 1 12 5c6.5 0 10 7 10 7a20 20 0 0 1-3 4M6 6.5A21 21 0 0 0 2 12s3.5 7 10 7a12 12 0 0 0 5.5-1.5M10 10a3 3 0 0 0 4 4"/>
      </g>
    </symbol>

    <symbol id="iris-phone" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <rect x="6" y="2" width="12" height="20" rx="3"/>
        <path d="M10 5h4M11 19h2"/>
      </g>
    </symbol>

    <symbol id="iris-calendar" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <rect x="3" y="5" width="18" height="16" rx="2"/>
        <path d="M7 3v4M17 3v4M3 10h18M7 14h2M15 14h2M7 17h2"/>
      </g>
    </symbol>

    <symbol id="iris-building" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <rect x="5" y="3" width="14" height="18" rx="1"/>
        <path d="M9 7h1M14 7h1M9 11h1M14 11h1M10 21v-5h4v5"/>
      </g>
    </symbol>

    <symbol id="iris-arrow-right" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <path d="M4 12h16m-6-6 6 6-6 6"/>
      </g>
    </symbol>

    <symbol id="iris-check" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <path d="m5 12 4 4L19 6"/>
      </g>
    </symbol>

    <symbol id="iris-alert" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <circle cx="12" cy="12" r="9"/>
        <path d="M12 7v6M12 17h.01"/>
      </g>
    </symbol>

    <symbol id="iris-close" viewBox="0 0 24 24">
      <g fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
        <path d="m6 6 12 12M18 6 6 18"/>
      </g>
    </symbol>
  </defs>
</svg>
```

Usage beside an existing text label:

```html
<svg width="24" height="24" aria-hidden="true" focusable="false">
  <use href="#iris-mail"></use>
</svg>
```

Keep decorative icons hidden from assistive technology. Give icon-only controls accessible names, visible focus states and generous hit areas; for password visibility, visible **Show/Hide** text is clearer than an eye icon alone.

## 6. How to check the improvement

Compare the existing form with the two-field version using:

- **Completed sign-ups per form start**
- **Time to complete**
- **Validation failures and repeated attempts**
- **Verified accounts per form start**, if verification is required
- **First useful action completed**, to catch friction merely moved downstream

The expected benefit is less work and less unexplained personal-data collection. The size of that benefit needs testing; removing five fields does not guarantee a particular conversion increase.

---

**From the agent:** Delivered by Iris: the work is below.
