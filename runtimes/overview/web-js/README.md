---
description: JS/WASM runtime for Rive
---

# Web (JS)

## Overview

This guide documents how to get started using the Web runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-wasm).\
\
This library has both a high-level JavaScript API (with TypeScript support), as well as a low-level API to load in Web Assembly (WASM) and control the rendering loop yourself. This runtime is great for the following:

* Quickly integrating Rive into all kinds of web applications (i.e Webflow, Wordpress, etc.)
* Providing a base API to build other web-based Rive runtime wrappers (i.e React, Angular, etc.)
* Advanced use cases in controlling a render loop

## Getting Started

Follow the steps below for a quick start on integrating Rive into your web app.

### 1. Install the dependency

Add the following script tag to a web page; we recommend sticking to one version, such as seen below:

```javascript
<script src="https://unpkg.com/@rive-app/canvas@1.0.65"></script>
```

Find the versions of the runtimes in the "Versions" tab here: [https://www.npmjs.com/package/@rive-app/canvas](https://www.npmjs.com/package/@rive-app/canvas)

Alternatively, you can import our recommended web runtime package via npm/yarn in your project:

```
npm install @rive-app/canvas

// example.js
import rive from "@rive-app/canvas";
```

If you need more details and options for a web runtime, check out the installation section in the [README](https://github.com/rive-app/rive-wasm#installing) docs.

### 2. Create a canvas

Create a canvas element where you want the Rive file to display in your HTML:

```javascript
<canvas id="canvas" width="500" height="500"></canvas>
```

### 3. Create a Rive instance

Create a new instance of a Rive object, providing the following properties:

* `src` - A string of the URL where the `.riv` file is hosted (like in the example below), or the path to a local `.riv` file
* `canvas` - The canvas element on which you want the animation to render
* `autoplay` - Boolean for whether you want the default animation to play

```javascript
<script>
    new rive.Rive({
        src: "https://cdn.rive.app/animations/vehicles.riv",
        // Or the path to a local Rive asset
        // src: './example.riv',
        canvas: document.getElementById("canvas"),
        autoplay: true
    });
</script>
```

### Complete example

Putting this altogether, you can load an example Rive animation in one HTML file:

```javascript
<html>
  <head>
    <title>Rive Hello World</title>
  </head>
  <body>
    <canvas id="canvas" width="500" height="500"></canvas>

    <script src="https://unpkg.com/@rive-app/canvas@1.0.65"></script>
    <script>
      new rive.Rive({
        src: "https://cdn.rive.app/animations/vehicles.riv",
        canvas: document.getElementById("canvas"),
        autoplay: true
      });
    </script>
  </body>
</html>
```

**Try it out on your own:** Check out this [CodeSandbox](https://codesandbox.io/s/rive-plain-js-sandbox-1ddrc?file=/src/index.js) for a small setup to test your own Rive files!

### Loading in Rive files

You can choose to load Rive files in the following ways:

* Hosted URL - The string of the URL where the `.riv` file is hosted. Set this to the `src` attribute when instantiating a `Rive` object
* Static assets in the bundle - String of a path to the public/static assets in your web project. Treat `.riv` files in the project just as you would any other asset in your bundle, such as images or font files
* Fetching a file - Instead of using the `src` attribute, use the `buffer` attribute to load in an ArrayBuffer when fetching a file. See an example [here](https://codesandbox.io/s/rive-buffer-import-9989fv)

## Resources

Github: [https://github.com/rive-app/rive-wasm](https://github.com/rive-app/rive-wasm)\
Examples: [https://github.com/rive-app/rive-wasm/tree/master/js/examples/\_frameworks/parcel\_example\_canvas](https://github.com/rive-app/rive-wasm/tree/master/js/examples/\_frameworks/parcel\_example\_canvas)&#x20;

