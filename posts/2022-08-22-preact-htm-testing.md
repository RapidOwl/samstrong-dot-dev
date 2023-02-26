---
title: Testing Preact + HTM
description: My experiences learning how to work with and test Preact + HTM.
date: 2021-06-08
tags:
  - webdev
  - frontend
  - testing
layout: layouts/post.njk
---

As part of the never ending work that is teaching myself "modern" front end development, I decided to take a look at Preact.

## Getting Started

I knew Preact was a tiny library that shared a lot of APIs with React, but it wasn’t until I read their getting started guide that I realised it now offered a way of working that avoids the standard build step that comes with pretty much all JavaScript these days.

Instead of the standard module imports, you can import from a CDN like so:

```js
import { html, render } from 'https://unpkg.com/htm/preact/index.mjs?module';
```

You can then write your components like this:

```js
function Result({url, alt, description}) {
	return html`
		<img src=${props.url} alt=${props.alt} />
		<p>${props.description}</p>
	`;
	
render(html`<${Result} 
	url='https://example.com/example.jpeg'
	alt='A lovely image'
	description='Buy stuff'
/>`, document.getElementById('TargetElement'));
```

HTM makes use of JavaScript's tagged template literals, which pretty much means passing a template literal to a function without using any brackets, hence the odd looking syntax.

Stick the above in a HTML file  and you've got yourself a very minimal Preact development setup. Add in the Live Server extension for Visual Studio Code and it hot reloads as well. Just lovely.

## Testing

Testing our components is also a vitally important and for this experiment I decided to try Vitest. At time of writing, Vitest is the hot new testing tool in the JavaScript world so in the grand tradition of hype-driven development here we go.

Vitest is basically Jest. That's to say it's a test framework that includes a test runner and all the basic assertions you need. But I was using Preact so I also brought in Preact Testing Library, a set of extensions that sit on top of Vitest and provide tools for testing components.

In order to make this work, we need to solve a few problems:

1. We're using CDN imports in our HTML file, but Vitest runs in Node, which doesn't support this.
2. In order to test our components, we need a browser-like environment and Vitest doesn't come with one.

### Import Aliasing

We import our dependencies like this:

```js
import { html } from 'https://unpkg.com/htm/preact/index.mjs?module';
```

But Vitest requires us to import our dependencies like this:

```js
import { html } from 'htm/preact';
```

So when we import the JavaScript file that contains our components and Vitest encounters a CDN style import it errors and the test fails. Enter Resolve Aliases. Vitest supports the aliasing of strings for module imports. This is intended to let you replace things like `'./../../../../src/thing.js'` with `'@/src/thing.js'`, but it doesn't matter what string you use it to replace.

In your `vite.config.js`, the object you pass into `defineConfig` would include a resolve property similar to this:

```js
resolve: {
	alias: [
		{
			find: 'https://unpkg.com/htm/preact/index.mjs?module',
			replacement: 'htm/preact'
		}
	]
}
```

### A Browser Like Environment

As I said above, Vitest doesn't come with a browser-like environment. For this we can use jsdom. As per the docs, jsdom is:

> jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG [DOM](https://dom.spec.whatwg.org/) and [HTML](https://html.spec.whatwg.org/multipage/) Standards, for use with Node.js.

What this means is we can use it as a fake browser to test our components.

### Setting Up Both With vitest/config

Vitest uses the same config file as Vite. Create a file called `vite.config.js` in the root of your project and populate it as below.

```js
import { defineConfig } from 'vitest/config'

export default defineConfig({
	test: {
		environment: 'jsdom'
	},
	resolve: {
		alias: [
			{
				find: 'https://unpkg.com/htm/preact/index.mjs?module',
				replacement: 'htm/preact'
			},
			{
				find: 'https://unpkg.com/preact@latest/hooks/dist/hooks.module.js?module',
				replacement: 'htm/preact'
			}
		]
	}
})
```

The first section of the config tells Vitest to use jsdom as its test environment. The second section sets up the aliases we'll need to stop Vitest erroring when it encounters a CDN style import.

### A Test

Once we've got everything set up, we can write tests like this example:

```js
import { render } from '@testing-library/preact';
import { describe, expect, test } from 'vitest';

import { html } from 'htm/preact';

import { Result } from './quiz';

describe('Result', () => {
	test('should display image', () => {
		const { getByAltText } = render(html`<${Result} 
			url='https://example.com/example.jpeg'
            alt='A lovely image'
            description='Buy stuff'
		/>`);

		const image = getByAltText('A lovely image');

		expect(image.src).toContain('https://example.com/example.jpeg');
	});
});
```