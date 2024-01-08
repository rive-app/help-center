---
description: Migration guide from the rive-js package
---

# Migrating from rive-js

Previously, the web runtime would deploy to the [rive-js](https://www.npmjs.com/package/rive-js) package on npm. We have since moved away from this one-package model and into a place where you can import from several different packages based on your API/rendering-level needs.

* @rive-app/webgl
* @rive-app/webgl-advanced
* @rive-app/canvas
* @rive-app/canvas-advanced

In addition to these new packages, there are `*-single` version packages for each of the above that have the WASM encoded in the JS. See [the web runtime docs](https://github.com/rive-app/rive-wasm/blob/master/WEB\_RUNTIMES.md) to help you decide which runtime package you may need for your project.\
\
We changed the package model to choose which renderer to use (i.e., [CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/Canvas\_API) vs. [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL\_API)), impacting bundle size and performance. In addition, all of the new web runtime packages will support the latest Rive features, such as raster assets.\
\
In any case, there should be no changes in high-level API usage required as far as using the `rive` instance. You only need to change the package you install in your project and the associated places you import it.\
\
For instance, instead of the following integration:

```bash
npm i rive-js
```

```javascript
import rive from 'rive-js';

const foo = new rive.Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
});
```

You could replace this with:

```bash
npm i @rive-app/canvas
```

```javascript
import {Rive} from '@rive-app/canvas';

const foo = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
});
```

Or, you can replace `@rive-app/canvas` with any of the new package outputs for web runtime that suit your need.
