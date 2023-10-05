# !important classes in tailwind

If you need to add `!important` to one of your Tailwind classes, you simply need to add `!` before the class name.

Here is a _crude_ example:

#### Blue Background

```html
<div class="bg-red-500 bg-blue-500">
  <!-- Insert content here -->
</div>
```

#### Red Background

```html
<div class="!bg-red-500 bg-blue-500">
  <!-- Insert content here -->
</div>
```

**When using this with responsive breakpoints, place it _after_ the _variant modifier_. **

#### Valid

```html
<div class="md:!bg-red-500">
  <!-- Insert conent here -->
</div>
```

#### Invalid

```html
<div class="!md:bg-red-500">
  <!-- Insert content here -->
</div>
```
