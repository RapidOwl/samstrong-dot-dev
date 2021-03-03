---
title: Ways of Building Web Apps
description: The various ways one can build a web application.
date: 2021-03-03
tags: webdev
layout: layouts/post.njk
---

There are many ways to build a web app and there are a lot of people out there with _very_ strong opinions about which is best. The answer, of course, is "it depends", but you can't let that stop anyone preach about their favourites.

The following are the methods that I'm aware of. And to be clear, we're making the assumption that most web apps will make a request for some JSON at some point in their lifecycle.

## 1. Server rendered with JavaScript

The old faithful that is still perfectly good for most people's needs. Your data is retrieved and your markup is rendered on the server before being delivered to the browser. Click a link and the same happens. Submit a form and the same happens.

You will write a lot of vanilla JavaScript that will more than likely end up in a spaghetti mess that no one will want to inherit.

e.g. Rails, Django and ASP.Net Core.

## 2. Server rendered with a small JS library

Same as the above, except you introduce a library on the client side to make writing your JavaScript a little more sane.

e.g. AlpineJS, KnockoutJS (for the elderly among us)

## 3. Server rendered with HTML over the wire

Same as number one except when your UI needs to be updated (load new page, update panel etc) your application shell sends a request that returns new markup. No JSON involved (at this point). Apparently the "new hotness" (if you like Ruby on Rails).

e.g. Hotwire, TurboLinks, Phoenix

## 4. Just a SPA

Be seduced by the godlike toolchain. Start with your index.html, write a bunch of JavaScript in a framework specific DSL and compile. Magic happens. Your UI is entirely rendered on the client.

Brilliant for rapid prototyping where you literally don't care about anything other than a short feedback loop. To be fair, hot reloading is incredible and tools like MirageJS mean you can build your UI without having to actually build an API.

e.g. React, Vue, Svelte

## 5. Server rendered SPA

Some of the benefits of number 1 with the client side smoothness and toolchain of a SPA. It's a little unclear, but as I understand it, your first request hits a server (or function, yes, thank you). The SPA is rendered and its state is hydrated server-side and the result is served to the browser. From there it behaves like a normal SPA. Each "page" is rendered on the server, but state mutations are done client side.

e.g. NextJS (React), NuxtJS (Vue), Sapper (Svelte)

## 6. Statically rendered SPA

Similar to a server-rendered SPA except all of your routes are parsed and the results statically rendered to HTML files, which are stored on the server. When someone hits a route the HTML is downloaded and the app becomes a SPA from there.

e.g. NextJS (React), NuxtJS (Vue), Sapper (Svelte)

## 7. <Language> Compiled to Web Assembly

Possibly the future. You write in your language of choice (that can be compiled to Web Assembly). The browser then executes the Web Assembly and does magic.

e.g. Blazor

## 8. Micro-front-end

You have multiple teams building the same website. They all have polarised opinions about what the best approach is. Enter micro-front-ends. Your site becomes a montrous hybrid that forces its users to download the core libraries for every JavaScript framework under the sun and your teams are incapable of working across the entire front end because they have very specific skillsets and very strong opinions.

e.g. I heard about this sort of thing at a conference and got pretty scared