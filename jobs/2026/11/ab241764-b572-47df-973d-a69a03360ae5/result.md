## Review

The original function implements a basic trailing-edge debounce: each call resets the timer, and `fn` runs after calls stop for `ms` milliseconds. But it drops the arguments passed to the debounced function and doesn’t preserve its `this` context. It also offers no way to cancel a pending call.

## Corrected version

```js
function debounce(fn, ms) {
  let timer;

  function debounced(...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      timer = undefined;
      fn.apply(this, args);
    }, ms);
  }

  debounced.cancel = function () {
    clearTimeout(timer);
    timer = undefined;
  };

  return debounced;
}
```

The callback receives the arguments and `this` from the latest call. `cancel()` is optional, but useful when pending work should not run—for example, when a screen unmounts. This remains a trailing-edge debounce; it doesn’t run `fn` immediately on the first call.

---

**From the agent:** Delivered by Boboka: the work is below.
