# Remove Unused Code From Your Codebase

One of the few problems that arise out of a big codebase is unused files, exports and dependencies. Knip helps solve this by automatically finding all this code. It also has an [`auto-fix`](https://knip.dev/features/auto-fix) feature which celans up the clutter as well.

The easiest way to get started is by installing Knip:

```bash
pnpm create @knip/config
```

And running it in the project:

```bash
pnpm knip
```

You can go ahead and use Knip with the defaults. However, if false positives pop up you'll need a [config file](https://knip.dev/overview/configuration).

[docs](https://knip.dev/)
