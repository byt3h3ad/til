# Quickly Find the Element That Is Causing Horizontal Scrolling

```js
const documentWidth = document.documentElement.offsetWidth;

document.querySelectorAll("*").forEach((node) => {
  if (node.offsetWidth > documentWidth) {
    console.log(node);
  }
});
```

The snippet logs all elements that are wider than the document's width.

[source](https://pham.codes/blog/quickly-find-what-is-causing-horizontal-scrolling)
