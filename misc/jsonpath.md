# JSONPath Basics

JSONPath is a query language for extracting values from JSON without walking the structure by hand. Think XPath for JSON.

Given this API response:

```json
{
  "store": {
    "books": [
      { "title": "1984", "author": "George Orwell", "price": 15 },
      { "title": "Dune", "author": "Frank Herbert", "price": 25 },
      { "title": "Atomic Habits", "author": "James Clear", "price": 20 }
    ],
    "location": "Bangalore"
  }
}
```

## Syntax

| Expression | Meaning                      |
| ---------- | ---------------------------- |
| `$`        | Root of the document         |
| `.key`     | Child property               |
| `['key']`  | Property by name             |
| `[0]`      | Array element by index       |
| `[*]`      | All elements of an array     |
| `..`       | Recursive descent            |
| `[?expr]`  | Filter by condition          |
| `@`        | Current item inside a filter |

`$` is always the document root; `@` is the item the filter is currently testing. RFC 9535 does not require parentheses around the filter expression, so `[?(@.price < 20)]` and `[?@.price < 20]` are equivalent.

## Examples

A query returns a list of matches:

| Query                                  | Result                                |
| -------------------------------------- | ------------------------------------- |
| `$.store.location`                     | `["Bangalore"]`                       |
| `$.store.books[0].title`               | `["1984"]`                            |
| `$.store.books[*].title`               | `["1984", "Dune", "Atomic Habits"]`   |
| `$..price`                             | `[15, 25, 20]`                        |
| `$.store.books[?(@.price < 20)].title` | `["1984"]`                            |

## JSONPath vs `jq`

Both query JSON, but they are different languages, and `jq` can also transform data.

```sh
# JSONPath expression
$.store.books[*].title

# jq equivalent
jq '.store.books[].title' data.json
```

## Where it shows up

- API testing: assert on specific response fields.
- Kubernetes: `kubectl get pods -o jsonpath='{.items[*].metadata.name}'`.
- Automation: pull values out of webhook payloads.

## Standardization

RFC 9535 (February 2024) standardizes JSONPath, but implementations still differ. Kubernetes adds its own extensions, and libraries vary on filters and functions. Check your tool's docs.

Source: [RFC 9535, JSONPath: Query Expressions for JSON](https://www.rfc-editor.org/rfc/rfc9535.html)
