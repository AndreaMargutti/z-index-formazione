---
theme: default
title: Z-Index and Stacking Contexts in CSS
class: text-center
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

Z-Index is a CSS property that controls the vertical stacking order of elements that <ins>overlap</ins>.
The higher the z-index value, the closer the element will appear to the user.

<img src="https://bitsofco.de/img/zeDmwlyRbq-780.png" width="650px" class="mx-auto"/>

---
layout: image
image: https://miro.medium.com/v2/resize:fit:1400/1*uGPV3qEF7yBq4PD0zua19A.png
---

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

With this new approach we are going to divide the z-index into two categories: **Global** and **Local**.
Global z-index are refferred to as the "root" stacking context (HTML element) while the local ones are those who stays inside other stacking contexts
created by any other element.

We can think of the global z-index as the "master" z-index that controls the overall stacking order of elements on the page, while local z-indexes are used to control the stacking order of elements within a specific context.

Local z-index will be used for elements that need to render on top of a sibiling or nearby element,
Gloabl z-index will be used for elements that need to render on top of the entire page or a specific section of it.

Combine this approach with the isolation property to get a cleaner code, less design bugs and a more organized layers structure.

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

1. [Layout Algorithms](https://www.joshwcomeau.com/css/understanding-layout-algorithms)
2. [Isolation Property - Browser Compability](https://caniuse.com/?search=isolation)
3. [CSS Stacking Context inspector](https://chromewebstore.google.com/detail/css-stacking-context-insp/apjeljpachdcjkgnamgppgfkmddadcki?pli=1)

</div>

## <span>Debugging Tools</span>

<div class="text-sm my-4">

1. **Chrome DevTools Extension**: [CSS Stacking Context Inspector](https://chromewebstore.google.com/detail/css-stacking-context-insp/apjeljpachdcjkgnamgppgfkmddadcki)
2. **VsCode Extension**: [Better CSS Stacking Contexts](https://marketplace.visualstudio.com/items?itemName=mikerheault.vscode-better-css-stacking-contexts)

</div>