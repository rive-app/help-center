# Parameters and Return Values

## Hooks

### useRive

The `useRive` hook is the recommended way to hook into the Rive runtime for full control, especially when using the Rive State Machine. See below for parameters to pass in and the return values.

`useRive(riveParams: UseRiveParameters, opts: UseRiveOptions): RiveState`

* `riveParams` - See below for a [set of parameters](parameters-and-return-values.md#riveparams) passed to the `Rive` object at instantiation from the Web runtime. `null` and `undefined` can be passed to conditionally display the .riv file
* `opts` - _(Optional)_ See below for a [set of options](parameters-and-return-values.md#opts) specific to `rive-react`

#### Parameters

**UseRiveParameters**

* `src?` - _(optional)_ File path or URL to the .riv file to use. One of `src` or `buffer` must be provided
* `buffer?` - _(optional)_ ArrayBuffer containing the raw bytes from a .riv file. One of `src` or `buffer` must be provided
* `artboard?` - _(optional)_ Name of the artboard to use
* `animations?` - _(optional)_ Name or list of names of animations to play
* `stateMachines?` - _(optional)_ Name or list of names of state machines to load
* `layout?` - _(optional)_ Layout object to define how animations are displayed on the canvas. See our [web runtime docs](https://github.com/rive-app/rive-wasm#layout) for more details
* `autoplay?` - _(optional)_ If true, the animation will automatically start playing when loaded. Defaults to false
* `onLoad?` - _(optional)_ Callback that gets fired when the .rive file loads
* `onLoadError?` - _(optional)_ Callback that gets fired when an error occurs loading the .riv file
* `onPlay?` - _(optional)_ Callback that gets fired when the animation starts playing
* `onPause?` - _(optional)_ Callback that gets fired when the animation pauses
* `onStop?` - _(optional)_ Callback that gets fired when the animation stops playing
* `onLoop?` - _(optional)_ Callback that gets fired when the animation completes a loop
* `onStateChange?` - _(optional)_ Callback that gets fired when a state change occurs

Most of these parameters come from the underlying web runtime configuration items for the Rive object. See the [types here](https://github.com/rive-app/rive-wasm/blob/master/js/src/rive.ts#L842).

**UseRiveOptions**

* `useDevicePixelRatio` - _(optional)_ If `true`, the hook will scale the resolution of the animation based on the [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio). Defaults to `true`. NOTE: Requires the `setContainerRef` ref callback to be passed to an element wrapping a canvas element. If you use the `RiveComponent`, then this will happen automatically
* `fitCanvasToArtboardHeight` - _(optional)_ If `true`, then the canvas will resize based on the height of the artboard. Defaults to `false`
* `useOffscreenRenderer` - _(optional)_ If `true`, the Rive instance will share (or create if one does not exist) an offscreen `WebGL` context. This allows you to display multiple Rive animations on one screen to work around some browser limitations regarding multiple concurrent WebGL contexts. If `false`, each Rive instance will have its own dedicated `WebGL` context and you may need to be cautious of the browser limitations just mentioned. We recommend **not** changing this default prop, so you don't have to manage WebGL contexts. Destroying a React component does not guarantee the browser cleans up the WebGL context that was created when the canvas was mounted. Only relevant when using `@rive-app/react-webgl`. Defaults to `true`

#### Return Values

**RiveState**

* `canvas` - Canvas element the Rive instance is attached to
* `setCanvasRef` - Ref callback to be passed to the canvas element
* `setContainerRef` - Ref callback to be passed to the container element of the canvas. This is optional, however, if not used then the hook will not take care of automatically resizing the canvas to its outer container if the window resizes
* `rive` - Newly created Rive instance from the Web runtime
* `RiveComponent` - JSX element to render the Rive instance in the DOM

{% hint style="info" %}
In most cases, you will just need to grab the `RiveComponent` and `rive` return values from the `useRive` hook. Setting the canvas ref and container ref is only needed if you need to control the canvas/containing element yourself.
{% endhint %}

### useStateMachineInput

The `useStateMachineInput` hook is the recommended way to grab references to Rive State Machine inputs, both for reading input values, and setting (or triggering) them. See below for parameters to pass in and the return value.

`useStateMachineInput(rive: Rive | null, stateMachineName?: string, inputName?: string, initialValue?: number | boolean): StateMachineInput | null`

#### Parameters

* `rive` - The 1st parameter is the Rive object instantiated - this can be retrieved via the `useRive` hook
* `stateMachineName?` - _(optional)_ Name of the state machine to grab the input from
* `inputName?` - _(optional)_ Name of a single state machine input to grab a reference to
* `initialValue?` - _(optional)_ Initial value to set on the input

#### Return Values

This hook returns a default instance of a `StateMachineInput`.

**StateMachineInput**

* `name` (get) - Access the name of the input
* `value` (get and set) - Access the value of the input, and set the value of the input via this property
* `fire()` - Fires off a trigger input

See the [State Machines page](../../state-machines.md) to see more usage of this hook.

## \<RiveComponent />

The `RiveComponent` default export and the `RiveComponent` returned from the `useRive` hook are both to be rendered in the JSX of a component. As noted previously, all attributes and event handlers that can be passed to a `canvas` element can also be passed to the `Rive` component and used in the same manner.

One thing to note is that `style`/`className` props set on the component will be passed onto the containing `<div>` element, rather than the underlying `<canvas>` itself. The reason for this is that the containing `<div>` element handles resizing and layout for you, and thus, all styles should be passed onto this element.\
\
The `<canvas>` element will still receive any other props passed into the component, such as `aria-*` attributes, `role`'s, etc.



