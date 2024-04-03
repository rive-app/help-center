---
description: Using low-level JS APIs to construct Rive scenes
---

# Low-level API Usage

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/low-level-api-usage/doctAfBY6v3P).
{% endhint %}

## Background

While the JS runtime offers a high-level API that allows for integrating Rives into web applications quickly, the runtime also allows for a smaller advanced low-level API that allows for constructing and controlling Rive(s) in your own render loop. There are several reasons and benefits to using this lower-level API:

* Construct a scene of multiple Rive files, artboards, linear animations, and state machines, all in one `<canvas>` element. This is useful if you're building a game!
* Control the render loop, which involves how you advance each artboard, animation, and state machine over time (including speed)
* Ability to tap into several transform property values on nodes/bones in the draw hierarchy
* Smaller dependency size
* ...and more!

See a simple game example [here](https://codesandbox.io/s/rive-canvas-advanced-api-centaur-example-exh2os).

## Premise

Here's the basic render workflow using the low-level API to render Rives:

1. Load the Rive Web Assembly (WASM) file, which contains the module with lower-level APIs
2. Load the Rive file in
3. Create instances for Artboards, LinearAnimations, and StateMachines
4. Build the render loop function to manipulate the instances created above
   1. Advance any animation instances and apply it
   2. Advance any state machine instances
   3. Advance the artboard
   4. Render the updated artboard on the canvas
   5. Request the next animation frame
5. Clean-up created instances when finished

## Getting Started

If you’ve decided that the low-level JS APIs are what you need for your app, read below for a guide on how to set everything up, or you can skip to the end to see some examples in action.

### Loading in WASM

The first step to setting up the low-level Rive APIs is to load in the Rive WASM file from either the `@rive-app/canvas-advanced` or `@rive-app/webgl-advanced` libraries (by default, we recommend `@rive-app/canvas-advanced` for a smaller dependency, unless you need to use WebGL). When the WASM file is loaded into your app, you'll gain access to necessary APIs such as the renderer for canvas/WebGL, along with relevant JS classes generated from underlying CPP bindings via [rive-cpp](https://github.com/rive-app/rive-cpp), the core c++ runtime used as the base for several other Rive runtimes. You'll use these classes to construct your rendering scene in the canvas below.

You can load the Rive WASM file via [unpkg](https://unpkg.com/) (hosts our NPM modules for the JS runtimes), which will make a network call to the CDN, or you can choose to host the WASM file on your own servers. With `unpkg`, the URL will look something like this:

> [https://unpkg.com/@rive-app/canvas-advanced@1.1.5/rive.wasm](https://unpkg.com/@rive-app/canvas-advanced@1.1.5/rive.wasm)

{% hint style="info" %}
You'll want to ensure that the version at the end of `@rive-app/canvas-advanced@` or `@rive-app/webgl-advanced@` matches the version of the dependency you installed in your app. For example, if you installed `@rive-app/canvas-advanced@1.1.5` in `package.json`, the Rive WASM file you request from `unpkg` would be `https://unpkg.com/@rive-app/canvas-advanced@1.1.5/rive.wasm`.\
\
[See docs here](https://help.rive.app/runtimes/overview/web-js/preloading-wasm) if you'd like to preload WASM in.
{% endhint %}

To start, import the default module from the library and then call it with an object where you only need to set a single parameter, `locateFile`, which is a function that returns the URI of the WASM file. This can be either the `unpkg` URL or the URI to your self-hosted version of it. Simply `await` for the call to resolve, and then you'll get a reference to the low-level Rive runtime APIs.

```tsx
import RiveCanvas from '@rive-app/canvas-advanced';

async function main() {
  const rive = await RiveCanvas({
    locateFile: (_) => '<https://unpkg.com/@rive-app/canvas-advanced@1.1.5/rive.wasm>'
  });
}
main();
```

### Creating the Renderer

Once the WASM is loaded in, the next step is to create the renderer with the `makeRenderer()` API and pass in the canvas element on which Rive should render. The renderer draws Rive onto the `<canvas>` element with a rendering context. If you're using `@rive-app/canvas-advanced`, it will create a Canvas2D rendering context. If you're using `@rive-app/webgl-advanced`, it will create a WebGL rendering context.

```tsx
const canvas = document.getElementById('your-canvas-element');
const renderer = rive.makeRenderer(canvas);
```

### Loading in Rive Files

After the renderer is created, you can also start to load in the Rive file(s) as an ArrayBuffer, which you'll feed into the runtime's `load()` API. You can fetch this at a URL or from somewhere within your project.

```tsx
const bytes = await (
  await fetch(new Request('basketball.riv'))
).arrayBuffer();

// import File as a named import from the Rive dependency
const file = (await rive.load(new Uint8Array(bytes))) as File;
```

{% hint style="info" %}
Make sure to await the `.load()` call, as it synchronously tries to load assets from the `File`. Additionally, pass in the `ArrayBuffer` to a `Uint8Array` view before sending it as a param to `.load()`
{% endhint %}

### Setting up the Instances

Once you have a reference to the loaded `File` object, you can begin instancing all the artboards, state machines, and linear animations from the Rive file. Instancing creates an underlying CPP reference and allows you to control how each entity advances over time. More on that further down this guide.

The main components you will most likely want to instance are:

* `Artboard` - Instance 1 or more artboards from the Rive file you want to draw
* `StateMachineInstance` - Instance a state machine from a given artboard
* `LinearAnimationInstance` - Instance a single timeline animation from a given artboard

Start by instancing an artboard, and then you can create a state machine and linear animation instances from the artboard reference like below.

```tsx
const artboard = file.artboardByName('New Artboard');
const animation = new rive.LinearAnimationInstnace(
  artboard.animationByName('idle'),
  artboard
);
const stateMachine = new rive.StateMachineInstance(
  artboard.stateMachineByName('your-state-machine-name'),
  artboard
);
```

The great thing here is if you want to display multiple artboards or even copies of the same one on the canvas, you can easily do so (as opposed to the high-level API, which only displays one at a time).

Beyond instancing the relevant pieces for the render loop, you can also extract references to nodes, targets, and bones within the drawing hierarchy. This is useful if you need to track any transform property values on a given node for any calculations or even to get world-space or parent transforms (i.e., tracking the x, y-coordinate, or rotation value of a node over the lifetime of an animation). See some of the examples at the bottom of the guide to see this in action.

### Constructing the Render Loop

You may be familiar with constructing a render loop using `requestAnimationFrame` (rAF) to build animations frame-by-frame in between the browser's repaint cycle. If not, check out [this guide](https://developer.mozilla.org/en-US/docs/Games/Anatomy#building\_a\_main\_loop\_in\_javascript) as a starting point for building a render loop.

In the case of a Rive render loop, you'll be using a custom Rive API that wraps rAF, so you'll need to use `rive.requestAnimationFrame()` as well as `rive.cancelAnimationFrame()`. The structure should be similar to any other rAF loop you build for other animations, but you'll be advancing the instances you created above and aligning the artboard to the canvas as you see fit.

Start by creating your callback loop for the rAF cycle and tracking the last time since the previous rAF callback to get an elapsed time in seconds. Then, clear the canvas by using the renderer's `.clear()` API.

```javascript
let lastTime = 0;
function renderLoop(time) {
  if (!lastTime) {
    lastTime = time;
  }
  const elapsedTimeMs = time - lastTime;
  const elapsedTimeSec = elapsedTimeMs / 1000;
  lastTime = time;

  renderer.clear();

  ...

  rive.requestAnimationFrame(renderLoop);
}
rive.requestAnimationFrame(renderLoop);
```

#### Advancing Animations

A `LinearAnimationInstance` has a set of keyframes to apply to objects in an artboard. In the render loop, you'll want to call `.advance()` on the created animation instances to get those keyframes and, like the API is named, advance the animation by a certain amount of time (in seconds).

{% hint style="info" %}
Normally, you would want to advance the animation by the elapsed time calculated above to playback at “normal” speed (or rather, whatever speed is set for that timeline animation). With the low-level APIs, by controlling the render loop, you can advance the instance by a custom time value, such as half the elapsed time (to playback the animation at 0.5x speed) or even twice the elapsed time (to playback the animation at 2x speed). You could even multiply the elapsed time by -1 to run the animation direction backward.
{% endhint %}

In addition to advancing a linear animation, you need to apply the keyframe values to the properties of relevant objects in the artboard for that animation and specify the animation's mix value using the `.apply()` call. When the animation applies the interpolated values from the keyframes, it blends these values with the current values on the artboard objects. This allows you to "blend" into an animation, which is helpful if you have two animation instances applying a keyframe value on the same property of an object. The default mix value to replace the old property values with the new keyframe values should be `1`.

After applying an animation’s values to the artboard, advance the artboard (more on that below) to update the artboard's objects and resolve the property value changes.

To summarize all of this, the order of operations in advancing a linear animation is as follows:

```tsx
advance animation -> apply animation values -> advance artboard
```

See the below snippet for an example:

```tsx
function renderLoop(time) {
  if (!lastTime) {
    lastTime = time;
  }
  const elapsedTimeMs = time - lastTime;
  const elapsedTimeSec = elapsedTimeMs / 1000;
  lastTime = time;

  renderer.clear();
  animation.advance(elapsedTimeSec);
  animation.apply(1);
  artboard.advance(elapsedTimeSec);
}
```

#### Advancing State Machines

A `StateMachineInstance` is similar to the `LinearAnimationInstance` flow above, with a few differences. With state machines, you don't need to apply a mix value since you should only have one state machine instance correlated to an artboard, and mix values are determined by the transitions set between timeline animations. Additionally, the `.advance()` method updates the properties of objects on the artboard. Therefore, the order of operations for advancing a state machine is simplified to:

```tsx
advance state machine -> advance artboard
```

See the below snippet for an example:

```tsx
function renderLoop(time) {
  if (!lastTime) {
    lastTime = time;
  }
  const elapsedTimeMs = time - lastTime;
  const elapsedTimeSec = elapsedTimeMs / 1000;
  lastTime = time;

  renderer.clear();
  stateMachine.advance(elapsedTimeSec);
  artboard.advance(elapsedTimeSec);
}
```

#### Advancing the Artboard

As you've seen above, advancing the artboard will do the work of updating the relevant objects in the hierarchy after the values have been applied through animations and/or state machines. If you're controlling multiple animations at once, you only need to advance the artboard once in the render loop. If you're controlling multiple artboards for your scene in the canvas, advance each artboard as needed in the render loop.

#### Align and Render

The last bit to consider in the render loop is to set the alignment of the artboard(s), set the bounds for the drawing area and artboard, and then finally pass the rendering context to the artboard so that the artboard gets drawn in the canvas.

After advancing the artboard, call the `save()` API on the rendering context to save the state of the canvas. Then call the `align()` API on the context to provide:

1. `Fit` and `Alignment` values
2. The bounds of the canvas space to draw to
3. The bounds of the Rive content to draw within that space

[See here](https://help.rive.app/runtimes/layout) for options for `Fit` and `Alignment`. For the latter two parameters, provide an axis-aligned bounding box (AABB). See the snippet below for an example of the `align()` API.

Finally, after calling the `align()` API, pass the renderer to the artboard via the `draw()` method to draw the artboard on the canvas, then end with a call to the `restore()` API on the renderer to restore the saved state of the canvas.

{% hint style="warning" %}
If you're using `@rive-app/webgl-advanced`, you will need to add an additional call on the renderer to `flush()` to empty different buffer commands.
{% endhint %}

The last thing to do is to call on Rive's `requestAnimationFrame` with this callback to queue up the next callback for the next frame.

Altogether, this looks like the following:

```tsx
function renderLoop(time) {
  if (!lastTime) {
    lastTime = time;
  }
  const elapsedTimeMs = time - lastTime;
  const elapsedTimeSec = elapsedTimeMs / 1000;
  lastTime = time;

  ...

  renderer.clear();
  stateMachine.advance(elapsedTimeSec);
  artboard.advance(elapsedTimeSec);
  renderer.save();
  renderer.align(
    rive.Fit.contain,
    rive.Alignment.center,
    {	
      minX: 0,	
      minY: 0,
      maxX: canvas.width,
      maxY: canvas.height
    },
    artboard.bounds,
  );
  artboard.draw(renderer);
  renderer.restore();
  // Optionally make the below call if using WebGL
  // renderer.flush()
  rive.requestAnimationFrame(renderLoop);
}
rive.requestAnimationFrame(renderLoop);
```

At this point, you should be able to render Rive on the canvas!

### Cleaning Up Instances

For each of the created CPP instances, you’ll want to delete them when you are finished so that you don’t have any memory leaks in your application. Unfortunately, this is a manual operation as we cannot yet rely on the new finalizer API in browsers to be called for garbage collection. Call the `.delete()` API on any instances created from the Rive runtime when they are no longer needed. An example is shown below:

```tsx
// Created instances
const renderer = rive.makeRenderer(canvas);
const bytes = await (
  await fetch(new Request('basketball.riv'))
).arrayBuffer();
const file = (await rive.load(new Uint8Array(bytes))) as File;
const artboard = file.artboardByName('New Artboard');
const animation = new rive.LinearAnimationInstnace(
  artboard.animationByName('idle'),
  artboard
);
const stateMachine = new rive.StateMachineInstance(
  artboard.stateMachineByName('your-state-machine-name'),
  artboard
);

...

renderer.delete();
file.delete();
artboard.delete();
animation.delete();
stateMachine.delete();
```

## Examples

See below for links to examples demonstrating the use of low-level JS APIs:

* [Simple use of @rive-app/canvas-advanced](https://codesandbox.io/s/rive-canvas-advanced-api-basketball-rgted8)
* [Simple use of @rive-app/webgl-advanced](https://codesandbox.io/s/rive-webgl-advanced-api-basketball-fwtmdb)
* [A volume knob state machine](https://codesandbox.io/s/rive-volume-knob-draft-xykywk)
* [Centaur apple game](https://codesandbox.io/s/rive-canvas-advanced-api-centaur-example-exh2os)

## API References

See our [types file](https://github.com/rive-app/rive-wasm/blob/master/js/src/rive\_advanced.mjs.d.ts) for the advanced API to understand the API signatures and return types.

## Caveats

The high-level JS runtime APIs are built with the low-level APIs specified above. Along with this, the high-level JS runtime has additional affordances that make it easy for users to do some of the following things:

* Ease of playback control with APIs such as `.play()`, `.pause()`, `.stop()`, etc.
* Callbacks such as `onStateChange`, `onLoad`, etc. that would allow you to hook into specific Rive lifecycle events
* Hooking up gesture events to [Rive Listeners](https://help.rive.app/editor/state-machine#listeners)

When using Rive’s advanced JS APIs to customize how you use Rive, you will have to set up some of these affordances yourself. Take a look at how the [high-level Rive API is built here](https://github.com/rive-app/rive-wasm/blob/master/js/src/rive.ts) to get a sense of how to replicate some of these high-level affordances should you need these.

## Integrating Rive into Existing rAF Loop

If you're looking to add Rive to your existing render loop (the JS API `requestAniationFrame()`) and do not want to use the Rive-wrapped `requestAnimationFrame()` API, you can do so with an extra API call at the end of your render loop. Call the `rive.resolveAnimationFrame()` API at the end of the render loop before calling `requestAnimationFrame()` again.

See more on usage in the [Rive Parameters](rive-parameters.md#resolveanimationframe) doc.
