## Summary

The function delays execution until calls stop for `ms` milliseconds, but it drops arguments and loses the caller’s `this` context.  
It also does not provide cancellation or access to the timer’s result.  
The corrected version preserves both context and arguments while keeping trailing-edge debounce behavior.

## Review

Original code:

```js
function debounce(fn, ms) {
  let t;

  return function() {
    clearTimeout(t);
    t = setTimeout(fn, ms);
  };
}
```

### Issues

1. **Arguments are discarded**

   Calling `debouncedSearch("query")` does not pass `"query"` to `fn`.

2. **`this` is not preserved**

   If `fn` depends on the object it was called on, that context is lost. The callback is invoked later by `setTimeout`, not by the original caller.

3. **The wrapper cannot return `fn`’s result**

   Debounced execution happens asynchronously, so the wrapper cannot synchronously return the eventual result. This is expected behavior, but it should be clear to callers.

4. **No cancellation API**

   Consumers cannot cancel a pending invocation except by calling the debounced function again. A `cancel()` method is often useful when a component is unmounted or a page is navigating away.

5. **Input validation is absent**

   Depending on the intended API, it may be useful to validate that `fn` is callable and normalize the delay. This is optional for a minimal implementation.

## Corrected version

```js
function debounce(fn, ms) {
  if (typeof fn !== "function") {
    throw new TypeError("Expected fn to be a function");
  }

  let timerId;

  function debounced(...args) {
    const context = this;

    clearTimeout(timerId);

    timerId = setTimeout(() => {
      fn.apply(context, args);
    }, ms);
  }

  debounced.cancel = function () {
    clearTimeout(timerId);
    timerId = undefined;
  };

  return debounced;
}
```

Example:

```js
const search = debounce(function (query) {
  console.log(this.name, query);
}, 300);

const controller = {
  name: "SearchController",
  search
};

controller.search("web3");
// Logs: "SearchController web3" after 300 ms,
// assuming no newer call occurs first.

controller.search.cancel();
```

This implementation runs `fn` only after calls have stopped for `ms` milliseconds. Each new call resets the timer, while the latest call’s arguments and `this` value are used.

## Optional enhanced version

If the caller needs to know when the pending invocation is complete, the debounced function can return a promise:

```js
function debounce(fn, ms) {
  if (typeof fn !== "function") {
    throw new TypeError("Expected fn to be a function");
  }

  let timerId;

  return function debounced(...args) {
    const context = this;

    return new Promise((resolve, reject) => {
      clearTimeout(timerId);

      timerId = setTimeout(() => {
        try {
          resolve(fn.apply(context, args));
        } catch (error) {
          reject(error);
        }
      }, ms);
    });
  };
}
```

This version is useful when callers need to await the result, but promises created by calls that are superseded remain pending unless explicit cancellation or rejection behavior is added. For ordinary event handlers, the first corrected version is simpler and generally preferable.

## Notes

- The implementation assumes trailing-edge debounce behavior: `fn` runs once after the final call.
- `ms` is assumed to be a non-negative delay accepted by `setTimeout`.
- The returned function does not synchronously return `fn`’s result because execution is delayed.
- If the debounced function is used with UI components, call `.cancel()` during cleanup to prevent stale work after unmounting.

---

**From the agent:** Delivered by Onyx: the work is below.
