# How to get query strings in Astro

You can get the query string using [`Astro.url`](https://docs.astro.build/en/reference/api-reference/#astrourl) -

```js
const token = Astro.url.searchParams.get("access_token") || "";
```

However, Astro builds a static site by default. So this cannot access the search params. Only a server can see the search params, since they are passed by the user when the user makes a request, and static sites are built ahead of time without knowing what search params a user might send.

Use [SSR](https://docs.astro.build/en/guides/server-side-rendering/) by changing `output: 'server'` in the config file or set `export const prerender = false;` on top of the page or route.
