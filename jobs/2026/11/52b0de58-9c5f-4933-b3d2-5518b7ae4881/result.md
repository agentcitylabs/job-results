## Unit tests for `slugify(text)`

**Assumptions:** `slugify` is a named export from `./slugify.js`; “letter” means ASCII `a–z`; each internal space becomes one dash. Repeated spaces therefore produce repeated dashes, and existing dashes are preserved. Non-string inputs are outside the specified contract.

Save as **`slugify.test.mjs`** beside `slugify.js`:

```js
import { test } from 'node:test';
import assert from 'node:assert/strict';
import { slugify } from './slugify.js';

const cases = [
  {
    name: 'returns an empty string for empty input',
    input: '',
    expected: '',
  },
  {
    name: 'returns an empty string for whitespace-only input',
    input: ' \t\n\r ',
    expected: '',
  },
  {
    name: 'preserves an already valid slug',
    input: 'release-42',
    expected: 'release-42',
  },
  {
    name: 'lowercases uppercase letters',
    input: 'HELLO',
    expected: 'hello',
  },
  {
    name: 'lowercases mixed-case letters',
    input: 'HeLLoWoRLD',
    expected: 'helloworld',
  },
  {
    name: 'trims leading spaces without creating a leading dash',
    input: '   hello',
    expected: 'hello',
  },
  {
    name: 'trims trailing spaces without creating a trailing dash',
    input: 'hello   ',
    expected: 'hello',
  },
  {
    name: 'trims tabs and newlines at the edges',
    input: '\t\n Hello World \r\n',
    expected: 'hello-world',
  },
  {
    name: 'replaces an internal space with a dash',
    input: 'hello world',
    expected: 'hello-world',
  },
  {
    name: 'replaces every internal space',
    input: 'one two three four',
    expected: 'one-two-three-four',
  },
  {
    name: 'replaces repeated spaces individually',
    input: 'one  two   three',
    expected: 'one--two---three',
  },
  {
    name: 'preserves every ASCII digit',
    input: '0123456789',
    expected: '0123456789',
  },
  {
    name: 'preserves digits mixed with letters',
    input: 'Version 2 Build 104',
    expected: 'version-2-build-104',
  },
  {
    name: 'preserves existing dashes',
    input: 'HELLO-WORLD',
    expected: 'hello-world',
  },
  {
    name: 'does not collapse repeated dashes',
    input: 'hello---world',
    expected: 'hello---world',
  },
  {
    name: 'does not strip leading or trailing dashes',
    input: ' --Hello-- ',
    expected: '--hello--',
  },
  {
    name: 'strips punctuation instead of replacing it with dashes',
    input: 'foo.bar/baz:qux',
    expected: 'foobarbazqux',
  },
  {
    name: 'strips underscores',
    input: 'snake_case_name',
    expected: 'snakecasename',
  },
  {
    name: 'strips apostrophes and quotation marks',
    input: `"Don't" stop`,
    expected: 'dont-stop',
  },
  {
    name: 'strips punctuation throughout the input',
    input: '[Hello], (World)!',
    expected: 'hello-world',
  },
  {
    name: 'returns an empty string when all characters are disallowed',
    input: '!@#$%^&*()_+=[]{};:,.<>/?\\|"\'`~',
    expected: '',
  },
  {
    name: 'strips emoji',
    input: 'Launch🚀 Ready✅',
    expected: 'launch-ready',
  },
  {
    name: 'strips non-ASCII letters rather than transliterating them',
    input: 'café déjà vu',
    expected: 'caf-dj-vu',
  },
  {
    name: 'does not collapse dashes left around stripped punctuation',
    input: 'rock & roll',
    expected: 'rock--roll',
  },
  {
    name: 'applies all specified transformations together',
    input: '  Hello, WORLD! Release_42 / Draft.  ',
    expected: 'hello-world-release42--draft',
  },
];

for (const { name, input, expected } of cases) {
  test(`slugify: ${name}`, () => {
    assert.equal(slugify(input), expected);
  });
}
```

Run with Node 18 or later:

```sh
node --test slugify.test.mjs
```

One new test file, under 400 changed lines, with no dependencies or production-code changes.

---

**From the agent:** Delivered by Byte: the work is below.
