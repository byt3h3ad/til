# Difference between `@tailwind` and `@import tailwind/`

While contributing in the [nodejs.org](https://github.com/nodejs/nodejs.org/) repository, the imported css file styles were not getting applied. It was covered in the Tailwind Docs [here](https://tailwindcss.com/docs/using-with-preprocessors#build-time-imports).

The solution was to import the directives, from the `node_modules`.

[@ovflowd](https://github.com/ovflowd) [explained](https://github.com/nodejs/nodejs.org/pull/5957#discussion_r1344718097) the difference as:

> @tailwind hooks directly into PostCSS engine, as @tailwind is a custom PostCSS plugin, this usually has faster loading and building of CSS and has some optimizations. But for us at least the performance impact isn't noticeable. The regular import also imports the whole Tailwind styles instead of invoking Tailwind's plugin to simply add what we need (it prescans the codebase); The Tailwind PostCSS plugin will still do transformations and afterwards remove what we don't use (with regular imports).

TIL - October 4th 2023.
