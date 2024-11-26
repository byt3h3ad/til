# Pipe function in JavaScript

```javascript
const surprise =
  (...fns) =>
  (input) =>
    fns.reduce((acc, fn) => fn(acc), input);
```

A pipe function allows you to chain multiple operations together by taking a series of functions as arguments and applying them in a specific order to the input.

Instead of doing something like this.

```javascript
const toUpperCase = (str) => str.toUpperCase();
const removeSpaces = (str) => str.replace(/\s/g, "");
const addExclamation = (str) => str + "!";

toUpperCase(removeSpaces(addExclamation("Subscribe to Bytes")));
```

You can do something like this.

```javascript
const pipe =
  (...fns) =>
  (input) =>
    fns.reduce((acc, fn) => fn(acc), input);

const formatString = pipe(toUpperCase, removeSpaces, addExclamation);

formatString("Subscribe to Bytes"); // SUBSCRIBETOBYTES!
```

[source](https://bytes.dev/archives/341)
