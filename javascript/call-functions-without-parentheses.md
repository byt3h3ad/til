# Call Functions Without Parentheses via prepareStackTrace

V8 calls `Error.prepareStackTrace(error, callSites)` when `error.stack` is read. Its signature matches `Function(arg, body)`, so assigning `Function` there makes `error.stack` return a new function whose body is `String(callSites)`.

Make that string the payload, then trigger the function without `()`:

```js
Object.prototype.message = 'console.log\x281337\x29'; // \x28 = ( , \x29 = )
Array.prototype.toString = Error.prototype.toString;  // array -> "message" string
Error.prepareStackTrace = Function;                   // stack getter builds a function
const x = new Error;                                  // `new` needs no parentheses
x.valueOf = x.stack;                                  // x.stack is the generated function
+x;                                                   // coercion calls valueOf -> logs 1337
```

- `Error.prototype.toString` reads `this.message`. The polluted `Object.prototype.message` supplies the payload, so the call-site array stringifies to it.
- Overwriting `Array.prototype.toString` with a constant function that returns the payload also works.
- `x.toString = x.stack` plus `'' + x` is an alternative trigger to `valueOf`.
- Tested in Node. The original uses `alert`. V8-only (`prepareStackTrace` is not standard).

Source: https://blog.ikaes.de/yet-another-way-to-invoke-functions-without-parentheses/
