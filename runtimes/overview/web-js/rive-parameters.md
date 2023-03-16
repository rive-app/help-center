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
* `src?` - _(optional)_ There are two optional ways to use `src`: either via URL to the `.riv` file, or a path to the public `.riv` asset to use. One of `src` or `buffer` must be provided.
  * URL - If you are hosting your `.riv` on some publicly accessible bucket/CDN (i.e. AWS, GCS, etc.), you can pass in the URL here.&#x20;
    * Alternatively, with ES6, you may import the `.riv` file as a data URI. Depending on your bundle loader, you may need to use a plugin (i.e `url-loader` for Webpack) to properly parse and load in `.riv` files as a data URI string. See [this project](https://github.com/zplata/rive-nextjs/blob/main/next.config.js#L8) as a basic example on how to set this up
  * Path to public asset - This is a string path to the`.riv` public asset if bundled in your application. Note that this is **not** a relative path to the asset from wherever the current JS file is in. Treat the `.riv` as any other asset bundled in your application, such as an image or font. If your JS is compiled and run at the root of your web application, you must specify the path from the root to the location of the asset. For example, if your asset is in `/public/foo.riv`, and your JS is run from the root at `/`, you would specify: `src: '/public/foo.riv'` in this property.
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
