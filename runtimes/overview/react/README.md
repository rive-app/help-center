---
description: React runtime for Rive
---

# React

## Overview

This guide documents how to get started using the React runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-react).\
\
This library contains a React component, as well as custom hooks to help integrate Rive into your web application (types included). Under the hood, this runtime is a React-friendly wrapper around the `@rive-app/webgl` runtime, exposing types and Rive instance functionality.

## Getting Started

Follow the steps below for a quick start on integrating Rive into your React app.

### 1. Install the dependency

The Rive React runtime allows for 2 main options based on which backing renderer you need.

* **(Recommended)** `@rive-app/react-canvas` - Wraps the `@rive-app/canvas` dependency. Unless you specifically need a `WebGL` backing renderer, we recommend you use this dependency when using Rive in your apps for quick and fast usage.
* `@rive-app/react-webgl` - Wraps the `@rive-app/webgl` dependency. In the future, we may have advanced rendering features that are only supported by using `WebGL` . We are currently working on improving the performance with this backing renderer.

{% code title="" %}
```bash
npm i --save @rive-app/react-canvas
```
{% endcode %}

\
Before v2.0.0, the React runtime was powered by the `rive-react` dependency and is still published today. Despite being actively published, It contains a larger bundle, as it has dependencies for both `@rive-app/canvas` and `@rive-app/webgl` . Starting in v2.0.0, we recommend you switch to one of the above dependencies instead.

### 2a. Render the Rive component

Rive React provides a basic component as its default import for displaying simple animations with a few props you can set such as artboard and layout. Include the code below in your React project to test out an example Rive animation.

```javascript
import Rive from '@rive-app/react-canvas';

export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" />
);
```

See [here](parameters-and-return-values.md) for more on the parameters and return values of the `<Rive />` component.

### 2b. Using the useRive hook

In many cases, you may not only need the React component to render your animation, but also the Rive object instance that controls it as well. The `useRive` hook provides both of these. This hook returns a component and a `rive` object which gives you control of the current Rive file.

See more in the [JS docs](https://github.com/rive-app/rive-wasm) to understand what you can control with the `rive` object.

```javascript
import { useRive } from '@rive-app/react-canvas';

export default function Simple() {
  const { rive, RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
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
Rive will not instantiate until the `RiveComponent` is rendered in the JSX, as the underlying `<canvas>` element needs to be present in the DOM. Also, keep in mind that the canvas size depends on the container it's placed within. Initially, this is 0x0.  Either pass a `className` to `RiveComponent` or wrap `RiveComponent` with an appropriately sized container.
{% endhint %}

See [here](parameters-and-return-values.md) for more on the parameters and return values of `useRive`.

Additionally, explore subsequent runtime pages to learn how to control animation playback, state machines, and more.

## Resources

Github: [https://github.com/rive-app/rive-react](https://github.com/rive-app/rive-react)\
Types: [https://github.com/rive-app/rive-react/blob/main/src/types.ts](https://github.com/rive-app/rive-react/blob/main/src/types.ts)\
Examples:

* Storybook demo: [https://rive-app.github.io/rive-react/](https://rive-app.github.io/rive-react/)
* Animated Login Form:
  * Code: [https://github.com/rive-app/rive-use-cases/tree/main/lib/components/login-form](https://github.com/rive-app/rive-use-cases/tree/main/lib/components/login-form)
  * Demo: [https://rive-app.github.io/rive-use-cases/?path=/story/example-loginformcomponent--primary](https://rive-app.github.io/rive-use-cases/?path=/story/example-loginformcomponent--primary)
* Rise of the Robots:
  * Code: [https://github.com/PaulieScanlon/rise-of-the-robots](https://github.com/PaulieScanlon/rise-of-the-robots)
  * Demo: [https://www.gatsbyjs.com/demos/rise-of-the-robots/](https://www.gatsbyjs.com/demos/rise-of-the-robots/)

