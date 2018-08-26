# CSS Cheatsheet

## Table of Contents

1. [Basics](#basics)
1. [CSS Specificity & Inheritance](#css-specificity-&-inheritance)
1. [CSS Combinators](#css-combinators)

## Basics

[back to top](#table-of-contents)

Three ways to write css:

- inline styles
  - apply style inline to html
  - not recommended
    - hard to read, not maintainable

```html
<section style="background: #ff1b68">
  <h1>Get the freedom you deserve.</h1>
</section>
```

- selectors
  - add style tag to head
  - use selector to decide to which element to apply style

```html
<style>
  section {
    background: #ff1b68;
  }
</style>
```

- external stylesheet (recommended)
  - css rule consists of selector, property, and value
  - use link tag to source stylehseet
  - note that browser caches stylesheet if used in multiple pages

**Fonts:**

Browsers have default fonts for sans-serif, serif, and monospace font-family properties.

Note: Use monospace for displaying code.

See [chrome://settings/fonts](chrome://settings/fonts) for an example.

Sometimes you want a specific font that a browser doesn't have installed.

Can use Google Fonts for this: [https://fonts.google.com/](https://fonts.google.com/)

**IDs:**

Not just used for styling. If ID name included in url after a hash, browser will jump to that ID in the html.

**Classes:**

CSS is case insensitive. Recommended to use kebab-case when naming CSS classes / IDs.

## CSS Specificity & Inheritance

[back to top](#table-of-contents)

1. Multiple rules affect the same element.

2. Different rules have different priorities.

So it's not necessarily the case that styles will be applied in order from top to bottom.

It's more like you construct the styles for specific elements based on priority and order.

Note that inline styles have highest priority. Element tags and pseudo-element selectors have lowest priority.

Order of Specificity / Priority:

1. Inline styles
1. #ID selectors
1. .class, :pseudo-class and [attribute] selectors
1. Element tag and ::pseudo-element selectors

If you inspect styles in chrome, styles that have been over-written are stricken through.

Cascading means multiple rules can be applied to the same element. Rules are applied based on their specificity i.e. specificity _resolves conflicts_ arising from multiple rules.

Note that parent styles are inherited by child elements if not overwritten. Inheritance has very low specificity.

If want rest of page to inherit a rule, often good idea to put in body tag. Good choice for properties like font-size, font-families. Keep in mind, any direct selector has higher specificity than inheritance and will overwrite inherited properties.

Rule with more information wins over rules with less information. Combinators are examples of rules with more info.

## CSS Combinators

[back to top](#table-of-contents)

Adjacent sibling:

```css
div + p {
  color: red;
}
```

```html
<div>
  <h2>Not applied</h2>
  <p>CSS applied</p>
  <h2>Not applied</h2>
  <h3>Not applied</h3>
  <p>Not applied</p>
  <h2>Not applied</h2>
  <p>CSS applied</p>
</div>
```

- Elements share the same parent.
- Second element comes **immediately** after first element.

General sibling:

```css
div ~ p {
  color: red;
}
```

```html
<div>
  <h2>Not applied</h2>
  <p>CSS applied</p>
  <h2>Not applied</h2>
  <h3>Not applied</h3>
  <p>CSS applied</p>
</div>
```

- Elements share the same parent.
- Second element comes **generally** after the first element.

Child:

```css
div > p {
  color: red;
}
```

```html
<div>
  <div>Not applied</div>
  <p>CSS applied</p>
  <div>Not applied</div>
  <article>
    <p>Not applied</p>
  </article>
  <p>CSS applied</p>
</div>
```

- Second element is a **direct** child of first element.

Descendant:

```css
div p {
  color: red;
}
```

```html
<div>
  <div>Not applied</div>
  <p>CSS applied</p>
  <div>Not applied</div>
  <article>
    <p>CSS applied</p>
  </article>
  <p>CSS applied</p>
</div>
```

- Second element is a **descendant** (however deep / nested) of the first element.

Direct selectors without combinators are more performant. That said, using a combinator doesn't automatically mean performance is bad. For example, if using an ID as a selector then should perform well, aren't many IDs on the page.
