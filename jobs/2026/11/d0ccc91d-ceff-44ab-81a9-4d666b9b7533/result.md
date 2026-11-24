# Unit tests for `slugify(text)`

I wrote these the way I write Solidity tests: one behaviour per case, no hidden magic, and the pipeline spelled out so a failing assertion tells you *which* step drifted.

## Assumptions (so we do not guess in silence)

The brief does not ship `slugify`, so the tests import it from `./slugify.js` as a named export:

```js
module.exports = { slugify };
```

Processing order I am holding you to (the only order that matches the words “replace spaces” *and* “strip anything that is not a letter, digit or dash”):

1. Lowercase the whole string.
2. Trim leading and trailing whitespace (`String.prototype.trim`).
3. Replace every U+0020 space with a single `-`. Multiple spaces become multiple dashes; they are **not** collapsed.
4. Strip every character that is not `a-z`, `0-9`, or `-`.

Further choices, written down so they are not surprises:

- “Letter” means ASCII `a-z` after lowercasing. Accented letters (`é`) are stripped.
- Only the space character becomes a dash. Tabs and newlines are not spaces, so they are stripped in step 4.
- Existing dashes stay. Consecutive dashes stay. The function does not squeeze `--` into `-`.
- Input is a string. These tests do not cover `null`, numbers, or objects.
- Empty string, whitespace-only string, and punctuation-only string all yield `""`.

If your implementation collapses dashes, maps Unicode letters, or treats all whitespace as spaces, the matching tests will fail on purpose. Change the assertion, not the spec, unless you meant to change the spec.

## How to run

Node 18+ (built-in runner, no extra packages):

```bash
node --test slugify.test.js
```

Put `slugify.js` next to the test file. A one-line smoke check after you implement it:

```bash
node --test --test-reporter=spec slugify.test.js
```

---

```js
/**
 * slugify.test.js — Node built-in test runner
 *
 * Pipeline under test:
 *   lowercase → trim → spaces to '-' → keep only [a-z0-9-]
 *
 * Run: node --test slugify.test.js
 */

const { describe, it } = require('node:test');
const assert = require('node:assert/strict');
const { slugify } = require('./slugify.js');

describe('slugify', () => {
  it('lowercases ASCII letters', () => {
    assert.equal(slugify('HELLO'), 'hello');
    assert.equal(slugify('HeLLo'), 'hello');
  });

  it('trims leading and trailing whitespace', () => {
    assert.equal(slugify('  hello  '), 'hello');
    assert.equal(slugify('\nhello\t'), 'hello');
  });

  it('replaces each space with a dash', () => {
    assert.equal(slugify('hello world'), 'hello-world');
  });

  it('does not collapse multiple spaces', () => {
    assert.equal(slugify('hello   world'), 'hello---world');
  });

  it('strips punctuation after spaces become dashes', () => {
    assert.equal(slugify('hello, world!'), 'hello-world');
    assert.equal(slugify('Wait... what?'), 'wait-what');
  });

  it('keeps digits', () => {
    assert.equal(slugify('Article 42'), 'article-42');
    assert.equal(slugify('ISO 8601 date'), 'iso-8601-date');
  });

  it('keeps dashes that were already there', () => {
    assert.equal(slugify('hello-world'), 'hello-world');
    assert.equal(slugify('a--b'), 'a--b');
  });

  it('strips underscores, at-signs, and similar', () => {
    assert.equal(slugify('hello_world@test'), 'helloworldtest');
  });

  it('returns an empty string for empty input', () => {
    assert.equal(slugify(''), '');
  });

  it('returns an empty string when nothing allowed remains', () => {
    assert.equal(slugify('!!!'), '');
    assert.equal(slugify('   '), '');
    assert.equal(slugify('___'), '');
  });

  it('applies lowercase, trim, spaces, and strip together', () => {
    assert.equal(slugify('  Hello, World!  '), 'hello-world');
  });

  it('strips internal tabs and newlines instead of turning them into dashes', () => {
    assert.equal(slugify('hello\tworld'), 'helloworld');
    assert.equal(slugify('hello\nworld'), 'helloworld');
  });

  it('turns spaces around an existing dash into extra dashes', () => {
    // trim(" - hello - ") → "- hello -" → "--hello--"
    assert.equal(slugify(' - hello - '), '--hello--');
  });

  it('strips non-ASCII letters (é is not [a-z])', () => {
    assert.equal(slugify('café'), 'caf');
    assert.equal(slugify('naïve'), 'nave');
  });

  it('handles a realistic title', () => {
    assert.equal(
      slugify('  Node.js 18: Test Runner (finally!)  '),
      'nodejs-18-test-runner-finally'
    );
  });

  it('keeps a single character that is already valid', () => {
    assert.equal(slugify('A'), 'a');
    assert.equal(slugify('7'), '7');
    assert.equal(slugify('-'), '-');
  });
});
```

That is 16 cases, each pinning one clause of the brief or one edge the pipeline actually produces. If a test fails, read the name first: it names the step that drifted.

---

**From the agent:** Delivered by Ledger: the work is below.
