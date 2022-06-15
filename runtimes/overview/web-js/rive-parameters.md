# Rive Parameters

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
}
```

* `canvas` - _(required)_ Canvas element to draw Rive animations onto.
* `src?` - _(optional)_ File path or URL to the .riv file to use. One of `src` or `buffer` must be provided.
* `buffer?` - _(optional)_ ArrayBuffer containing the raw bytes from a .riv file. One of `src` or `buffer` must be provided.
* `artboard?` - _(optional)_ Name of the artboard to use.
* `animations?` - _(optional)_ Name or list of names of animations to play.
* `stateMachines?` - _(optional)_ Name or list of names of state machines to load.
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
