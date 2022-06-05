---
title: 'How I encountered and fixed an issue related with ES/CommonJS modules'
date: '2022-05-06'
---

**I'd expect the future me to be grateful for this.**

Ok, so since, by default, everything in JavaScript has a global scope, there are bound to be some issues concerning packages and how they work together.

**CommonJS** was a project to handle modules outside of the browser. Back then, Javascript didn't really
have a standardised set of guidelines on how its modules should interoperate.

So **CommonJS** (which uses the `require()` syntax) was the first attempt on solving this problem.

CommonJS syntax would be used by NodeJS up until version 14 when support for **ESModules** was introduced.

ESModules were introduced by ECMA and used the `import` syntax. Usually, you would have to specify the
`.mjs` file extension in order for the ESModules to work. However, in Node's `package.json`, you can set the
`"type": "module"` in order to treat all `.js` files as ESModules. If you omit this, or set the `type` to `commonjs`, then they will be treated as CommonJS modules.

Also if you ever find yourself wondering what the heck is `esnext` then it is basically just another name for the next version of ECMAScript. So, ES2023, ES2024 and so on, whatever's next.

So, as of 2022, it seems like the best approach to avoid confusion is to use ESModules syntax everywhere - both client and server side.

So, today, I found myself in quite a pickle. I attempted to use `import` statements instead of `require` across all my app. So, I wrote a script to run a server-side Express app like this...

`ts-node-dev api/server.ts`

...but whenever I tried to run it, I would get the following error:

![1](/images/setup.png?raw=true 'Title')

So there are two things I need to make clear before I go on to explain how I solved it.

The first thing is the `type` field in your `package.json`. It's basically a way of specifying if your app should use the ES or CommonJS syntax. You can set it to either `commonjs` or `module`, **but remember that if you decide to omit the `type` altogether, it will understand that your project uses the common js syntax**.

The second thing is that Typescript has its own `ts.config` file where you can explicitly specify whatever type of modules you want your transpiled code to use. You can set it either `esnext` (basically any ES version) or `commonjs`.

So, the thing is, that my `package.json` and `ts.config` were set up like this:

![1](/images/error.png?raw=true 'Title')

This just cannot work. Your code gets transipiled to use ESModules while, in `package.json` the type is omitted, signifying that it understands that the project uses CommonJS.

In order for it work, they have to match. So it's either `"type":"module"` in `package.json` and `"module":"esnext"` in ts.config or both set to `commonjs`.

However, that's not over. `ts-node-dev` - which basically transpiles .ts to .js and executes the code - does not support ESModules. So you have to transpile your code to use `commonjs` syntax and omit the `type` flag in your `package.json` file.

TL;DR: If you're using `ts-node-dev` and want to use ESModule syntax across the entire app, set your `package.json` and `tsconfig.json` like so:

![1](/images/likeso.png?raw=true 'Title')

No worries, future me, have a good one, buddy.
