---
description: Runtime versions that start supporting new Rive features
---

# Feature Support

As our Rive editor grows to support more features and toolsets in making Rive assets, sometimes our runtimes need to be updated to support these updates. These editor additions may or may not introduce an API change or feature. You may need to update the runtime version to support these features. Check below to see if a feature your Rive asset takes advantage of is supported at runtime yet or not. We generally recommend you are on the latest version of the runtimes to take advantage of follow-on bug fixes and new features.\
\
We may include notes on migrating to newer versions if a new feature warrants recent API changes.

## Nested Inputs and Nested Events

To add support for nested inputs and nested events, bump the appropriate versions noted below to support this new feature. For more information, see [nested-artboards.md](../editor/fundamentals/nested-artboards.md "mention").

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.7.0</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.5.0</code></td></tr><tr><td>React Native</td><td>❗️Coming soon</td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.12.3</code></td></tr><tr><td>iOS/macOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.6.0</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 8.7.0</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Out-of-band Assets

To load assets dynamically, instead of embedded in the `riv` file, bump the appropriate versions noted below to support this new feature. For runtime specific information, see [loading-assets.md](loading-assets.md "mention").

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.7.0</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.5.0</code></td></tr><tr><td>React Native</td><td>❗️Coming soon</td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.12.0</code></td></tr><tr><td>iOS/macOS</td><td>❗️Coming soon</td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 8.6.1</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Rive Events Support

To receive reported Rive Events at runtime, bump to the appropriate versions noted below to support this new feature. For runtime specific information, see [rive-events.md](rive-events.md "mention").

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.4.3</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.3.3</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 6.1.0</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.11.17</code></td></tr><tr><td>iOS/macOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.3.1</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 8.4.0</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Text Support

To take advantage of basic Text support at runtime, bump to the appropriate versions noted below to support this new feature. For runtime specific information, see [text.md](text.md "mention").

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.1.3</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.1.3</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 6.0.3</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.11.14</code></td></tr><tr><td>iOS/macOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.1.5</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 8.1.3</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

{% hint style="info" %}
Note that we will proactively update the above versions as additional APIs on runtimes expose ways to dynamically set text (high and low-level), among other related features.
{% endhint %}

## Follow Path Support

To take advantage of follow path at runtime, bump to the appropriate versions noted below to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.2.4</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.55</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.0.0</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.11.6</code></td></tr><tr><td>iOS/macOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.5</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 6.0.1</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Interpolation on States Support

To take advantage of Interpolation on States at runtime, bump to the appropriate versions noted below to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.2.1</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.54</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.1.2</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.11.4</code></td></tr><tr><td>iOS/macOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.4</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.1.5</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Joystick Support

To allow any Joystick configuration from the editor to reflect at runtime, bump to the appropriate versions noted below to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.1.9</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.49</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.1.0</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.11.1</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.1</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 5.0.0</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Solo Support

To take advantage of Solos at runtime, bump to the appropriate versions noted below to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code> and <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.1.2</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.42</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.4</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.10.4</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.1.9</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.4.0</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Speed on States Support

If you set speed values on states in the state machine, bump to the appropriate versions noted below for the runtime being used to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.102</code></td></tr><tr><td>(Web) <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.98</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.38</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.1</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.10.3</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.1.7</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.2.7</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Graph Editor Support

If you use the timeline graph editor in the Rive editor and export a `.riv` file for runtime usage, bump to the appropriate versions noted below for the runtime being used to support this new feature.

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.97</code></td></tr><tr><td>(Web) <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.93</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.34</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.0.1</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.10.0</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.1.3</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 4.2.2</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

## Listeners Support

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.65</code></td></tr><tr><td>(Web) <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.62</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.6</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.38</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.9.0</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.0.21</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.8</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

**Notes**

* `rive-react` - Starting in `v3.0.0`  the React runtime has split into two different published packages; `@rive-app/react-canvas` and `@rive-app/react-webgl`, each wrapping the respective `@rive-app/canvas` and `@rive-app/webgl` web runtimes. We recommend using `@rive-app/react-canvas`
* `@rive-app/webgl` - There is a new flag here, `useOffscreenRenderer` which is off by default. This flag will allow you to work around the various browser constraints on the number of WebGL contexts created. We **highly recommend** setting this option to `true` when instantiating Rive in the high-level API. See more here: [https://github.com/rive-app/rive-wasm#other-notes](https://github.com/rive-app/rive-wasm#other-notes).
* `rive-react-native` - Starting in `v3.0.0`, it will have a minimum iOS `14.0` support

{% hint style="info" %}
No extra code is needed to support listeners, and you do not need to invoke listeners via event listener/detector code at runtime. If the Rive file has a listener as part of the state machine at design time, the runtime library has implicit event listener/detector code to trigger the listeners at the appropriate time
{% endhint %}

## Mesh Deformation Support

<table><thead><tr><th width="368">Runtime</th><th>Version</th></tr></thead><tbody><tr><td>(Web) <code>@rive-app/canvas</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.47</code></td></tr><tr><td>(Web) <code>@rive-app/webgl</code></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.44</code></td></tr><tr><td>React</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 3.0.1</code></td></tr><tr><td>React Native</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.1.37</code></td></tr><tr><td>Flutter</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 0.8.4</code></td></tr><tr><td>iOS</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 1.0.18</code></td></tr><tr><td>Android</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span><code>>= 2.0.24</code></td></tr><tr><td>C++</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> Supported</td></tr></tbody></table>

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
