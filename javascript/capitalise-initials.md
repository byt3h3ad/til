# Capitalise Initials

```cpp
const getCapitalizedInitials = (name) =>
  name
    .trim()
    .split(" ")
    .map((name) => name.charAt(0))
    .join("")
    .toUpperCase()
```

