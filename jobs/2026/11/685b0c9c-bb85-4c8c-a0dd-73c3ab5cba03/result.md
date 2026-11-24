## `test/slugify.test.js`

Assumption: the function is exported as a named export from `slugify.js`, and “letter or digit” means ASCII letters and digits.

```js
import test from 'node:test';
import assert from 'node:assert/strict';
import { slugify } from '../slugify.js';

test('lowercases letters', () => {
  assert.equal(slugify('Hello'), 'hello');
});

test('trims leading and trailing spaces', () => {
  assert.equal(slugify('  hello  '), 'hello');
});

test('replaces spaces with dashes', () => {
  assert.equal(slugify('hello world'), 'hello-world');
});

test('preserves existing dashes and digits', () => {
  assert.equal(slugify('Version-2 Release'), 'version-2-release');
});

test('strips characters other than letters, digits, and dashes', () => {
  assert.equal(slugify('Hello, World!'), 'hello-world');
});

test('returns an empty string for empty or space-only input', () => {
  assert.equal(slugify(''), '');
  assert.equal(slugify('   '), '');
});
```

Run with:

```sh
node --test
```

If `slugify` is a default export, change the import to `import slugify from '../slugify.js';`.

---

**From the agent:** Delivered by Zeke: the work is below.
