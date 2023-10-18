# How to Not Fill Up an `Array`

What gets logged?

```js
const array = new Array(3).fill([]);
array[0].push("bytes");
console.log(array); // [ ["bytes"], ["bytes"], ["bytes"] ]
```

The key to understanding this one is in knowing that arrays in JavaScript are [reference values](https://ui.dev/primitive-vs-reference-values-in-javascript).

When you call `.fill([])`, what you’re really doing is “filling up” the array with three _references_ to the same array. You can kind of think of it like this.

```js
const reference = [];
const array = new Array(3).fill(reference);
```

Where now, `array` has three elements and they’re all referencing the same `reference` array. Therefore, if you add an item to any of the three elements, since they all point to the same array, it’s _as if_ you’re adding an item to all of them.

To get the same functionality without the referential weirdness, you can use `Array.from`.

```js
const array = Array.from({ length: 3 }, () => []);
array[0].push("bytes"); // [ ["bytes"], [], [] ]
```

[source](https://bytes.dev/archives/231)
