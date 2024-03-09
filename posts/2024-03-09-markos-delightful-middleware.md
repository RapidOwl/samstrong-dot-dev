---
title: Marko Run's Delightful Middleware
description: Marko Run's file-based routing supports middleware at every level.
date: 2024-03-09
tags:
  - webdev
  - frontend
layout: layouts/post.njk
---

One of the things that I judge front end frameworks by is how easy it is to manage requests by way of middleware. Most frameworks I see support a single middleware file that sits somewhere near the root of your project. If you have operations that you need to perform against specific routes then you get to manually check the URL path.

[Marko Run](https://github.com/marko-js/run) (the SSR layer behind Marko) features folder-based routing like a lot of other frameworks, but it also lets you drop a `+middleware.js` file in at any level. Middleware files will only execute when a user hits a route in that folder path. The even cooler part is that if you have a middleware file next to a handler and a middleware file further up that folder path then both of those bits of middleware will execute before that handler.

Stealing an example from the Marko docs, using the folder structure below, the `+middleware.js` in the routes folder and the `+middleware.js` in the about folder will both execute before the `+handler.js` (in that order).
```
routes/
  about/
    +handler.js
    +layout.marko
    +middleware.js
    +page.marko
  +layout.marko
  +middleware.js
  +page.marko
```

I think this makes middleware much easier to compose and reason about.
