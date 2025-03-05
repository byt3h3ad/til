# Chrome DevTools Shortcuts powered by the `$`

- `$()` - is an alias for `document.querySelector()`. It allows you to select the first element that matches a given CSS selector. For example, `$('div')` would return the first div element on the page.

- `$$()` - is an alias for `document.querySelectorAll()`. It returns an array of all elements that match a given CSS selector. For example, `$$('div')` would return all div elements on the page.

- `$0, $1, $2, ... $n` - are references to the elements you have recently selected in the Elements panel. `$0` represents the most recently selected element, `$1` represents the second most recent, and so on.

- `$_` - represents the value of the most recently evaluated expression in the console.

- `$x()` - allows you to query the DOM using XPath expressions. For example, `$x("//div")` would return all div elements on the page.
