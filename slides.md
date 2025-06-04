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

Z-Index is a CSS property that controls the vertical stacking order of elements that <span class="underline underline-offset-2">overlap</span>.
The higher the z-index value, the closer the element will appear to the user.
<span class="text-blue-300">[CodePen](https://codepen.io/AndreaMargutti/pen/OPVXZXV?editors=1100)</span>

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

The Browser uses a *layout algorithm* to determine how elements are positioned on the page.
By default, the browser uses the **Flow Layout**, which positions elements in the order they appear in the HTML document, from top to bottom.
There are other layouts,:
1. Positioned (eg. relative, absolute)
2. FlexBox
3. Grid
4. Table


---

# How does z-index work?

Z-Index works by assigning a numerical value to an element, which determines its position in the stacking order relative to other elements on the page.
Z-index is not implemented in all the layout algorithms, but it is used in the following:

1. Positioned
2. FlexBox / Grid

<div class="mt-8">

::code-group

```html [HTML ~i-vscode-icons:file-type-html~]
<div class="box">
  BOX 1
</div>
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

But there's a problem that many developers face when working with z-index:

## [CodePen](https://codepen.io/AndreaMargutti/pen/QwbwaoW?editors=1100)

---
level: 3
layout: image-right
background-size: contain
image: https://www.joshwcomeau.com/_next/image/?url=%2Fimages%2Fstacking-contexts%2Fphotoshop-groups.png&w=640&q=75
---

# Stacking Contexts

*Stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.*

---

# What creates a stacking context?

<div class="text-sm">

1. Root element of the document.
2. Element with a position value other thant static (with relative and absolute we need z-index != auto).
4. Element with a container-type value size or inline-size set (See container queries).
5. Element that is a flex / grid item with a z-index value other than auto.
6. Setting opacity to a value less than 1
7. Applying a mix-blend-mode other than normal
8. Using transform, filter, clip-path, or perspective
9. Element with the isolation value isolate.
    
</div>

---

# And so...

## [CodePen](https://codepen.io/AndreaMargutti/pen/KwpwZjd)

<div class="mt-8">
This example is now working as expected because by removing the `position: absolute`from the `.red`div, we are
no longer creating a new stacking context, and the z-index values are now applied correctly.
</div>


---

# This relationship is not a One to One

It's important to note that a stacking context is not a one-to-one relationship with z-index. An element can create a stacking context without having a z-index value set, and in
the opposite way an element can have a z-index value set without creating a stacking context. **They DO NOT always go together**.

---

# Isolation Property

The isolation CSS property determines whether an element must create a new stacking context: 
- No need to prescribe a z-index value
- Can be used on statically-positioned elements
- Doesn't affect the child's rendering in any way
- Keeps the code cleaner and easier to read
- No need to change the position / layout of the element / component

```css
.element {
  isolation: auto; /* default / no new stacking context is created */
}

.element-2 {
  isolation: isolate; /* a new stacking context is created */
}
```

With the default value *auto*, no new stacking context is created—unless it is overridden by other properties such as **transform**, **filter**, **opacity**, etc.

---

# A new Approach to Z-Index: Local vs Global

With this new approach, we divide z-index usage into two categories: Global and Local.

Global z-index values refer to the "root" stacking context (typically the HTML element), while local z-index values are used within stacking contexts created by other elements.

We can think of global z-index as the “master” layer that controls the overall stacking order of elements across the page, whereas local z-index values are used to manage the stacking order within a specific stacking context.

Local z-index should be applied to elements that need to appear above a sibling or nearby element.
Global z-index should be applied to elements that need to appear above the entire page or a significant section of it.

Combine this approach with the isolation property to achieve cleaner code, fewer design bugs, and a more organized layer structure.
