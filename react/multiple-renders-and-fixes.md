# Common Reasons of Infinite Loops in React and How to Fix Them

The how and why of `Uncaught Error: Too many re-renders. React limits the number of renders to prevent an infinite loop.`

- **Updating state inside the render**

```JSX
function App() {
  const [count, setCount] = useState(0);
  setCount(1); // infinite loop
  return ...
}
```

If you update the state directly inside your render method or a body of a functional component, it will cause an infinite loop.

*State updates → triggers re-render → state updates → triggers re-render → …*

FIX:

Use useEffect with an empty array as a dependency.

```JSX
function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setCount(1);
  }, [])
  return ...
}
```

- **Infinite loop in UseEffect**

```JSX
function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setCount(count + 1) // infinite loop
  }, [count])
  return ...
}
```

If you keep updating a state inside useEffect with a property you update set as a dependency, it will cause an infinite loop.

*count updates → useEffect detects updated dependency → count updates → useEffect detects updated dependency → …*

FIX:

If you want to update a state based on its previous value, use a functional update. This way, you can remove state property from the dependency list and prevent an infinite loop.

```JSX
function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setCount(previousCount => previousCount + 1)
  }, [])
  return ...
}
```

- **Incorrectly set event handlers**

```JSX
export default function App() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={setCount(1)}>Submit</button> // infinite loop
  );
}
```

It is not the right way to set event handlers. You need to provide a function to the `onClick`, not the result of the function execution. By executing a function before setting a handler, you update a state inside the render, which causes an infinite loop.

*State updates → triggers re-render → state updates → triggers re-render → …*

FIX:

Set a function to `onClick` event. It is a proper way to set event handlers. This way state will only update after a click of a button and won’t cause an infinite loop.

```JSX
export default function App() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(1)}>Submit</button>
  );
}
```

