# Canvas vs WebGL

### Background

The JS/WASM runtime provides various packages which are published to npm. For simple usage and best performance, we recommend starting with `@rive-app/canvas` (more on that below). On the web, Rive offers the ability to use a backing `CanvasRenderingContext2D` context or `WebGL` context associated with a `<canvas>` element for rendering Rive animations.

**Note:** The high-level API and the logic for creating a Rive instance in your script remain the same for the `@rive-app/webgl` and `@rive-app/canvas` packages. That means if you decide one runtime package is better for your use case over the other, you only need to switch the dependency, but not the usage.\
\
See more below on each of the different runtimes for JS/WASM and when to use which package.

### (Recommended) @rive-app/canvas

```
npm install @rive-app/canvas
```

An easy-to-use high-level Rive API using a backing [CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/Canvas\_API) renderer. This lets Rive use the browser's native high-level vector graphics renderer. Some benefits of this package:

* Extremely small download size
* Great for displaying many animated canvases concurrently on the screen. This is ideal for when you want to render lists or grids of Rive animations on the screen, as there is no context limit by the browser (as opposed to WebGL)
* Support for simple vector graphics animations and raster
* Requests the Web Assembly (WASM) backing dependency for you
* Web Assembly (WASM) backing dependency is part of the JS bundle; no need for any WASM request

{% hint style="warning" %}
On some newer versions of Chrome, the use of some blend modes defined at design-time in Rive files may cause unexpected behavior at runtime with `@rive-app/canvas`. It is a [reported bug](https://bugs.chromium.org/p/chromium/issues/detail?id=1350133) that hopefully will be addressed properly soon. If you run into this issue and cannot remove problematic blend modes in the Rive file, please temporarily use the `@rive-app/webgl` runtime below, as the renderer does not face this issue.
{% endhint %}

### @rive-app/webgl

An easy-to-use high-level Rive API using the [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL\_API) renderer. Some benefits of this package:

* Highest fidelity with edit-time experience.
* Requests the Web Assembly (WASM) backing dependency for you

**A note about WebGL:** Most browsers limit the number of concurrent WebGL contexts by page/domain. Using Rive, this means that the browser limit impacts the number of `new Rive({...})` instances created. See the [Rive Parameters](rive-parameters.md) docs for the `useOffscreenRenderer` option that may assist in working around this limitation.

If you're planning on displaying Rive content in a list/grid or many times on the same page, it's up to you to manage the lifecycle of the provided context and `canvas` element. If you need to display many animations (i.e grids/lists), consider using the `@rive-app/canvas` package which uses the `CanvasRenderingContext2D` renderer and does not have a context limitation.

We are working on a better solution to getting around the browser context limit, while still being performant in various browsers.&#x20;

{% hint style="warning" %}
It is highly recommended to set the `useOffscreenRenderer: true` property in the Rive parameters when creating a `new Rive({...})` object, especially when rendering multiple Rive objects on the page.
{% endhint %}

### @rive-app/canvas-advanced

A low-level Rive API using the [CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/Canvas\_API) renderer. It has the same benefits as the regular `@rive-app/canvas` package plus:

* Full control over the update and render loop
* Allows for rendering multiple Rive artboards to a single canvas
* Allows deeper control and manipulation of the components in a Rive hierarchy
* Web Assembly (WASM) is part of the NPM bundle, but you load in the WASM manually

See an example of usage here: [https://codesandbox.io/s/rive-js-advanced-api-centaur-example-exh2os](https://codesandbox.io/s/rive-js-advanced-api-centaur-example-exh2os)

### @rive-app/webgl-advanced

A low-level Rive API using the [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL\_API) renderer. It has the same benefits as the regular `@rive-app/webgl` package plus:

* Full control over the update and render loop.
* Allows for rendering multiple Rive artboards to a single canvas.
* Allows deeper control and manipulation of the components in a Rive hierarchy.
* Web Assembly (WASM) is part of the NPM bundle, but you load in the WASM manually

See an example of usage here: [https://codesandbox.io/s/rive-webgl-advanced-api-fwtmdb](https://codesandbox.io/s/rive-webgl-advanced-api-fwtmdb)

### \*-single versions

Each of the above NPM packages includes the `rive.wasm` file. In the high-level APIs (`@rive-app/canvas` and `@rive-app/webgl`) the runtime requests this for you so you don't have to, as opposed to their `-advanced` counterparts. We also have alternative versions of each of the above packages on NPM that have the WASM encoded in the JS bundle. This means you won't have to make a network request for the WASM that powers the Rive animations, as it's all in one main JS file.

* [@rive-app/canvas-single](https://www.npmjs.com/package/@rive-app/canvas-single)
* [@rive-app/canvas-advanced-single](https://www.npmjs.com/package/@rive-app/canvas-advanced-single)
* [@rive-app/webgl-single](https://www.npmjs.com/package/@rive-app/webgl-single)
* [@rive-app/webgl-advanced-single](https://www.npmjs.com/package/@rive-app/webgl-advanced-single)

While there is no request for WASM here, the JS bundle will be larger than the above packages. The non-single versions may cache better on your web applications, as the WASM gets loaded from the same CDN source if loaded from multiple pages.

