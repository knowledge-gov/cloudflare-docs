---
pcx-content-type: how-to
title: Module Support
weight: 13
---

## Module Support

Pages Functions provide support for several module types(// TODO @Carmen link to our blog post), just like Workers [do]((https://blog.cloudflare.com/workers-javascript-modules/)). This means that you can import modules such as `wasm` or `binary` inside your Functions code and they will just work (// TODO @Carmen change wording here)

{{<Aside type="note">}}
// TODO @Carmen
do we want to mention anything about bundling here, or is that outside the scope of this section?
{{</Aside>}}

### ECMAScript Modules

ECMAScript modules (or in short ES Modules) are the official, [standardized](https://tc39.es/ecma262/#sec-modules) module system for JavaScript. They are the recommended mechanism for writing modular and reusable Javascript code. 

[ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) are defined by the use of `import` and `export` statements:

```js
---
filename: src/greeting.ts
---
export function greeting(name: string): string {
  return `Hello ${name}!`;
}
```

You can import ES Modules within your Functions code the following way:

```js
---
filename: functions/hello.ts
---
import { greeting } from "../src/greeting.ts";

export async function onRequest(context) {
    return new Response(`${greeting("Pages Functions")}`);
}
```

### WebAssembly Modules

[WebAssembly](https://webassembly.org/) (or Wasm) is a low-level language that provides programming languages such as C++ or Rust with a compilation target so that they can run on the web. The distributable, loadable, and executable unit of code in WebAssembly is called a [module](https://webassembly.github.io/spec/core/syntax/modules.html).

Here is a basic example of how you can import Wasm Modules inside your Functions code:

```js
---
filename: functions/meaning-of-life.ts
---
import addModule from "add.wasm";

export async function onRequest() {
	const addInstance = await WebAssembly.instantiate(addModule);
	return new Response(
		`The meaning of life is ${addInstance.exports.add(20, 1)}`
	);
}
```

### Text Modules

Text Modules are a non-standardized means of importing resources such as HTML files as a `String`.

For example, given the following HTML file:

```html
---
filename: index.html
---
<!DOCTYPE html>
<html>
  <body>
    <h1>Hello Pages Functions!</h1>
  </body>
</html>
```

here is how you can import it inside your Function:

```js
---
filename: functions/hey.ts
---
import html from "../index.html";

export async function onRequest() {
	return new Response(
		html,
    {
      headers: { "Content-Type": "text/html" }
    }
	);
}
```


### Binary Modules

Similarly, Binary Modules are a non-standardized way of importing resources such as images, as an `ArrayBuffer`.

// TODO @Carmen tentative for now. must verify it works
```js
---
filename: functions/flower.ts
---
import flower from "../images/flower.png";

export async function onRequest() {
	return new Response(
		flower,
    {
      headers: { "Content-Type": "image/png" }
    }
	);
}
```