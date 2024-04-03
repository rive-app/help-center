---
description: Rive React API
---

# Parameters and Return Values

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/parameters-and-return-values/docJlDMNulDh).
{% endhint %}

## Hooks

### useRive

The `useRive` hook is the recommended way to hook into the Rive runtime for full control, especially when using the Rive State Machine. See below for parameters to pass in and the return values.

`useRive(riveParams: UseRiveParameters, opts: UseRiveOptions): RiveState`

* `riveParams` - See below for a [set of parameters](parameters-and-return-values.md#riveparams) passed to the `Rive` object at instantiation from the Web runtime. `null` and `undefined` can be passed to conditionally display the .riv file
* `opts` - _(Optional)_ See below for a [set of options](parameters-and-return-values.md#opts) specific to `rive-react`

#### Parameters

**UseRiveParameters**

Most of these parameters come from the underlying web runtime configuration items for the Rive object, with the exception of supplying a `canvas` element. See [rive-parameters.md](../web-js/rive-parameters.md "mention") for all the parameters you can supply in this object.

{% hint style="warning" %}
If you supply an `onLoad` callback in the parameters, you may not have access to the `rive` instance yet. The React runtime uses `onLoad` internally to setState with the `rive` instance, and therefore may not be populated by the time it reaches a consumer-supplied callback. We recommend using a `useEffect` in place of `onLoad` to reliably use the `rive` instance if you are looking for a similar method. In a future version of the web runtime, we may supply the `rive` instance in the parameters of your callback so you can supply an `onLoad` here.
{% endhint %}

**UseRiveOptions**

* `useDevicePixelRatio` - _(optional)_ If `true`, the hook will scale the resolution of the animation based on the [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio). Defaults to `true`. NOTE: Requires the `setContainerRef` ref callback to be passed to an element wrapping a canvas element. If you use the `RiveComponent`, then this will happen automatically
* `fitCanvasToArtboardHeight` - _(optional)_ If `true`, then the canvas will resize based on the height of the artboard. Defaults to `false`
* `useOffscreenRenderer` - _(optional)_ If `true`, the Rive instance will share (or create if one does not exist) an offscreen `WebGL` context. This allows you to display multiple Rive animations on one screen to work around some browser limitations regarding multiple concurrent WebGL contexts. If `false`, each Rive instance will have its own dedicated `WebGL` context and you may need to be cautious of the browser limitations just mentioned. We recommend **not** changing this default prop, so you don't have to manage WebGL contexts. Destroying a React component does not guarantee the browser cleans up the WebGL context that was created when the canvas was mounted. Only relevant when using `@rive-app/react-webgl`. Defaults to `true`

#### Return Values

**RiveState**

* `canvas` - Canvas element the Rive instance is rendered onto
* `container` - Container element of the canvas that Rive instance is rendered onto
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

{% hint style="warning" %}
The return value which is the state machine input may not be immediately available due to the need for the `rive` instance to resolve first. You may want to use a `useEffect` to watch for when the `rive` instance and the return value of the `useStateMachineInput` hook has value
{% endhint %}

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

### useResizeCanvas

The `useResizeCanvas` hook is an optional utility hook to resize the `<canvas>` element to its parent container element's size, while also resetting the appropriate surface area size of the canvas as well. This is useful when you don't want to use the `useRive` hook to render your Rive, and are perhaps using the web JS runtime in your React apps, but still want the ability to scale the `<canvas>` to its parent appropriately.

{% hint style="success" %}
This hook is already internally used in the Rive React runtime, so if you use the `useRive` hook or the default exported `<RiveComponent />` to render your Rive, you don't need to consume this hook yourself.
{% endhint %}

`useResizeCanvas(resizeProps: UseResizeCanvasProps): void`

* `resizeProps` - See below for a set of properties to set onto this object parameter

#### Parameters

**UseResizeCanvasProps**

* `riveLoaded: boolean` -  If `true`, the Rive instance has been created and the Rive file have been parsed. This ensures the hook does not prematurely scale the `<canvas>` element. Defaults to `false`
* `canvasRef: MutableRefObject<HTMLCanvasElement | null>` - React `Ref` for the `<canvas>` element where Rive will be rendering onto
* `containerRef: MutableRefObject<HTMLElement | null>` - React `Ref` for the canvas's parent container element
* `onCanvasHasResized?: () => void` (Optional) Callback to be invoked after the canvas has been resized due to a resize of its parent container. This is where you would want to reset the layout dimensions for the Rive renderer to dictate the new min/max bounds of the canvas.
  * Using the high-level JS runtime, this might be a simple call to `rive.resizeToCanvas()`&#x20;
  * Using the low-level JS runtime, this might be invoking the renderer's `.align()` method, with the Layout and min/max X/Y values of the canvas.
* `options?: Partial` - (Optional) Options passed to the useRive hook (see `UseRiveOptions` further up the document)
* `artboardBounds?: Bounds` - (Optional) AABB bounds of the Artboard; you only need to supply this if `options.fitCanvasToArtboardHeight` is set to `true`.

## Components

### \<RiveComponent />

The `RiveComponent` default export and the `RiveComponent` returned from the `useRive` hook are both to be rendered in the JSX of a component. As noted previously, all attributes and event handlers that can be passed to a `canvas` element can also be passed to the `Rive` component and used in the same manner.

One thing to note is that `style`/`className` props set on the component will be passed onto the containing `<div>` element, rather than the underlying `<canvas>` itself. The reason for this is that the containing `<div>` element handles resizing and layout for you, and thus, all styles should be passed onto this element.\
\
The `<canvas>` element will still receive any other props passed into the component, such as `aria-*` attributes, `role`'s, etc. You can also set children content inside the component for fallback scenarios where the `<canvas>` element cannot be shown
