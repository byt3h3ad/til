# Debug Next.js Fetch Requests

Make the following change in the `next.config.js` file -

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
    logging: {
        fetches: {
              fullUrl: true,
        },
    },
};

module.exports = nextConfig;
```

This will log the requests in the console when running the app in development mode.

[source](https://x.com/asidorenko_/status/1791537169468698965)

[docs](https://nextjs.org/docs/app/api-reference/next-config-js/logging)
