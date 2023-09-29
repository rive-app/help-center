---
description: React runtime for Rive
---

# React

## Overview

This guide documents how to get started using the React runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-react).\
\
This library contains a React component, as well as custom hooks to help integrate Rive into your web application (types included). Under the hood, this runtime is a React-friendly wrapper around the `@rive-app/canvas` runtime, exposing types, and Rive instance functionality.

## Quick Start

See our [quick start example](https://codesandbox.io/s/rive-quick-start-react-qqkn52?file=/src/App.js) that shows how to play a Rive animation with React.

{% embed url="https://codesandbox.io/embed/rive-quick-start-react-qqkn52?fontsize=14&hidenavigation=1&theme=dark" %}
Interactive Rive React Quick Start
{% endembed %}

## Getting Started

Follow the steps below for a quick start on integrating Rive into your React app.

### 1. Install the dependency

The Rive React runtime allows for two main options based on which backing renderer you need.

* **(Recommended)** `@rive-app/react-canvas` - Wraps the `@rive-app/canvas` dependency. Unless you specifically need a `WebGL` backing renderer, we recommend you use this dependency when using Rive in your apps for quick and fast usage.
* `@rive-app/react-webgl` - Wraps the `@rive-app/webgl` dependency. In the future, we may have advanced rendering features that are only supported by using `WebGL`. At the moment, however, due to the size of the dependency (with Skia), we do not recommend it unless you have specific needs here. We are currently working on improving the performance and size with the [Rive Renderer](https://rive.app/renderer).

{% code title="" %}
```bash
npm i --save @rive-app/react-canvas
```
{% endcode %}

### 2a. Render the Rive component

Rive React provides a basic component as its default import for displaying simple animations with a few props you can set such as artboard and layout. Include the code below in your React project to test out an example Rive animation.

```javascript
import Rive from '@rive-app/react-canvas';

export const Simple = () => (
  <Rive
    src="https://cdn.rive.app/animations/vehicles.riv"
    stateMachines="bumpy"
  />
);
```

See [here](parameters-and-return-values.md) for more on the parameters and return values of the `<Rive />` component.

### 2b. Using the useRive hook

In many cases, you may not only need the React component to render your animation but also the `rive` object instance that controls it as well. The Rive object instance allows you to tap into APIs for:

* Setting Rive Text values dynamically
* Subscribing to Rive Events with your own callbacks
* Controlling animation playback (i.e. pause and play)
* ... and [much more](https://github.com/rive-app/rive-wasm)

The `useRive` hook returns both this `rive` instance, as well as the React component that mounts the underlying `<canvas>` element that Rive will draw onto.

```javascript
import { useRive } from '@rive-app/react-canvas';

export default function Simple() {
  const { rive, RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    stateMachines: "bumpy",
    autoplay: false,
  });

  return (
    <RiveComponent
      onMouseEnter={() => rive && rive.play()}
      onMouseLeave={() => rive && rive.pause()}
    />
  );
}
```

{% hint style="info" %}
**Note:** Rive will not instantiate until the `<RiveCopmonent />` is rendered out, as the underlying `<canvas>` element needs to be present in the DOM.\
\
Also, keep in mind that the canvas size depends on the container it's placed within. Initially, this is 0x0.  Either pass a `className` to `RiveComponent` or wrap `RiveComponent` with an appropriately sized container.
{% endhint %}

See [here](parameters-and-return-values.md) for more on the parameters and return values of `useRive`.

Additionally, explore subsequent runtime pages to learn how to control animation playback, state machines, and more.

#### Rendering Considerations with useRive

At this time, we highly recommend isolating your usage of `useRive` to its own wrapper component if you plan on conditionally rendering the `<RiveComponent />` returned from the `useRive` hook. This is due to Rive being instanced when the component is mounted and the rendering context associated with a specific underlying `<canvas>` element. When React tries to unmount/re-render, you may end up with the animation restarting or not displaying when a new `<canvas>` is mounted.\
\
By isolating `useRive` to its own wrapper component, Rive will have a chance to properly clean up, and restart the animation with a new canvas. In a parent component, you can then conditionally render the wrapper component based on any state or prop-based logic.

Check out this example to see this pattern in use: [https://codesandbox.io/s/userive-wrapper-pattern-zx2h3j](https://codesandbox.io/s/userive-wrapper-pattern-zx2h3j)

## Resources

Github: [https://github.com/rive-app/rive-react](https://github.com/rive-app/rive-react)\
Types: [https://github.com/rive-app/rive-react/blob/main/src/types.ts](https://github.com/rive-app/rive-react/blob/main/src/types.ts)\
Examples:

* Simple skinning example: [https://codesandbox.io/s/rive-skins-7tmtxm](https://codesandbox.io/s/rive-skins-7tmtxm)
* Storybook demo: [https://rive-app.github.io/rive-react/](https://rive-app.github.io/rive-react/)
* Animated Login Form:
  * Code: [https://github.com/rive-app/rive-use-cases/tree/main/lib/components/login-form](https://github.com/rive-app/rive-use-cases/tree/main/lib/components/login-form)
  * Demo: [https://rive-app.github.io/rive-use-cases/?path=/story/example-loginformcomponent--primary](https://rive-app.github.io/rive-use-cases/?path=/story/example-loginformcomponent--primary)
* Rise of the Robots:
  * Code: [https://github.com/PaulieScanlon/rise-of-the-robots](https://github.com/PaulieScanlon/rise-of-the-robots)
  * Demo: [https://www.gatsbyjs.com/demos/rise-of-the-robots/](https://www.gatsbyjs.com/demos/rise-of-the-robots/)

