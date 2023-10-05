# Tailwind divide to add border between

I found out that tailwind has a few [divide](https://tailwindcss.com/docs/divide-width#add-borders-between-horizontal-children) classes that helps to add border between elements. So if you want to add a separator border for your children you can try:

```html
<div class="grid grid-cols-1 divide-y">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

And that will add horizontal borders between those nodes.

![image](https://i.imgur.com/bYuKLmd.png)

viniciusnegrisolo

June 29, 2023