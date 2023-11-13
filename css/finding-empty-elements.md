# Finding Empty Elements With `:empty`

The `:empty` CSS pseudo-class represents any element that has no children.

AN element is considered 'empty' if there is nothing in between the tags. This nothing, however, does not include whitespace. And if the CSS for the element has generated [content](https://css-tricks.com/almanac/properties/c/content/)  —  as from a pseudo-element like `::before` or `::after`  —  it is also still considered empty.

One has to be careful with pictures and inputs as they have no container contents. A good way of dealing with something like this is to use the `:not` pseudo-class to catch them.

```css
:not(svg, img, picture, input, textarea):empty {
  /* css */
}
```

[docs](https://developer.mozilla.org/en-US/docs/Web/CSS/:empty)

[source](https://youtube.com/shorts/wk79huqm1h4)

There is a new experimental (`:blank`)[https://developer.mozilla.org/en-US/docs/Web/CSS/:blank] pseudo-class that builds upon the `:empty` pseudo-class, but also selects elements that include whitespace and empty user input elements like `<input>` or `<textarea>`. However there is no browser support and `:empty` is being updated to include whitespace. There is a version by Mozilla supported on only Firefox called (`:-moz-only-whitespace`)[https://developer.mozilla.org/en-US/docs/Web/CSS/:-moz-only-whitespace].
