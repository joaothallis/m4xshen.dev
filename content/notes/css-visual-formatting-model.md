---
title: "CSS Visual Formatting Model"
date: 2023-04-03T10:36:57+08:00
draft: true
---

Visual formatting model defines how user agents process the document tree for visual media.

## Boxes model

CSS uses DOM to create box tree in two steps:
1. Assign a computed value for each CSS property to each element and text node in the DOM.
2. Generate zero or more boxes as specified by that element's `display` property.

Boxes are often referred to by their display type—e.g. a box generated by an element with `display: block` is called a “block box” or just a “block”.

### Principal box

Typically, an element generates a single box, the principal box, which represents itself and contains its contents in the box tree.

### Anonymous boxes

#### Anonymous block boxes

```html
<div>
  anonymous block box
  <p>some text</p>
  anonymous block box
</div>
```

#### Anonymous inline boxes

```html
<p>anonymous inline box <em>emphasized</em> anonymous inline box</p>
```

#### Properties of anonymous boxes

The properties of anonymous boxes are inherited from the enclosing non-anonymous box. Non-inherited properties have their initial value.

Anonymous block boxes are ignored when resolving percentage values that would refer to it: the closest non-anonymous ancestor box is used instead.

## Box layout modes

The display property defines an element’s display type, which consists of the two basic qualities of how an element generates boxes: 
- inner display type: dictates how its descendant boxes are laid out
- outer display type: dictates how the principal box itself participates in flow layout

The `display` property has no effect on an element's semantics.

### Values of display

```text
[ <display-outside> || <display-inside> ] | <display-box> | <display-legacy>
```

- \<display-outside\>  = block | inline
- \<display-inside\>   = flow | flow-root | table | flex | grid
- \<display-box\>      = contents | none
- \<display-legacy\>   = inline-block | inline-table | inline-flex | inline-grid

The principal box’s inner display type defaults to `flow`. The principal box’s outer display type defaults to `block`.

### \<display-outside\>

- `block`: The element generates a box that is block-level when placed in flow layout.
- `inline`: The element generates a box that is inline-level when placed in flow layout.

### \<display-inside\>

- `flow`: The element lays out its contents using flow layout.
- `flow-root`: The element generates a block container box, and lays out its contents using flow layout. It always establishes a new block formatting context for its contents.
- `table` The element generates a principal table wrapper box that establishes a block formatting context, and which contains an additionally-generated table grid box that establishes a table formatting context.
- `flex`: The element generates a principal flex container box and establishes a flex formatting context. 
- `grid`: The element generates a principal grid container box, and establishes a grid formatting context. 

### \<display-box\>

- `contents`: The element itself does not generate any boxes. The element must be treated as if it had been replaced in the element tree by its contents.
- `none`: The element and its descendants generate no boxes or text sequences.

### \<display-legacy\>

- `inline-block` = `inline flow-root`
- `inline-table` = `inline table`
- `inline-flex` = `inline flex`
- `inline-grid` = `inline grid`

### Automatic box type

- Absolute positioning or floating an element blockifies the box’s display type.
- A parent with a `grid` or `flex` display value blockifies the box’s display type.
- The root element’s display type is always blockified, and its principal box always establishes an independent formatting context.

## Positioning schemes

The `position` and `float` properties determine which of the positioning algorithms is used to calculate the position of a box. 

### Values of position

```text
static | relative | absolute | fixed
```

### static

The box is a normal box, laid out according to the normal flow. The `top`, `right`, `bottom`, and `left` properties do not apply.

### relative

The box's position is calculated according to the normal flow. Then the box is offset relative to its normal position.

### absolute

The box's position is specified with the `top`, `right`, `bottom`, and `left` properties. These properties specify offsets with respect to the box's containing block.

### fixed

The box's position is calculated according to the `absolute` model, but in addition, the box is fixed with respect to the viewport and does not move when scrolled. 

## Normal flow