# The English Alphabet as a String

```javascript
Array.from({ length: 26 }, (x, i) => (i + 10).toString(36)).join(", ");
```
