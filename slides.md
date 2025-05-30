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

## <div class="text-center">[Z-Index not Working](https://codepen.io/AndreaMargutti/pen/QwbwaoW?editors=1100)</div>
