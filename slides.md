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
level: 2
---

# What is Z-Index?

Z-Index is a CSS property that controls the vertical stacking order of elements that <ins>overlap</ins>
The higher the z-index value, the closer the element will appear to the user.

<img src="https://bitsofco.de/img/zeDmwlyRbq-780.png" width="650px" class="mx-auto"/>

---

# The Problem with Z-Index

But there's a problem that many developers face when working with z-index:

## <div class="text-center mb-10">[Z-Index not Working](https://codepen.io/AndreaMargutti/pen/QwbwaoW?editors=1100)</div>

Why does it not work as expected?
Well, to explain that, we need to understand what stacking contexts are and how they works.

## Stackging Contexts

*Stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.*

<div class="text-end"><strong>Source:</strong> <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Stacking_context">MDN</a></div>