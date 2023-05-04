---
description: API docs for the Rive instance
---

# Rive Parameters

## Parameters

You can set any of the following parameters on the Rive object when instantiating:

```typescript
export interface RiveParameters {
  canvas: HTMLCanvasElement | OffscreenCanvas, // required
  src?: string, // one of src or buffer is required
  buffer?: ArrayBuffer, // one of src or buffer is required
  artboard?: string,
  animations?: string | string[],
  stateMachines?: string | string[],
  layout?: Layout,
  autoplay?: boolean,
  onLoad?: EventCallback,
  onLoadError?: EventCallback,
  onPlay?: EventCallback,
  onPause?: EventCallback,
  onStop?: EventCallback,
  onLoop?: EventCallback,
  onStateChange?: EventCallback,
  useOffscreenRenderer?: boolean,
  shouldDisableRiveListeners?: boolean,
}
```

* `canvas` - _(required)_ Canvas element to draw Rive animations onto.
* `src?` - _(optional)_ There are two optional ways to use `src`: either via URL to the `.riv` file, or a path to the public `.riv` asset to use. One of `src` or `buffer` must be provided.
  * URL - If you are hosting your `.riv` on some publicly accessible bucket/CDN (i.e. AWS, GCS, etc.), you can pass in the URL here.&#x20;
    * Alternatively, with ES6, you may import the `.riv` file as a data URI. Depending on your bundle loader, you may need to use a plugin (i.e `url-loader` for Webpack) to properly parse and load in `.riv` files as a data URI string. See [this project](https://github.com/zplata/rive-nextjs/blob/main/next.config.js#L8) as a basic example on how to set this up
  * Path to public asset - This is a string path to the`.riv` public asset if bundled in your application. Note that this is **not** a relative path to the asset from wherever the current JS file is in. Treat the `.riv` as any other asset bundled in your application, such as an image or font. If your JS is compiled and run at the root of your web application, you must specify the path from the root to the location of the asset. For example, if your asset is in `/public/foo.riv`, and your JS is run from the root at `/`, you would specify: `src: '/public/foo.riv'` in this property.
* `buffer?` - _(optional)_ ArrayBuffer containing the raw bytes from a .riv file. One of `src` or `buffer` must be provided.
* `artboard?` - _(optional)_ Name of the artboard to use.
* `animations?` - _(optional)_ Name or list of names of animations to play.

{% hint style="warning" %}
Currently, Rive will play the first timeline animation it finds if no `stateMachines` or `animations` parameter is provided, however, in a future major version of `rive-wasm`, the default will be to play the first state machine it finds.
{% endhint %}

* `stateMachines?` - _(optional)_ Name or list of names of state machines to load.

{% hint style="warning" %}
Note: You should only provide a single state machine string for `stateMachines`. Running multiple state machines of the same artboard at the same time may cause unintended consequences.

In a future major version of `rive-wasm`, `stateMachines` will be a singular string you pass in.
{% endhint %}

* `layout?` - _(optional)_ Layout object to define how animations are displayed on the canvas.
* `autoplay?` - _(optional)_ If true, the animation will automatically start playing when loaded. Defaults to false.
* `onLoad?` - _(optional)_ Callback that gets fired when the .riv file loads.
* `onLoadError?` - _(optional)_ Callback that gets fired when an error occurs loading the .riv file.
* `onPlay?` - _(optional)_ Callback that gets fired when the animation starts playing.
* `onPause?` - _(optional)_ Callback that gets fired when the animation pauses.
* `onStop?` - _(optional)_ Callback that gets fired when the animation stops playing.
* `onLoop?` - _(optional)_ Callback that gets fired when the animation completes a loop.
* `onStateChange?` - _(optional)_ Callback that gets fired when a state change occurs.
* `useOffscreenRenderer?` - _(optional)_ Boolean flag to determine whether to use a shared offscreen WebGL context rather than create its own WebGL context for this instance of Rive. This is only relevant for the `@rive-app/webgl` package. If you are displaying multiple Rive animations, it is highly encouraged to set this flag to `true`. Defaults to `false`.
* `shouldDisableRiveListeners?` - _(optional)_ Boolean flag to disable setting up Rive Listeners on the `<canvas>` element, thus preventing any event listeners from being set up on the element.&#x20;
  * **Note:** Rive Listeners by default are not set up on a `<canvas>` element if there is no playing state machine, or a state machine without any Rive Listeners set up on the state machine

## APIs

The following API's are available after instantiating Rive:

### play()

`play(names?: string | string[], autoplay?: true): void`

Plays a specified linear timeline animation(s) or state machine via the passed-in name. Useful if you have either programmatically called `pause()` or `stop()` or set `autoplay: false` when instantiating Rive. If no name is passed in, it plays all instantiated timeline animations or state machines (or the default animation if neither is instantiated).

**Example**:

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: false,
  canvas: document.querySelector("canvas"),
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Play the 'bumpy' state machine
  riveInstance.play("bumpy");
};
```

### pause()

`pause(names?: string | string[]): void`

Pauses a specified linear timeline animation(s) or state machine via the passed-in name. Useful if you want to programmatically pause the playing animation and pause the render loop. You may want to use this API too if the associated Rive instance's `<canvas>` element is scrolled offscreen. If no name is passed in, it pauses all instantiated timeline animations or state machines.

**Example**:

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: true,
  canvas: document.querySelector("canvas"),
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Pause the 'bumpy' state machine
  riveInstance.pause("bumpy");
};
```

### stop()

`stop(names?: string | string[]): void`

Stops a specified linear timeline animation(s) or state machine via the passed-in name. Useful if you want to programmatically stop the playing animation and render loop. You may want to use this API too if the associated Rive instance's state machine is "done," or in an exit state. If no name is passed in, it stops all instantiated timeline animations or state machines.

**Example**:

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: true,
  canvas: document.querySelector("canvas"),
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Stop the 'bumpy' state machine
  riveInstance.stop("bumpy");
};
```

### reset()

```typescript
interface RiveResetParameters {
  artboard?: string;
  animations?: string | string[];
  stateMachines?: string | string[];
  autoplay?: boolean;
}

reset(params?: RiveResetParameters): void
```

Resets the Rive Artboard, linear timeline animations, and/or the state machine from the start (or entry state) based on the parameters passed in. Implicitly, this method will also cleanup any existing instances (Artboard, Animation, and/or State Machine) created already before creating new ones. The instantiated timeline animation or state machine will play immediately depending on the `autoplay` property passed in.

**Example**:

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: true,
  canvas: document.querySelector("canvas"),
  stateMachines: "bumpy"
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Reset the 'bumpy' state machine
  riveInstance.reset({
    stateMachines: "bumpy",
    autoplay: true,
  });
};
```

### scrub()

`scrub(animationNames?: string | string[], value?: number): void`

Scrubs through (a) linear timeline animation(s) by a specified amount of seconds.

{% hint style="info" %}
Note: This will not do anything if you are playing through a state machine. This only applies if you are using instantiated animations through the `animations` property.
{% endhint %}

### cleanup()

`cleanup(): void`

This API is **important** to call, as it will stop the animation render loop, and clean up all created instances for the Rive file, artboard, linear timeline animation(s), state machine, and the renderer. The reason it's important to delete these instances is because these entities hold generated C++ references through WASM behind-the-scenes, and therefore will not automatically get garbage collected like normal JS objects, and must be "cleaned up" manually, to prevent memory leaks. Once you are done with the Rive instance (i.e., a state machine has finished running, a user is navigating off the page, etc.), call `.cleanup()` to ensure all underlying memory allocated is freed up.

{% hint style="info" %}
If you want to present a different Artboard from the same Rive file dynamically, you can call `cleanupInstances()` which will only delete the Artboard, animation, and state machine instances (but not the Rive file or renderer object, which you can still reuse).
{% endhint %}

**Example:**

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: true,
  canvas: document.querySelector("canvas"),
  stateMachines: "bumpy"
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Cleanup Rive before navigating user off page
  riveInstance.cleanup();
  window.location.href = "https://google.com";
};
```

### cleanupInstances()

Similar to `cleanup()`, but will only clean up instances for the Artboard, linear timeline animation(s), and/or state machine, thus allowing you to re-initialize a different Artboard from the same Rive file, different timeline animation(s), or a different state machine. You can do this with `reset()`.

### load()

```typescript
interface RiveLoadParameters {
  src?: string;
  buffer?: ArrayBuffer;
  autoplay?: boolean;
  artboard?: string;
  animations?: string | string[];
  stateMachines?: string | string[];
  useOffscreenRenderer?: boolean;
  shouldDisableRiveListeners?: boolean;
}

load(params: RiveLoadParameters): void
```

Replace the existing Rive instance with a new one, with potentially new parameters (including a new `src` file). This API also implicitly cleans up existing references to the old animation(s)/state machine/artboard. The backing WASM code that powers the Rive Web (JS) runtime shouldn't have to be loaded in again, so there should be no other network overhead unless you are loading in a new Rive file over the web.

### resizeToCanvas()

`resizeToCanvas(): void`

This sets the layout bounds to the current canvas size. You may want to call this if your canvas is resized.

### resizeDrawingSurfaceToCanvas()

`resizeDrawingSurfaceToCanvas(): void`

This resets the `<canvas>` width and height properties to render at its current bounding rect size (CSS `height` and `width` properties) with the browser's `devicePixelRatio` in mind. This prevents blurry output on high-dpi screens (i.e., a `<canvas>` that is 500x500 on the page may render with an internal area size of 1000x1000 while still maintaining its original canvas size). This calls `resizeToCanvas()` implicitly. It's recommended to call this in the `onLoad` callback.

{% hint style="info" %}
In a future major version of this runtime, this API may be called internally on initialization by default, with an option to opt-out if you have specific `width` and `height` properties you want to set on the canvas
{% endhint %}

```typescript
import {Rive} from '@rive-app/canvas';

const riveInstance = new Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  autoplay: true,
  canvas: document.querySelector("canvas"),
  stateMachines: "bumpy",
  onLoad: () => {
    riveInstance.resizeDrawingSurfaceToCanvas();
  },
});
```

### stateMachineInputs()

```typescript
class StateMachineInput {
  // name of the input
  public get name: string
  // value of the input (for number or boolean inputs)
  public get value: number | boolean
  // directly set the input value (for number or boolean inputs)
  public set value: number | boolean
  // Method call to fire a trigger input
  public fire(): void
}

stateMachineInputs(stateMachineName: string): StateMachineInput[]
```

Returns a list of state machine input objects from the given state machine name passed in (if the state machine has been instantiated). Use these state machine inputs to drive the state machine forward.

```typescript
const riveInstance = new rive.Rive({
  src: 'https://cdn.rive.app/animations/vehicles.riv',
  canvas: document.getElementById('canvas'),
  autoplay: true,
  stateMachines: 'bumpy',
  onLoad: () => {  
    // Get the inputs via the name of the state machine
    const inputs = riveInstance.stateMachineInputs('bumpy');
    // Find the input you want to set a value for, or trigger
    const bumpTrigger = inputs.find(i => i.name === 'bump');
    button.onclick = () => bumpTrigger.fire();
  },
});
```

### stopRendering()

`stopRendering(): void`

Stops the render loop, and can only be resumed with `startRendering()`. This is useful for situations when the `<canvas>` is not visible. The React runtime utilizes this for that particular situation implicitly.

### startRendering()

`startRendering()`

Starts the render loop if it has been previously stopped. It will have no effect if the render loop is already active.

## Debugging Tools

### contents

Unlike other APIs that you call as a method on the Rive instance, `contents` is just a property on the Rive instance you can log to the console to see an object hierarchy of what was loaded in from the Rive file. This is useful to see what Rive file was loaded in, all the artboards associated with the file, animations and state machines, state machine inputs, and more. It's also useful if you don't have access to the file to inspect in the Rive editor directly.

Example:

```typescript
const riveInstance = new rive.Rive({
  src: 'https://cdn.rive.app/animations/vehicles.riv',
  canvas: document.getElementById('canvas'),
  autoplay: true,
  stateMachines: 'bumpy',
  onLoad: () => {  
    // Log the contents of the Rive file
    console.log(riveInstance.contents);
  },
});
```

### enableFPSCounter()

```typescript
type FPSCallback = (fps: number) => void;

enableFPSCounter(fpsCallback?: FPSCallback): void
```

Reports frames-per-second (FPS) for the runtime. You can supply a callback to handle how to display or ingest the data, or if you don't provide a callback, calling this API will append a fixed-position `<div>` in the top-right corner of the page, reading out the FPS. Call this after Rive has initialized in the `onLoad` callback, or at another point in time.

### disableFPSCounter()

`disableFPSCounter(): void`

Disables the FPS reporting for the runtime.

## Other

Other API's and properties are provided to report playing/paused/stopped entities in the Rive instance.

### source

Property to get the `src` attribute in the Rive instance

### activeArtboard

Property to get the name of the active artboard

### animationNames

Property to get an array of all animation names on the loaded in Artboard (even if the animations are not specified at instantiation)

### stateMachineNames

Property to get an array of all state machine names on the loaded in Artboard (even if the state machine(s) are not specified at instantiation)

### playingAnimationNames

Property to get an array of all active playing timeline animation names on the Rive instance (if you are playing a state machine, this will not return the currently active state timeline animations)

### playingStateMachineNames

Property to get an array of all active playing state machine names on the Rive instance

### pausedAnimationNames

Property to get an array of all active paused timeline animation names on the Rive instance (if you are playing a state machine, this will not return the currently active state timeline animations)

### pausedStateMachineNames

Property to get an array of all active paused state machine names on the Rive instance

### isPlaying

Property that returns `true` if any animation is playing

### isPaused

Property that returns `true` if all instanced animations are paused

### isStopped

Property that returns `true` if no instanced animations are playing or paused

### bounds

Property that returns the bounds of the Artboard

### layout (get/set)

Property to either get or set a Rive `Layout`

**Example:**

```typescript
import {Rive, Layout, Fit, Alignment} from '@rive-app/canvas';

const riveInstance = new rive.Rive({
  src: 'https://cdn.rive.app/animations/vehicles.riv',
  canvas: document.getElementById('canvas'),
  autoplay: true,
  stateMachines: 'bumpy',
});

const buttonEl = document.querySelector("button");
buttonEl.onclick = function() {
  // Set new Layout
  riveInstance.layout = new Layout({
    fit: Fit.Cover,
    alignment: Alignment.TopCenter,
  });
};
```

