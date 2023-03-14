---
description: Runtime versions that start supporting new Rive features
---

# Feature Support

As our Rive editor grows to support more features and toolsets in making Rive assets, sometimes our runtimes need to be updated to support these updates. These editor additions may or may not introduce an API change or feature. You may need to update the runtime version to support these features. Check below to see if a feature your Rive asset takes advantage of is supported at runtime yet or not. We generally recommend you are on the latest version of the runtimes to take advantage of follow-on bug fixes and new features.\
\
We may include notes on migrating to newer versions if a new feature warrants recent API changes.

## Graph Editor Support

If you use the timeline graph editor in the Rive editor and export a `.riv` file for runtime usage, bump to the appropriate versions noted below for the runtime being used to support this new feature.

| Runtime                  | Version                         |
| ------------------------ | ------------------------------- |
| (Web) `@rive-app/canvas` | :white\_check\_mark:`>= 1.0.97` |
| (Web) `@rive-app/webgl`  | :white\_check\_mark:`>= 1.0.93` |
| React                    | :white\_check\_mark:`>= 3.0.34` |
| React Native             | :white\_check\_mark:`>= 4.0.0`  |
| Flutter                  | :white\_check\_mark:`>= 0.10.0` |
| iOS                      | :white\_check\_mark:`>= 3.1.3`  |
| Android                  | :white\_check\_mark:`>= 4.2.2`  |
| C++                      | :white\_check\_mark: Supported  |

## Listeners Support

| Runtime                  | Version                         |
| ------------------------ | ------------------------------- |
| (Web) `@rive-app/canvas` | :white\_check\_mark:`>= 1.0.65` |
| (Web) `@rive-app/webgl`  | :white\_check\_mark:`>= 1.0.62` |
| React                    | :white\_check\_mark:`>= 3.0.6`  |
| React Native             | :white\_check\_mark:`>= 3.0.38` |
| Flutter                  | :white\_check\_mark:`>= 0.9.0`  |
| iOS                      | :white\_check\_mark:`>= 2.0.21` |
| Android                  | :white\_check\_mark:`>= 3.0.8`  |
| C++                      | :white\_check\_mark: Supported  |

**Notes**

* `rive-react` - Starting in `v3.0.0`  the React runtime has split into two different published packages; `@rive-app/react-canvas` and `@rive-app/react-webgl`, each wrapping the respective `@rive-app/canvas` and `@rive-app/webgl` web runtimes. We recommend using `@rive-app/react-canvas`
* `@rive-app/webgl` - There is a new flag here, `useOffscreenRenderer` which is off by default. This flag will allow you to work around the various browser constraints on the number of WebGL contexts created. We **highly recommend** setting this option to `true` when instantiating Rive in the high-level API. See more here: [https://github.com/rive-app/rive-wasm#other-notes](https://github.com/rive-app/rive-wasm#other-notes).
* `rive-react-native` - Starting in `v3.0.0`, it will have a minimum iOS `14.0` support

{% hint style="info" %}
No extra code is needed to support listeners, and you do not need to invoke listeners via event listener/detector code at runtime. If the Rive file has a listener as part of the state machine at design time, the runtime library has implicit event listener/detector code to trigger the listeners at the appropriate time
{% endhint %}

## Mesh Deformation Support

| Runtime                  | Version                         |
| ------------------------ | ------------------------------- |
| (Web) `@rive-app/canvas` | :white\_check\_mark:`>= 1.0.47` |
| (Web) `@rive-app/webgl`  | :white\_check\_mark:`>= 1.0.44` |
| React                    | :white\_check\_mark:`>= 3.0.1`  |
| React Native             | :white\_check\_mark:`>= 2.1.37` |
| Flutter                  | :white\_check\_mark:`>= 0.8.4`  |
| iOS                      | :white\_check\_mark:`>= 1.0.18` |
| Android                  | :white\_check\_mark:`>= 2.0.24` |
| C++                      | :white\_check\_mark: Supported  |

**Notes**

* `rive-react` - Starting in `v3.0.0`  the React runtime has split into two different published packages; `@rive-app/react-canvas` and `@rive-app/react-webgl`, each wrapping the respective `@rive-app/canvas` and `@rive-app/webgl` web runtimes. We recommend using `@rive-app/react-canvas`
* `@rive-app/webgl` - There is a new flag here, `useOffscreenRenderer` which is off by default. This flag will allow you to work around the various browser constraints on the number of WebGL contexts created. We **highly recommend** setting this option to `true` when instantiating Rive in the high-level API. See more here: [https://github.com/rive-app/rive-wasm#other-notes](https://github.com/rive-app/rive-wasm#other-notes)
* Regarding web-based runtimes and meshes:
  * Keep in mind that as meshes grow across larger screen areas, they become more resource-heavy on some devices
  * Avoid complex transforms repeatedly on the `<canvas>` elements that display Rive animations (or `<RiveComponent />` in the React runtimes)
  * We recommend using `@rive-app/webgl` to display mesh on Firefox for best performance

## Raster Asset Support

| Runtime                      | Version                         |
| ---------------------------- | ------------------------------- |
| \*\*(Web) `@rive-app/canvas` | :white\_check\_mark:`>= 1.0.2`  |
| \*\*(Web) `@rive-app/webgl`  | :white\_check\_mark:`>= 1.0.2`  |
| \*\*React                    | :white\_check\_mark:`>= 0.0.28` |
| React Native                 | :white\_check\_mark:`>= 2.1.36` |
| Flutter                      | :white\_check\_mark:`>= 0.8.1`  |
| iOS                          | :white\_check\_mark:`>= 1.0.1`  |
| Android                      | :white\_check\_mark:`>= 2.0.5`  |
| C++                          | :white\_check\_mark: Supported  |

**Notes**

* For the web runtimes, we have deprecated `rive-js` and moved to a multi-package setup for a JS runtime that runs against the `context2d` and `webgl` renderer:
  * Note that the new web runtime packages all support raster assets, and the high-level JS API did not change in this migration
  * `@rive-app/canvas` - Renders Rive with a `CanvasRenderingContext2D` renderer
  * `@rive-app/webgl` - Renders Rive with a `WebGLRenderingContext` renderer.
  * We recommend using the `@rive-app/canvas` dependency, but [check here](https://github.com/rive-app/rive-wasm/blob/master/WEB\_RUNTIMES.md) to see which might fit your needs better
