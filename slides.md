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
<span class="text-blue-300">[Z-index First Example](https://codepen.io/AndreaMargutti/pen/OPVXZXV?editors=1100)</span>

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
layout: image
image: https://miro.medium.com/v2/resize:fit:1400/1*uGPV3qEF7yBq4PD0zua19A.png
---

---

# <span class="text-red-300">But First</span>: how does the browser position elements?

The defualt positioning of elements is called <span class="underline underline-offset-2">**Flow Layout**</span>.
Normally, elements are positioned in the order they appear in the HTML document, from top to bottom. On the z axes,
elements are stacked in the following order:
1. Elements with a position value of static (default).
2. Elements with a position value different thant static (relative, absolute, fixed, sticky).
3. Elements with a z-index value other than auto (<span class="text-red-300">if z-index is a valid property</span>).

<!--TODO: remove or keep this image? -->
<img src="https://miro.medium.com/v2/resize:fit:600/1*tgDL2s_DsMFmjMG9GK14AA.png" alt="Stacking Order" class="w-1/2 mx-auto block mt-8">


---

# How does z-index work?

Z-Index works by assigning a numerical value to an element, which determines its position in the stacking order relative to other elements on the page.
But the z-index property *only works on positioned elements* (elements with a position value of relative, absolute, fixed, or sticky).

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
  z-index: 1; /* This will not work */
}
```
::

The above code will not work because the element is not positioned. To make it work, we need to add a position value:

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

---

# How does z-index work? (2)

Saying that z-index only works on positioned elements is not quite accurate. To be more precise, the example above doesn't work because the z-index
property is not implemented in the Flow layout algorithm (default).
But it works with position becasue the z-index property is implemented in the Positioned Layout as in the FlexBox / Grid Layout.

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
  position: relative; /* change the layout from Flow to Positioned */
  z-index: 1; /* Property is now usable and will have effect */
}
```

::

## <div class="mt-8">[Understanding Layout Algorithms](https://www.joshwcomeau.com/css/understanding-layout-algorithms/)</div>

---

# The Problem with Z-Index

But there's a problem that many developers face when working with z-index:

## <div class="text-center mb-10 text-blue-300">[Z-Index not Working](https://codepen.io/AndreaMargutti/pen/QwbwaoW?editors=1100)</div>

Why does it not work as expected?
Well, to explain that, we need to understand what stacking contexts are and how they work.

## Stacking Contexts

*Stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.*

<div class="text-end"><strong>Source:</strong> <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Stacking_context">MDN</a></div>

---

# The Solution

To solve the problem with z-index, we need to understand how stacking contexts work and how to create them.

## What creates a stacking context?

<div class="text-sm">

1. Root element of the document.
2. Element with a position value absolute or relative and z-index value other than auto.
3. Element with a position value fixed or sticky.
4. Element with a container-type value size or inline-size set (See container queries).
5. Element that is a flex item with a z-index value other than auto.
6. Element that is a grid item with z-index value other than auto.
7. Element with any of the following properties with a value other than none:
    - transform
    - scale
    - rotate
    - translate
    - filter
8. Element with the isolation value isolate.
    
</div>

---

# And so...

## <div class="text-center mb-10 text-blue-300">[Solution](https://codepen.io/AndreaMargutti/pen/KwpwZjd)</div>

This works because the red div creates a new stacking context, which means its children (orange div) are positioned relative to it, not the body.
If we want to position the orange div on top of the blue one we need to set a z-index value higher than the blue one on the red div.

### <span class="text-red-300">[What the Heck, z-index??](https://www.joshwcomeau.com/css/stacking-contexts/)</span>

---

# This relationship is not a One to One

It's important to note that a stacking context is not a one-to-one relationship with z-index. An element can create a stacking context without having a z-index value set, and in
the opposite way an element can have a z-index value set without creating a stacking context. <span class="text-red-600">**They DO NOT always go together**</span>.
The most common case is when we are using flex: flex items can use the property *z-index* but <ins>they do not always create a stacking context</ins>.

## <a href="https://codepen.io/AndreaMargutti/pen/bNdEGoe?editors=1100" class="text-blue-300">z-index with flex</a>

[Z-index Explained: How to stack element Using CSS](https://medium.com/free-code-camp/z-index-explained-how-to-stack-elements-using-css-7c5aa0f179b3)

---

# Isolation Property

The isolation CSS property determines whether an element must create a new stacking context: 
- No need to prescribe a z-index value
- Can be used on statically-positioned* elements
- Doesn't affect the child's rendering in any way

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

<!-- TODO: add example -->

[Global v Local Z-Index](https://david-gilbertson.medium.com/my-approach-to-using-z-index-eca67feb079c)

---

<div class="text-sm my-4">

# Some Useful Resources

1. [z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index)
2. [Using z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Using_z-index)
3. [Stacking without the z-index property](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Stacking_without_z-index)
4. [Understanding z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index)
5. [My approach to using z-index](https://david-gilbertson.medium.com/my-approach-to-using-z-index-eca67feb079c)

</div>

<div class="text-sm my-4">

## <span>Deep Dive</span>

1. [Isolation Property - Browser Compability](https://caniuse.com/?search=isolation)
2. [Layout Mode - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Layout_mode)

</div>

## <span>Debugging Tools</span>

<div class="text-sm my-4">

1. **Chrome DevTools Extension**: [CSS Stacking Context Inspector](https://chromewebstore.google.com/detail/css-stacking-context-insp/apjeljpachdcjkgnamgppgfkmddadcki)
2. **VsCode Extension**: [Better CSS Stacking Contexts](https://marketplace.visualstudio.com/items?itemName=mikerheault.vscode-better-css-stacking-contexts)

</div>