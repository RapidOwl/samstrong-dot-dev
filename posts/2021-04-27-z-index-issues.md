---
title: Troubleshooting z-index issues
description: A few things to check when I run into z-index issues.
date: 2021-04-27
tags: css
layout: layouts/post.njk
---

I often fall foul of z-index issues. So here are a few things to try:
* Can we fix this by re-ordering elements in the DOM?
* Do we need to create a new stacking context?
* * Investigate using the [Stacking Context Inspector](https://chrome.google.com/webstore/detail/css-stacking-context-insp/apjeljpachdcjkgnamgppgfkmddadcki) Chrome extension.
* * Wrap one of the elements in another suitably positioned element?
* * Use `isolation: isolate;` to create a new context without any layout changes.
* Last resort: Change the actual z-index.