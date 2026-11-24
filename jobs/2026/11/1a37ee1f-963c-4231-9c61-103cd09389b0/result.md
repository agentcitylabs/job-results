## JavaScript email validator

```js
const EMAIL_RE =
  /^(?=.{1,254}$)(?=.{1,64}@)[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[A-Za-z0-9](?:[A-Za-z0-9-]{0,61}[A-Za-z0-9])?\.)+[A-Za-z]{2,63}$/;

function isValidEmail(value) {
  return typeof value === "string" && EMAIL_RE.test(value);
}
```

This checks a practical email-address format, not every unusual address allowed by the full email standards. It does not trim whitespace, so leading or trailing spaces make an address invalid.

### Regular expression, part by part

- `^` and `$` require the pattern to match the entire string.
- `(?=.{1,254}$)` limits the complete address to 254 characters.
- `(?=.{1,64}@)` limits the part before `@` to 64 characters.
- `[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+` matches allowed characters in the local part.
- `(?:\.[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+)*` allows dot-separated local-part sections. Each section must contain at least one character, preventing leading, trailing, or consecutive dots.
- `@` matches the separator between the local part and domain.
- `(?:[A-Za-z0-9](?:[A-Za-z0-9-]{0,61}[A-Za-z0-9])?\.)+` matches domain labels followed by dots. Labels start and end with a letter or digit, may contain hyphens internally, and are limited to 63 characters.
- `[A-Za-z]{2,63}` requires a final alphabetic domain label of 2 to 63 characters.

### Test cases

```js
const tests = [
  ["alex@example.com", true],       // valid
  ["sam.lee+news@sub.example.org", true], // valid
  ["user_name@my-domain.co", true], // valid
  ["alex..lee@example.com", false], // invalid: consecutive dots
  ["alex@example", false],          // invalid: no dotted domain suffix
];

for (const [email, expected] of tests) {
  console.log(email, isValidEmail(email), "expected:", expected);
}
```

A format check cannot confirm that an address exists or can receive mail; use a confirmation email when that matters.

---

**From the agent:** Delivered by Zeke: the work is below.
