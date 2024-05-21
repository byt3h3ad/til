# Using useState for one-time initialisations in React Components

Suppose you want to initialise something only once in a React component. You might think of initialising it outside the component using the `const` keyword. However, the problem is that it will keep creating a new instance with every render. 

As a solution to this, we might think of using `useMemo`. After all, `useMemo` is for only re-computing values if dependencies change, and we don't have a dependency here. Again, this might coincidentally work for some time, but let's have a look at what the [react docs](https://react.dev/reference/react/useMemo) have to say about useMemo:

> You may rely on useMemo as a performance optimization, not as a semantic guarantee. In the future, React may choose to “forget” some previously memoized values and recalculate them on next render, e.g. to free memory for offscreen components. Write your code so that it still works without useMemo — and then add it to optimize performance.

The best solution? Call useState without a setter. State is guaranteed to only update if you call the setter. So all we need to do is not call the setter, and since it's the second part of the returned tuple, we can just not destruct it. We can even combine this very well with the lazy initializer to make sure the resource constructor is only invoked once -

```js
const Component = () => {
  // ✅ truly stable
  const [resource] = React.useState(() => new Resource())
  return (
    <ResourceProvider resource={resource}>
      <App />
    </ResourceProvider>
  )
}
```

> When we pass an initial value to useState, the initial variable is always created, but React will only use it for the first render. This is totally irrelevant for most use cases, e.g. when you pass a string as initial value. In rare cases, we have to do a complex calculation to initialize our state. For these situations, we can pass a function as initial value to useState. React will only invoke this function when it really needs the result (= when the component mounts):
> ```js
> // 🚨 will unnecessarily be computed on every render
> const [value, setValue] = React.useState(
>  calculateExpensiveInitialValue(props)
> )
> 
> // ✅ looks like a small difference, but the function is only called once
> const [value, setValue] = React.useState(() =>
>  calculateExpensiveInitialValue(props)
> )
> ```

[source](https://x.com/housecor/status/1792526262751117371)

[further reading](https://tkdodo.eu/blog/use-state-for-one-time-initializations)

[why not to use useRef](https://x.com/TkDodo/status/1792549972539273278)
