---
title: CSS as a System
description: Thinking about CSS as a system instead of adding random rules reactively.
date: 2021-04-09
tags: css
layout: layouts/post.njk
---

I've been working through Josh Comeau's [CSS for JavaScript developers](https://css-for-js.dev/) (it's really great). What initially attracted me to the course was his aim to teach CSS as a system. When writing CSS, it's very easy to code yourself into endless corners and end up reactively adding more and more rules in order to fix any problems you may encounter.

By thinking about CSS as a system (even before you start writing any code), I hope to do better! So, some things I need to make sure I think about:
- Does an element really need margins? Is their utility worth dealing with the collapse?
- What are the containing blocks of my elements? Can I limit the number of containing blocks in order to better reason about the positioning of my elements?
- What stacking contexts do I need? Can I limit the number of stacking contexts in order to better reason about how z-index functions?
- What design tokens will I need? Colours? Relative font sizes?