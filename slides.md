---
theme: seriph
title: Z-Index and Stacking Contexts in CSS
class: text-center
mdc: true
fonts:
  # basically the text
  sans: Robot
  # use with `font-serif` css class from UnoCSS
  serif: Robot Slab
  # for code blocks, inline code, etc.
  mono: Fira Code
background: https://img.freepik.com/premium-photo/powerpoint-presentation-gradient-with-some-effects-light-blue-dark-blue_555090-56528.jpg
---

# Z-Index and Stacking Contexts in CSS

## A new approach to understanding CSS stacking

---

class: text-left
level: 3

---

# What is Z-Index?

<br>

Z-Index is a CSS property that controls the vertical stacking order of elements that <span class="underline underline-offset-2">overlap</span>.
The higher the z-index value, the closer the element will appear to the user.

<div class="container">
  <div class="blue">z-index 1</div>
  <div class="red"> z-index 2</div>
  <div class="green">z-index 3</div>
  <div class="yellow">z-index 4</div>
</div>

<style>
.container > div {
  width: 110px;
  height: 110px;
  color: white;
  margin: auto;
  position: relative;
  text-align: center;
}

.red {
  background: red;
  top: 30px;
  left: 30px;
  z-index: 2;
}

.blue {
  background: blue;
  top: 100px;
}

.green {
  background: green;
  top: -30px;
  left: 60px;
  z-index: 3;
}

.yellow {
  background: yellow;
  left: 80px;
  top: -100px;
  z-index: 4;
  color: black !important;
}
</style>

---

# But First: how does the browser position elements?

<br />
The browser uses a layout algorithm to determine how elements are positioned on the page.

Common layout types include:

1. Positioned (e.g., relative, absolute)
2. Flexbox
3. Grid
4. Table
5. Flow

By default, the browser uses the Flow Layout, which positions elements in the order they appear in the HTML document—from top to bottom.

---

# How does z-index work?

<br />

Z-Index works by assigning a numerical value to an element, which determines its position in the stacking order relative to other elements on the page.
Z-index is not implemented in all the layout algorithms, but it is used in the following:

1. Positioned
2. FlexBox
3. Grid

<div class="mt-8">

::code-group

```html [HTML ~i-vscode-icons:file-type-html~]
<div class="box">BOX 1</div>
```

```css [CSS ~i-vscode-icons:file-type-css~]
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  position: relative;
  z-index: 1; /* This will work */
}
```

::

</div>

---

# The Problem with Z-Index

<br />

But there's a problem that many developers face when working with z-index

<br />

[CodePen](https://codepen.io/AndreaMargutti/pen/QwbwaoW?editors=1100)

---

level: 3
layout: image-right
background-size: contain
image: https://www.joshwcomeau.com/_next/image/?url=%2Fimages%2Fstacking-contexts%2Fphotoshop-groups.png&w=640&q=75

---

# Stacking Contexts

<br />

_Stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered._

---

# What creates a stacking context?

<br />

1. Root element of the document.
2. Element with a position value other thant static (with relative and absolute we need z-index != auto).
3. Element with a container-type value size or inline-size set (See container queries).
4. Element that is a flex / grid item with a z-index value other than auto.
5. Setting opacity to a value less than 1
6. Applying a mix-blend-mode other than normal
7. Using transform, filter, clip-path, or perspective
8. Element with the isolation value isolate.

---

# And so...

<br />

[CodePen](https://codepen.io/AndreaMargutti/pen/KwpwZjd)

<br />

<div class="mt-8">
This example is now working as expected because by removing the `position: absolute`from the `.red`div, we are
no longer creating a new stacking context, and the z-index values are now applied correctly.
</div>

---

# This relationship is not a One to One

<br />

It's important to note that a stacking context is not a one-to-one relationship with z-index. An element can create a stacking context without having a z-index value set, and in
the opposite way an element can have a z-index value set without creating a stacking context. **They DO NOT always go together**.

---

# Isolation Property

<br />

Traditionally, we might create a stacking context by setting a non-static position (e.g., relative) and assigning a z-index. But this approach has limitations:
- It requires changing the element’s positioning.
- Choosing a fixed z-index (like 1) introduces assumptions that may not hold across different contexts.
- It risks unintended stacking behavior when components are reused in complex layouts.

Instead, we can use the CSS isolation property. It does exactly one thing: it creates a new stacking context—cleanly and predictably.

```css
.element {
  isolation: isolate; /* Creates a new stacking context */
}
```

---

# A new Approach to z-index: Local vs Global

<br />

With this new approach, we divide z-index usage into two categories:

- Global z-index values refer to the "root" stacking context (typically the HTML element);

- Local z-index values are used within stacking contexts created by other components.

---

## Example of global z-index

<br />

| Elevation Name | Typical z-index / Shadow | Example Element          |
|----------------|--------------------------|---------------------------|
| Base           | 0                        | Page background           |
| Raised         | 1–2                      | Card, tile                |
| Overlay        | 10                       | Popover, overlay          |
| Dropdown       | 20                       | Dropdown menu             |
| Sticky         | 30                       | Sticky header/footer      |
| Modal          | 40                       | Modal dialog              |
| Tooltip        | 50                       | Tooltip                   |
