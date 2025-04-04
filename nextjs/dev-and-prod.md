# Running `dev` and `prod` Server at the Same Time

Both the servers use the same output directory which causes the conflict. Use a custom directory for each environment and you're good to go.

```ts
const nextConfig: NextConfig = {
  distDir: process.env.NODE_ENV === "production" ? ".next" : ".next-dev",
};
```

[source](https://x.com/aidenybai/status/1908210960264823150)
