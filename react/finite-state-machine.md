# Simplify Your Components with Finite State Machines and Derived States

Consider a common situation of a React component with multiple UI states such as `loading`, `error`, and `success`. The common paatern is to use multiple `useState` hooks to manage these states. This results in code that is hard to read and error-prone -

```js
const MyComponent = () => {
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(false)
  const [success, setSuccess] = useState(false)

  return (
    <div>
      {loading && !error && !success && <p>Loading...</p>}
      {error && !loading && !success && <p>Error occurred</p>}
      {success && !loading && !error && <p>Operation completed successfully</p>}
    </div>
  )
}
```

These states are **distinct from each other**. When `loading` is true, the `error` and `success` states should be false. Using multiple useState hooks can cause unexpected behaviors, like accidentally setting two states to true simultaneously.

Instead, consider using the ["finite state machine" (FSM) pattern](https://en.wikipedia.org/wiki/Finite-state_machine). A FSM allows only a finite number of states. In the UI example above, a single useState can manage the current state more robustly and with less risk of error, as shown here:

```js
import { useState } from 'react'

type State = 'loading' | 'error' | 'success'

const MyComponent = () => {
  const [state, setState] = useState<State>('loading')

  const handleClick = () => {
    setState('loading')
    // Simulate an async operation
    setTimeout(() => {
      setState('success')
    }, 2000)
  }

  return (
    <div>
      {state === 'loading' && <p>Loading...</p>}
      {state === 'error' && <p>Error occurred</p>}
      {state === 'success' && <p>Operation completed successfully</p>}
      <button onClick={handleClick}>Click me</button>
    </div>
  )
}
```

An even better way would be to use something like [Tanstack Query](https://tanstack.com/query/latest/docs/framework/react/overview) to fetch data. `useQuery` eliminates the need for separate `useState` hooks for `loading`, `error`, and `success` states.

[source](https://www.nico.fyi/blog/you-dont-need-usestate-in-react)
