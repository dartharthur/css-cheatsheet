# CSS Cheatsheet

## Table of Contents

1. [Basics](#basics)
1. [CSS Specificity & Inheritance](#css-specificity-&-inheritance)
1. [CSS Combinators](#css-combinators)
1. [Box Model](#box-model)
1. [Margin Collapsing](#margin-collapsing)
1. [Height and Width](#height-and-width)
1. [Display Property](#display-property)
1. [Pseudo-classes and Pseudo-elements](#pseudo-classes-and-pseudo-elements)
1. [Classes and IDs](#classes-and-ids)
1. [!important](#!important)
1. [float](#float)
1. [Positioning](#positioning)
1. [Stacking Context](#stacking-context)
1. [Background Images](#background-images)

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

## Box Model

[back to top](#table-of-contents)

Every element in HTML is interpreted as a box by CSS.

Every element has:

1. content - what's inside the element
1. padding - internal space between content and border
1. border - boundary between padding and margin (can take up space)
1. margin - external space between element and next sibling

Note that all elements have a default margin, even the body. If want to remove body's default margin, can set to 0 so elements take up entirety of page.

## Margin Collapsing

[back to top](#table-of-contents)

If two block elements are next to each other, margins between them are collapsed into one margin, the bigger margin wins. This is a CSS feature that ensures distances between objects don't get too big.

To work around this, good practice to use either `margin-top` or `margin-bottom`.

## Height and Width

[back to top](#table-of-contents)

Block level elements take full available width of surrounding container by default.

Available height isn't determined by height of page. It's determined by height of the container and / or the size of the container's content. In other words, elements are only as "tall" as they need to be.

_If ever want to style height of element relative to the height of the page, need to create a chain where you pass the page height down from the html tag, to body tag, to element tag (set them all to 100%)._

When setting the height and width, you are setting the height and width of the element **content** by default. Padding and border are not included in the calculation. They aren't part of what width and height target.

**Key property here is `box-sizing`.** This property defines how the width and height of an element are calculated.

The default is `content-box` which doesn't take padding and border into account.

`border-box` will take padding and border into account when calculating height and width.

**Common practice to overwrite styling for all elements to use `box-sizing: border-box` because it's more convenient to think of entire box instead of just the content.**

Since this practice is so common, this is the rare case where it's acceptable to use the `*` selector to overwrite all styles. Since browser by default overwrites block level elements with `box-sizing: content-box`, just adding the property to the body and attempting to set it via inheritance won't work.

```css
* {
  box-sizing: border-box;
}
```

## Display Property

[back to top](#table-of-contents)

Allows us to change display behavior of an element.

In HTML, have inline and block level elements.

Block-level elements are rendered as a block and hence take up all the available horizontal space. Two block-level elements will render in two different lines.

Inline elements don't take entire available width of container. They only take up the space they require to fit their content in. Two inline-elements will fit into the same line (as long as the combined content doesn't take up the entire space, in which case a line break would be added).

Both follow the Box Model, but since inline elements don't take a new line, i.e. they don't flow with the document, one consequence is that you margin-top and margin-bottom have no effect on the element. Another consequence is that setting a width or height has no effect. The width and height automatically take up as much space as required by the content.

Changing inline to block and vice-versa isn't useful in many cases since elements are meant to behave a certain way.

Setting display to `inline-block` can be useful, however. You can style elements like block level elements, but allow them to sit next to each other like inline elements. Note that flexbox is a better solution, but this is good to understand.

`display: none` takes element out of document flow. It doesn't remove it from the DOM, it's just not visible. It also doesn't "block its position" so other elements can (and will) take its place instead. This is useful for something like creating a modal that is there but can't be seen.

Note that if you want to hide an element but you want to keep its place (i.e. other elements don't fill the empty spot), you can use: `visibility: hidden;`. As before, the element is not removed from the DOM but it is also **not** removed from the document flow.

**Important**: In editor, empty whitespace in HTML is added as extra inline element. This can lead to unexpected behavior with positioning of inline-block level elements.

## Pseudo-classes and Pseudo-elements

[back to top](#table-of-contents)

Pseudoclasses (:class name) - define the style of a _**special state**_ of an element.

Pseudoelements (::element name) - define the style of a _**special part**_ of an element.

## Classes and IDs

[back to top](#table-of-contents)

### IDs

Only used once per page. Use it to identify elements that there will only be a single instance of on a page. Also have non-CSS meaning (e.g. on-page link).

Using an ID just for a style is not recommended. Use if available anyways i.e. it semantically makes sense to use an ID.

### Classes

CSS is case insensitive. Recommended to use kebab-case when naming CSS classes / IDs.

Can use multiple classes on one element. Order in which you set classes in HTML doesn't matter. Order in which rules are defined in CSS determines what rules get applied.

Advantages:

- Re-usable
- Allow you to "mark" and name things for styling purposes only

Rarely wrong to use classes for styling. It's the most-used selector type.

Only use tag selector for generic rules that you really want to apply to all of one type of element.

## !important

[back to top](#table-of-contents)

Overrides specificity and all other selectors.

In general: **don't use !important**. Better to make use of specificity and rules.

## float

[back to top](#table-of-contents)

This is the old school way of positioning elements on a page.

Not used anymore in this way as it messes with the document flow and leads to hacky solutions to fix the flow.

Modern tools for positioning elements on page include Flexbox and CSS Grid.

That said, float is still useful for positioning / floating text around an image.

## Positioning

[back to top](#table-of-contents)

Positioning theory: [https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Positioning](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Positioning)

Position property reference: [https://developer.mozilla.org/en-US/docs/Web/CSS/position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

Position property value defaults to `static` which keeps elements in the document flow.

Other values are:

- absolute
- relative
- fixed
- sticky<sup>\*</sup> (experimental feature currently)

Can tell element to move in four different directions:

- top
- right
- bottom
- left

These properties refer to the position in the document flow relative to the positioning context.

Tips & Tricks:

- Adding a fixed navigation bar is usually a good practice.
  - As soon as you add margin to html or body element, whichever is the parent of the navigation bar, need to add top and left properties to ensure nav bar fits correctly into top of page.
- Adding a `z-index` to elements that don't have a position property applied (something other than `static`), has no effect.
- If you want to prevent content from accidentally being positioned outside of a parent / ancestor container, you can add the `overflow: hidden` property to the parent / ancestor.
  - Note that if trying to apply this property to the `body` element then you must also apply it to the `html` element because the default CSS behavior is to pass the property from `body` to `html`. Adding it in both places gives the desired behavior.
- When adding a position property other than `static`, each element gets its own stacking context. For elements **inside** an element (child elements), the `z-index` will only have an impact on the child elements **inside** the parent element.

### fixed

Removes element from document flow. Element width will default to width of content. Behavior changes from block level element to inline-block level. The viewport / window, not the html element, becomes the positioning context i.e. the positioning of the element is relative to only the viewport. This type of positioning can be applied to both block level and inline elements.

TL;DR - Positioning context is always the viewport.

### absolute

"Removes element from document flow. It is positioned relative to it's closest positioned ancestor, if any; otherwise, it is placed relative to the initial containing block (most often this is the content area of an element's nearest block-level ancestor)." - [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

TL;DR - Positioning context depends.

- If you **don't have** any ancestors with a positioning property applied, then the positioning context is the `html` element.
- If you **have** ancestors with a positioning property applied, then the absolutely positioned element will be positioned in relation to the closest ancestor that has a position property applied.

### relative

Element is positioned according to normal document flow. If want to set an absolute position on an element, good idea to set a relative position on an ancestor element. This will allow positioning of the child element relative to the ancestor.

Setting top, left, right, or bottom will move the element relative to **itself** i.e. the positioning context is the element **itself**.

### sticky

A hybrid of relative and fixed. Fixed relative to the viewport (depending on top, bottom, left, right value supplied) until the parent container is out of view.

## Stacking Context

[back to top](#table-of-contents)

Reference: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

When you have two elements with the same `z-index` in the same stacking context, the element that comes later gets stacked on top.

Note that position fixed automatically creates a new stacking context.

A new stacking context is created for elements set to position relative or absolute when a `z-index` is specified/applied to that element. Otherwise it remains in the enclosing stacking context.

## Background Images

[back to top](#table-of-contents)

- background --> background-image (shorthand default)
- `background-size: cover` --> ensures image always fills container, may crop image, maintains aspect-ratio
- `background-size: contain` --> ensures entire image fits container, doesn't crop, may be white space
- `background-position` -- _good to understand_
  - set initial position, relative to background position layer (set by `background-origin`)
  - default is `background-position: 50% 50%` which effectively centers the image
  - ex: `background-position: left 10% bottom 20%`
    - this would ensure that only 20% is cropped from the bottom of the image and 80% is cropped from the top
- `background-origin`
  - default is `padding-box`, not `content-box` or `border-box`
- `background-attachment` not used often

```css
/* shorthand demo */
#overview {
  background-image: url("freedom.jpg");
  background-size: cover;
  background-position: left 10% bottom 20%;
  background-repeat: no-repeat;
  background-origin: border-box;
  background-clip: border-box;
}

#overview {
  background: url("freedom.jpg") left 10% bottom 20%/cover no-repeat border-box;
}
```

- note that setting width and height on element (the container) that CONTAINS an `img` tag doesn't do anything if the container is an `inline` level element because the default behavior is for the `img` tag to take up as much width and height as the source image
  - setting height to 100% on `img` also doesn't work if container is `inline` element, uses original `img` height
  - if `block` level element, setting percentage or height works

best practices:
- if have an actual background image, fine to put in a `div` and set as `background-image`
- if have an image that belongs in document flow, best to use an `img` tag, better for accessibility
- ensure the container of an `img` is `inline-block` or `block` level so that you can control the width and/or height of an `img`

tips:
- if container is `inline-block` and you give it a `box-shadow`, there will be white space between `img` and bottom of container
  - can solve by either:
    - adding `vertical-align: top` to `img` class
    - making container `block` instead of `inline-block`
