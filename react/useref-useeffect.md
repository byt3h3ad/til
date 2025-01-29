# Use callback ref functions instead of useEffect (when possible)

When working with React, you might encounter scenarios where you need to interact with DOM elements, such as implementing a "scroll to bottom" button. The traditional approach involves using a `ref` and wrapping it in a `useEffect` hook to trigger the action on mount. However, there's a simpler and cleaner way: **callback refs**.

### The Traditional Approach: `useEffect` with `ref`

Here’s how you might traditionally implement a scroll-to-bottom feature:

```jsx
import React, { useEffect, useRef } from "react";

export const App = () => {
  const bottomAnchor = useRef < HTMLDivElement > null;
  useEffect(() => {
    if (bottomAnchor.current) {
      bottomAnchor.current.scrollIntoView({ behavior: "smooth" });
    }
  }, []);
  return (
    <div>
      App
      <div ref={bottomAnchor} />
    </div>
  );
};
```

While this works, it introduces unnecessary complexity. The `useEffect` hook is used solely to trigger the scroll action when the component mounts, which feels like overkill for this simple task.

### The Cleaner Solution: Callback Refs

Callback refs provide a more elegant solution. They are automatically called when the element is mounted, eliminating the need for `useEffect`. Here's how you can rewrite the above example using a callback ref:

```jsx
import React from "react";

export const App = () => {
  const scrollIntoView = (node: HTMLDivElement) => {
    node.scrollIntoView({ behavior: "smooth" });
  };
  return (
    <div>
      App
      <div ref={scrollIntoView} />
    </div>
  );
};
```

Simple, clean.

### When to Use Callback Refs

Callback refs are ideal for simple DOM interactions that don’t require side effects or updates based on state/props changes. For more complex scenarios, `useEffect` might still be necessary. However, for cases like scrolling, focusing, or measuring elements, callback refs are often the better choice.

[source](https://x.com/_georgemoller/status/1883847755430306090)
