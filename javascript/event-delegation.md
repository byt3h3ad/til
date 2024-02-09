# Event Delegation in JavaScript

How can we improve the performance of this code?

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
  <!-- ... this is a really long list -->
</ul>

<script>
let listItems = document.querySelectorAll('li');

listItems.forEach(function(item, i) {
  item.addEventListener('click', () => {
    item.style.backgroundColor = 'lightblue';
  });
});
</script>
```

Instead of adding an event listener to _each_ list element, we can use “event delegation” to add a single event listener to the parent element `(ul)` and check if the event target is a list item. This has a few advantages:

_Memory Efficiency:_ Event delegation conserves memory by reducing the number of event listeners, since one parent listener replaces multiple child listeners.

_Handling Dynamic Element:_ Event delegation smoothly manages dynamically added or removed elements, as the parent listener remains consistent, without needing to re-bind.

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
  <!-- ... this is a really long list -->
</ul>

<script>
  let ul = document.querySelector('ul');
  ul.addEventListener('click', (event) => {
    if(event.target.tagName === 'LI') {
      event.target.style.backgroundColor = 'lightblue';
    }
  });
</script>
```

[source](https://bytes.dev/archives/189)
