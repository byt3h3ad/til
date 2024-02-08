# Remove the Duplicate Elements from the Array of Objects

```js
const list = [
  { name: 'John' }, 
  { name: 'Sara' },
  { name: 'Sara' },
  { name: 'Lynn' },
  { name: 'Jake' }
];
```

There are a few ways to do this. Your first intuition might be to do something like this.

```js
const uniqueList = Array.from(new Set(list))
```

It’s the right idea since creating a `Set` will ensure our collection only contains _unique_ values and `Array.from` allows us to create a new, shallow-copied array. Unfortunately, `Sets` only enforce uniqueness for primitive values, but our list is full of objects.

Instead, we can do something like this.

```js
const map = new Map();

list.forEach(item => {
	map.set(item.name, item)
});

const uniqueList = Array.from(map.values())
```

Notice we’re using a `Map` here. Since `Map`’s preserve insertion order, our `uniqueList` will have the same order as the original list, but without the duplicates since we’re using `name` as the key.

Another fancy approach is using `filter` with `findIndex`,

```js
const uniqueList = list.filter((item, index) => 
  list.findIndex(({name}) => 
    name === item.name
  ) === index
);
```

Or using `.reduce` and `.map`,

```js
const uniqueList = Object.keys(
  list.reduce((acc, cur) => {
    acc[cur.name] = cur.name;
    return acc;
  }, {}),
).map((name) => ({ name }));
```

[source](https://bytes.dev/archives/247)
