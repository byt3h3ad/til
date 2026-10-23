# Lazy Pipelines with Iterator Helpers

`items.filter(...).map(...).slice(0, 10)` is eager: each step builds a full intermediate array, even if you only need 10 results.

Iterator helpers (`map`, `filter`, `take`, `drop`, `flatMap`, `find`, `some`, `every`, `reduce`, `toArray`) are lazy. Values are pulled one at a time and the pipeline stops as soon as it can. They live on iterators, not arrays, so get one first with `values()`, `keys()`, `entries()`, or a generator.

```js
const visibleItems = items
  .values()
  .filter(isVisible)
  .map(transform)
  .take(10)
  .toArray();
```

Checked in Node 22 with 100,000 items:

```js
let calls = 0;
items.values()
  .filter(x => { calls++; return x % 2 === 0; })
  .map(x => x * 2)
  .take(3)
  .toArray(); // [0, 4, 8], calls === 5
```

## Gotchas

- Iterators are one-shot: a second `toArray()` on the same iterator returns `[]`.
- Nothing runs until something consumes the iterator (`toArray()`, `for...of`, `reduce`, ...), so side effects can look missing.
- No random access (`items[5]`). Keep arrays for indexing, mutation, or small data.
- Debugging by inspecting the iterator (`[...it]`, `it.toArray()`) consumes it, leaving nothing for the real pipeline. A `console.log` inside a step is fine; it only runs when the iterator is consumed.
- Async generators do not have these helpers yet: `typeof asyncGen().filter` is `undefined` in Node 22. The source article shows `fetchPages().filter(...)`, which does not work today.

Supported in modern browsers and Node 22+.

Source: [Stop turning everything into arrays (and do less work instead)](https://allthingssmitty.com/2026/01/12/stop-turning-everything-into-arrays-and-do-less-work-instead/)
