# Small Programming Tricks

Small, directly usable pieces of knowledge compound. They are cheap to learn, easy to share, and often remove minutes of friction from work that happens every day.

### Search and reuse shell history

`Ctrl-R` searches shell history. Pair it with `fzf` for fuzzy matching, or use a history tool such as Atuin when command history needs to be searchable across machines and directories.

### Use the database as a calculator

`SELECT` does not need a table. It is useful for checking function behaviour and SQL three-valued logic without modifying data.

```sql
select true <> null;
select date_trunc('month', current_timestamp);
```

When investigating a slow query, use `EXPLAIN ANALYZE`, not just `EXPLAIN`: it executes the query and reports the actual row counts and timings. Run it carefully on write queries because it really does execute them.

### Make searches more precise

A regular-expression word boundary, `\b`, matches the edge of a word. It avoids matching an identifier inside a longer word.

```js
/\berror\b/.test('server error') // true
/\berror\b/.test('terror')      // false
```

For repository history, `git log -S <text>` finds commits that added or removed the exact text. `git log -G <pattern>` instead matches diffs that change a line matching a regular expression.

```sh
git log -S 'deprecatedOption' --all
git log -G 'timeout.*5000' --all
```

### Bucket skewed measurements logarithmically

A linear histogram hides a long-tailed distribution. Group counts by powers of ten to see whether values cluster around tens, hundreds, or thousands.

```js
const bucket = Math.floor(Math.log10(userInGroupCount))
metrics.increment('users_per_group', { bucket })
```

Treat zero separately because `Math.log10(0)` is `-Infinity`.

### Prefer built-in primitives before custom helpers

Modern JavaScript already provides useful composition tools such as `Array.prototype.flatMap`, `Object.entries`, and `Promise.withResolvers`. Likewise, shell globs often replace a complex `find` invocation, `rg` is generally a better default than `grep` for recursive source search, and `git checkout -` returns to the previously checked-out revision.

The useful habit is not memorising a fixed list. Share one small, proven trick at a time; the right detail can save someone a debugging session, reveal the relevant documentation, or make an existing tool do exactly what they need.

[source](https://will-keleher.com/posts/small-programming-tricks-matter/)
