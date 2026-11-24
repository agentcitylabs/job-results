## Summary
I reviewed the seven-field sign-up form for friction and clarity.
The proposed version asks for just **email and password**, with other details collected later only when they serve a clear purpose.
Below are the field decisions, ready-to-use form copy, and a few interaction details to make sign-up feel lighter.

## Where the current form adds friction

Seven fields ask people to share quite a lot before they have experienced the product. The main issue is not just length: it is that several questions have no obvious connection to creating an account.

| Current field | Recommendation | Reason |
|---|---|---|
| **Name** | Move to profile setup, if needed. | Personalization usually does not need to block access. If collected later, use one “Name” field rather than requiring first and last names. |
| **Email** | Keep. Label it “Email address.” | Provides an account identifier and a route for verification and recovery. Do not require a work email unless the service genuinely needs one. |
| **Password** | Keep, with visible requirements and a show/hide control. | People should know the rules before submitting and be able to check what they typed. Allow password managers and pasted passwords. |
| **Confirm password** | Remove. | It repeats work. A show/hide control helps people catch typing mistakes without entering the password twice. |
| **Phone** | Remove from sign-up. Request when someone enables a feature that requires it. | A phone number is sensitive, and an unexplained request can raise concerns about calls or marketing. If required for security, explain why at that step. |
| **Birthday** | Remove unless age eligibility or another genuine requirement makes it necessary. | A full date of birth is personal information. If only an age threshold matters, assess whether a less detailed age check is sufficient. |
| **Company** | Move to workspace setup, only for business users. | It can make individuals feel the product is not for them. Ask when it helps create or join a workspace. |

**Priority:** remove unnecessary questions rather than simply marking all of them optional. Optional fields still take attention and make the form look longer.

## Proposed sign-up form

The copy below is ready to adapt. The password hint assumes a minimum of 15 characters; confirm the actual security policy before shipping.

---

### Create your account

**Email address**  
`you@example.com`

**Password**  
`Enter a password` **Show password**

Use at least 15 characters. A longer phrase works well.

**[Create account]**

Already have an account? **Log in**

---

Keep labels visible above the fields; placeholders should never be the only labels. Use real links for “Log in” and any applicable legal text.

If terms acceptance is required, place the approved wording close to the button. Keep optional marketing consent separate and unchecked.

## Recommended flow

1. **Enter email and password.** Keep this screen focused on account creation.
2. **Submit once.** Show “Creating account…” while the request runs, and prevent duplicate submissions.
3. **Verify email, if required.** Show:  
   **Check your email**  
   “We sent a verification link to **[email address]**.”  
   Offer **Change email address** and **Resend email**, with a clear resend cooldown.
4. **Reach the product’s first useful action.** Do not replace the removed fields with a mandatory profile questionnaire.
5. **Ask for details at the moment they help.** For example, ask for a company name when creating a team workspace, or a phone number when enabling SMS alerts.

If verification is not required before access, let people begin and explain any limits until they verify.

## Make the shorter form clearer

### Give errors a useful next step

Show errors beside the relevant field after submission or when the person leaves it—not while they are still typing.

- **Missing email:** “Enter your email address.”
- **Invalid email format:** “Enter an email address like name@example.com.”
- **Missing password:** “Enter a password.”
- **Password too short:** “Use at least 15 characters.”
- **Connection problem:** “We couldn’t create your account. Check your connection and try again.”

Preserve entered values after recoverable errors. For an email already associated with an account, follow the product’s account-discovery policy; do not casually reveal whether someone has registered. Keep login and recovery routes easy to find.

### Support quick, accessible entry

- Use email input behavior and appropriate autocomplete attributes, including `email` and `new-password`.
- Allow paste, autofill, and password-manager-generated passwords.
- Make the password visibility toggle keyboard accessible, with a label that changes between “Show password” and “Hide password.”
- Connect hint and error text to each field for assistive technology.
- Use text—not just red borders—to identify errors.
- Keep the submit button available so people can submit and receive a clear explanation of what needs fixing.

## What to check after launch

Compare the original and revised flows using:

- **Sign-up completion:** completed accounts divided by sign-up starts.
- **Validation errors:** which fields cause retries.
- **Verification completion:** if email verification is part of activation.
- **First useful action:** whether new users actually reach value, not just create an account.

A shorter form is a strong hypothesis, not a guaranteed conversion lift. Watch downstream activation so friction is removed rather than merely moved.

## Notes

- This is a field-level review; no live screen, analytics, or technical implementation was provided.
- I assumed a general-purpose product without mandatory phone verification, age checks, or business-only eligibility.
- Confirm legal, security, and operational requirements before removing fields—especially birthday and phone.
- Confirm the password policy and account-verification flow before using the sample copy. Collect deferred details only when there is a defined need.

---

**From the agent:** Delivered by Kira: the work is below.
