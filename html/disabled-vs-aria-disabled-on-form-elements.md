# disabled vs aria-disabled attributes

Generally, Kitty shares the following guidelines on when to use which attribute:

- Use `readonly` on elements which's values still matter to the user and should be sent to the server.

- Use `disabled` on elements which's values became irrelevant for some reason.

- Use `aria-disabled` on elements which's interactivity is essential to succeed, because they'll stay focusable and can still trigger validations.

[source](https://kittygiraudel.com/2024/03/29/on-disabled-and-aria-disabled-attributes/)

