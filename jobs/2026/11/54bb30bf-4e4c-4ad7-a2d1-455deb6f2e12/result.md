## JavaScript email validator

**Assumption:** This checks common email formats, such as `alex@example.com` and `alex.smith+news@sub.example.org`. It requires a dotted domain and rejects surrounding whitespace rather than trimming it. It does not cover every format permitted by email standards or confirm that a mailbox exists.

```javascript
function isValidEmail(email) {
  if (typeof email !== "string" || email.length > 254) {
    return false;
  }

  const pattern = /^[A-Za-z0-9_%+-]+(?:\.[A-Za-z0-9_%+-]+)*@(?:[A-Za-z0-9](?:[A-Za-z0-9-]{0,61}[A-Za-z0-9])?\.)+[A-Za-z]{2,63}$/;

  if (!pattern.test(email)) {
    return false;
  }

  // The part before @ must not exceed 64 characters.
  return email.indexOf("@") <= 64;
}
```

### Regular expression, part by part

- **`^`** — Starts matching at the beginning of the string.
- **`[A-Za-z0-9_%+-]+`** — Requires one or more letters, digits, underscores, percent signs, plus signs, or hyphens before `@`.
- **`(?:\.[A-Za-z0-9_%+-]+)*`** — Allows additional sections separated by literal dots. Each dot must be followed by at least one allowed character, preventing leading, trailing, or consecutive dots in the local part. `(?:...)` groups characters without capturing them; `*` allows zero or more repetitions.
- **`@`** — Requires the separator between the local part and domain.
- **`(?:[A-Za-z0-9](?:[A-Za-z0-9-]{0,61}[A-Za-z0-9])?\.)+`** — Matches one or more domain labels followed by dots, such as `example.` or `mail.example.`:
  - Each label starts with a letter or digit.
  - The optional remainder allows internal hyphens but must end with a letter or digit.
  - `{0,61}` limits the middle portion so each label is at most 63 characters.
  - `\.` matches a literal dot; the outer `+` requires at least one label.
- **`[A-Za-z]{2,63}`** — Requires a final domain suffix of 2–63 letters, such as `com` or `org`.
- **`$`** — Ends the match at the end of the string.

Whitespace is not allowed by any character class, so spaces and line breaks are rejected.

### Five test cases

```javascript
const tests = [
  // Three valid addresses:
  ["alex@example.com", true],
  ["alex.smith+news@sub.example.org", true],
  ["team_42@my-company.co.uk", true],

  // Two invalid addresses:
  ["alex..smith@example.com", false], // Consecutive local-part dots
  ["alex.example.com", false],       // Missing @
];

for (const [email, expected] of tests) {
  const actual = isValidEmail(email);
  console.log({ email, expected, actual, passed: actual === expected });
}
```

**Scope:** This deliberately excludes quoted local parts, internationalized addresses, and some less-common valid characters. For account registration, follow format validation with a verification email to confirm mailbox access.

---

**From the agent:** Delivered by Byte: the work is below.
