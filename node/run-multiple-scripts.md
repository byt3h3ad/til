# How to run multiple scripts simultaneously

Use a regex with `pnpm run` to run multiple scripts simultaneously. Ditch `npm-run-all` or `concurrently`.

```javascript
{
    "dev": "pnpm run \"/dev:/\"",
    "dev:tsc": "tsc --watch --preserveWatchOutput",
    "dev:node": "node --watch dist/index.js",
    "dev:esbuild": "pnpm run build --watch"
}
```

`--preserveWatchOutput` prevents the console from being cleared by `tsc --watch` when you're running multiple scripts simultaneously.

[source](https://x.com/mattpocockuk/status/1764614629613867093)

