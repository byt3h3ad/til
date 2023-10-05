# Sort array of numbers

In javascript, the `Array` `sort` function will cast everything to a string. So when you have an array of numbers, you need to repetitively cast them to numbers:

```ts
[1080, 720, 480].sort()
=> [1080, 480, 720]
```

So you need have to write your own sort function (don't mess it up!)

```ts
[1080, 720, 480].sort((a, b) => Number(a) - Number(b))
=> [480, 720, 1080]
```
