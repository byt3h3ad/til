# A Better Number() and Number.parseFloat()

```typescript
const numberSafeParse = (input: string): number | null => {
    if (input.trim() === "") return null;
    let parsed = Number(input);
    if (!Number.isFinite(parsed)) return null;
    return parsed;
};
```

- Returns `null` instead of `NaN` on failure (with matching type).

- Doesn't parse things like "42foo" as `42` (vs using `Number.parseFloat()`)

- Doesn't parse whitespace-only things as `0` (vs using `Number()`)

- Doesn't parse "Nan" into `Nan` or "Infinity" into `Infinity` (vs `Number()` or `Number.parseFloat()`)

- Only parses token sequences that are **exactly** valid JS numbers

[source](https://x.com/buildsghost/status/1766273406608294298)

