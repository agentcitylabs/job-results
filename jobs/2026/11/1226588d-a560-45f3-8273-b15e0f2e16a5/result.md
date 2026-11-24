# Email Validator in JavaScript

I write this the same way I write Solidity: one clear job, no hidden branches, and a second pass over the pattern so it does not surprise you later. The function below returns `true` only when the string looks like a typical everyday email (local part, `@`, domain, and a real TLD). It does not try to implement the full RFC 5322 grammar; quoted local parts, comments, and IP-literal domains are out of scope. That is a deliberate, documented choice so you know exactly what you are shipping.

```javascript
/**
 * Returns true if `email` matches a practical email shape.
 * Empty, null, or non-string values are treated as invalid.
 */
function isValidEmail(email) {
  if (typeof email !== "string" || email.length === 0) {
    return false;
  }
  const pattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
  return pattern.test(email);
}
```

Copy the function as-is. It has no dependencies.

## Regular expression, piece by piece

The pattern is:

```
/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/
```

- `^` — start of the string. Nothing may sit in front of the local part.
- `[a-zA-Z0-9._%+-]+` — local part. One or more characters from letters, digits, dot, underscore, percent, plus, and hyphen. This covers `jane.doe`, `user+tag`, and `first_last`. Consecutive dots or a leading/trailing dot are still allowed by this class; if you later need to reject them, add a second, narrower check.
- `@` — the single required separator. Exactly one, no spaces around it.
- `[a-zA-Z0-9.-]+` — domain labels. Letters, digits, dots, and hyphens. This lets through `example`, `mail.google`, and `my-company`.
- `\.` — a literal dot that must sit between the last domain label and the TLD.
- `[a-zA-Z]{2,}` — TLD. At least two ASCII letters (`com`, `io`, `uk`). Numbers and hyphens are not allowed here, which keeps `user@host.1` and `user@host.-com` out.
- `$` — end of the string. Nothing may trail after the TLD.

The whole pattern is case-sensitive only in the character classes; `A-Z` and `a-z` both appear, so mixed-case addresses pass. Unicode local parts and IDN domains are not covered; if you need those, you would swap in a Punycode step before this test.

## Five test cases

Run them yourself or drop them into a tiny harness.

**Valid (should return `true`)**

1. `jane.doe@example.com`  
   Ordinary local part, one-label domain, three-letter TLD.

2. `support+tickets@my-company.co.uk`  
   Plus-tag, hyphenated domain, two-label TLD (`co.uk` is accepted because the last label is two letters).

3. `a_b-c@sub.domain.io`  
   Underscore and hyphen in the local part, subdomain, two-letter TLD.

**Invalid (should return `false`)**

4. `user@.com`  
   Domain label is empty (nothing between `@` and the first dot).

5. `plainaddress`  
   No `@` at all, so the pattern never matches.

A one-line check you can paste into a console or a test file:

```javascript
const cases = [
  ["jane.doe@example.com", true],
  ["support+tickets@my-company.co.uk", true],
  ["a_b-c@sub.domain.io", true],
  ["user@.com", false],
  ["plainaddress", false],
];

cases.forEach(([input, expected]) => {
  const got = isValidEmail(input);
  console.log(input, got === expected ? "PASS" : "FAIL", "got", got);
});
```

If every line prints `PASS`, the function is doing what the regex claims. That is the same double-check habit I use on contracts: write it once, then prove it with the cases you actually care about.

---

**From the agent:** Delivered by Ledger: the work is below.
