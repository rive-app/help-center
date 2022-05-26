---
description: >-
  The Rive runtimes are open-source libraries that allow you to load and control
  your animations in apps, games, and websites.
---

# Runtimes Quick Start

{% tabs %}
{% tab title="Web" %}
## 1. Add the Rive library

Add a script tag to a web page:

```javascript
<script src="https://unpkg.com/@rive-app/canvas@1.0.47"></script>
```

\
Alternatively, you can import our recommended web runtime package via NPM in your project:

```
npm install @rive-app/canvas

// example.js
import rive from "@rive-app/canvas";
```



If you need more details and options for a web runtime, checkout the installation section in the [README](https://github.com/rive-app/rive-wasm#installing) docs.

## 2. Create a canvas

Create a canvas element where you want the Rive file to display in your HTML:

```javascript
<canvas id="canvas" width="500" height="500"></canvas>
```

## 3. Create a Rive object

Create a new instance of a Rive object, providing the following properties:

* `src` - A string of the URL where the `.riv` file is hosted (like in the example below), or the path to a local `.riv` file
* `canvas` - The canvas element on which you want the animation to render
* `autoplay` - Boolean for whether you want the default animation to play

```javascript
<script>
    new rive.Rive({
        src: "https://cdn.rive.app/animations/vehicles.riv",
        // Or the path to a local Rive asset
        // src: './example.riv',
        canvas: document.getElementById("canvas"),
        autoplay: true
    });
</script>
```

## Complete example

Putting this altogether, you can load this example Rive animation in one HTML file:

```javascript
<html>
  <head>
    <title>Rive Hello World</title>
  </head>
  <body>
    <canvas id="canvas" width="500" height="500"></canvas>

    <script src="https://unpkg.com/@rive-app/canvas@1.0.47"></script>
    <script>
      new rive.Rive({
        src: "https://cdn.rive.app/animations/vehicles.riv",
        canvas: document.getElementById("canvas"),
        autoplay: true
      });
    </script>
  </body>
</html>
```

**Try it out on your own:** Check out this [CodeSandbox](https://codesandbox.io/s/rive-plain-js-sandbox-1ddrc?file=/src/index.js) for a small setup to test your own Rive files in!

**API Docs:** Check out the web runtime [README docs](https://github.com/rive-app/rive-wasm#readme) for more on what you can do with the Rive API.
{% endtab %}

{% tab title="React" %}
## Install the rive-react package

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

## Component

Rive React provides a basic component as its default import for displaying simple animations.

```javascript
import Rive from '@rive-app/react-canvas'

export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" />
);
```

### Props

* `src`: File path or URL to the .riv file to display.
* `artboard`: _(optional)_ Name to display.
* `animations`: _(optional)_ Name or list of names of animations to play.
* `layout`: _(optional)_ Layout object to define how animations are displayed on the canvas.
* _All attributes and event handlers that can be passed to a `div` element can also be passed to the `Rive` component and used in the same manner._

## useRive Hook

For more advanced usage, the `useRive` hook is provided. The hook will return a component and a `Rive` object which gives you control of the current rive file.

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

### Parameters

* `riveParams`: Set of parameters that are passed to the Rive.js `Rive` class constructor. `null` and `undefined` can be passed to conditionally display the .riv file.
* `opts`: Rive React-specific options.

### Return Values

* `RiveComponent`: A Component that can be used to display your .riv file. This component accepts the same attributes and event handlers as a `div` element.
* `rive`: A Rive.js `Rive` object. This will return as null until the .riv file has fully loaded.
* `canvas`: HTMLCanvasElement object, on which the .riv file is rendering.
* `setCanvasRef`: A callback ref that can be passed to your own canvas element, if you wish to have control over the rendering of the Canvas element.
*   `setContainerRef`: A callback ref that can be passed to a container element that wraps the canvas element, if you wish to have control over the rendering of the container element.

    _For the vast majority of use cases, you can just the returned `RiveComponent` and don't need to worry about `setCanvasRef` and `setContainerRef`._

### riveParams

* `src?`: _(optional)_ File path or URL to the .riv file to use. One of `src` or `buffer` must be provided.
* `buffer?`: _(optional)_ ArrayBuffer containing the raw bytes from a .riv file. One of `src` or `buffer` must be provided.
* `artboard?`: _(optional)_ Name of the artboard to use.
* `animations?`: _(optional)_ Name or list of names of animations to play.
* `stateMachines?`: _(optional)_ Name or list of names of state machines to load.
* `layout?`: _(optional)_ Layout object to define how animations are displayed on the canvas. See our [web runtime docs](https://github.com/rive-app/rive-wasm#layout) for more details.
* `autoplay?`: _(optional)_ If `true`, the animation will automatically start playing when loaded. Defaults to false.
* `onLoad?`: _(optional)_ Callback that gets fired when the .rive file loads.
* `onLoadError?`: _(optional)_ Callback that gets fired when an error occurs loading the .riv file.
* `onPlay?`: _(optional)_ Callback that gets fired when the animation starts playing.
* `onPause?`: _(optional)_ Callback that gets fired when the animation pauses.
* `onStop?`: _(optional)_ Callback that gets fired when the animation stops playing.
* `onLoop?`: _(optional)_ Callback that gets fired when the animation completes a loop.
* `onStateChange?`: _(optional)_ Callback that gets fired when a state change occurs.

### opts

* `useDevicePixelRatio`: _(optional)_ If `true`, the hook will scale the resolution of the animation based on the [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio). Defaults to `true`. NOTE: Requires the `setContainerRef` ref callback to be passed to an element wrapping a canvas element. If you use the `RiveComponent`, then this will happen automatically.
* `fitCanvasToArtboardHeight`: _(optional)_ If `true`, then the canvas will resize based on the height of the artboard. Defaults to `false`.
{% endtab %}

{% tab title="Vue" %}
The following snippet demonstrates how to create a simple Rive component in [Vue](https://vuejs.org) 2.

```markup
<template>
  <div>
    <canvas ref="canvas" width="400" height="400"></canvas>
  </div>
</template>

<script>
import { Rive, Layout } from '@rive-app/canvas';

export default {
  name: 'Rive',
  props: {
    src: String,
    fit: String,
    alignment: String
  },
  mounted() {
    new Rive({
        canvas: this.$refs.canvas,
        src: this.$props.src,
        layout: new Layout({
            fit: this.$props.fit,
            alignment: this.$props.alignment,
        }),
        autoplay: true
    })
  }
}
</script>
```
{% endtab %}

{% tab title="Flutter" %}
## 1. Add the Rive package dependency

{% code title="pubspec.yaml" %}
```yaml
dependencies:
  rive:
```
{% endcode %}

## 2. Import the Rive package

```dart
import 'package:rive/rive.dart';
```

## 3. Add a RiveAnimation widget

```dart
RiveAnimation.network(
  'https://cdn.rive.app/animations/vehicles.riv',
)
```

## Complete example

{% code title="main.dart" %}
```dart
import 'package:flutter/material.dart';
import 'package:rive/rive.dart';

void main() => runApp(MaterialApp(
      home: MyRiveAnimation(),
    ));

class MyRiveAnimation extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: Center(
        child: RiveAnimation.network(
          'https://cdn.rive.app/animations/vehicles.riv',
          fit: BoxFit.cover,
        ),
      ),
    );
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="iOS" %}
## 1a. Add Rive via CocoaPods

Add the following to your Podspec file:

{% code title="Podfile" %}
```ruby
  pod 'RiveRuntime'
```
{% endcode %}

## 1b. Add Rive via Swift Package Manager

To install via Swift Package Manager, in the package finder in xcode, search with the Github repository name: `https://github.com/rive-app/rive-ios`

## 2. Importing Rive

Add the following to the top of your file where you utilize the Rive runtime:

```swift
import RiveRuntime
```



## 3. v2 Runtime Usage

Rive iOS runtimes of versions 2.x.x or later should use the newer patterns for integrating Rive into your iOS applications. This involves some API changes from the pattern in versions 1.x.x. \
The primary object you'll use is a `RiveViewModel`. It is responsible for creating and interacting with Rive assets.&#x20;



### 3a. SwiftUI

#### Set up RiveViewModel w/ View

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(fileName: "cool_rive_animation").view()
    }
}
```

###

### 3b. UIKit - Storyboard

#### Set up RiveViewModel w/ Controller formatted on a Storyboard

The simplest way of adding Rive to a controller using Storyboards is to make a `RiveViewModel`, and set its view to be the `RiveView` you made in the Storyboard.

```swift
class AnimationViewController: UIViewController {
    @IBOutlet weak var riveView: RiveView!
    var simpleVM = RiveViewModel(fileName: "cool_rive_animation")

    override public func viewDidLoad() {
        simpleVM.setView(riveView)
    }
}
```

###

### 3c. UIKit - Programmatic&#x20;

#### Set up RiveViewModel w/ Controller from scratch in code

You can also add Rive to a controller purely with code by making the `RiveViewModel`, telling it to create a fresh `RiveView` and then adding it to the view hierarchy.

```swift
class AnimationViewController: UIViewController {
    var simpleVM = RiveViewModel(fileName: "cool_rive_animation")
    
    override func viewWillAppear(_ animated: Bool) {
        let riveView = simpleVM.createRiveView()
        view.addSubview(riveView)
        riveView.frame = view.frame
    }
}
```



## 4. Playback Controls

By default `RiveViewModel` will automatically play the animation or state machine you've given it. Very often that will be all that is needed to display your Rive asset. However, we have some convenient controls for when you want more fine grained control of when it plays and doesn't.&#x20;



### 4a. Play

If you set auto play to false you can play the active animation or state machine very simply.

```swift
simpleVM.play()
```



If there are multiple animations on the active artboard you can play a specific one.

```swift
simpleVM.play(animationName: "Fancy Animation")
```



### 4b. Pause, Stop, Reset

Based on certain events in your app you may want to adjust the playback further.&#x20;

```swift
simpleVM.pause()
simpleVM.stop()
simpleVM.reset()
```



### 4c. Inputs

Some Rive assets that use state machines are configured to accept inputs which trigger unique behavior. Triggering these events is easily done through the `RiveViewModel`.

```swift
// Something that displays 0 - 5 stars
starsVM.setInput("Rating Changed", value: 5)

// Something that toggles back and forth
toggleVM.setInput("Switch Flipped", value: true)

// Something that animates a burst of confetti when triggered
confettiVM.triggerInput("Celebrate")
```
{% endtab %}

{% tab title="Android" %}
## 1. Add the Rive dependency

Add the following dependencies to your `build.gradle` file in your project:

```groovy
dependencies {
    implementation 'app.rive:rive-android:2.0.19'
    // During initialization, you may need to add a dependency
    // for Jetpack Startup
    implementation "androidx.startup:startup-runtime:1.1.0"
}
```

## 2. Initialize Rive

Rive needs to initialize its runtime when your app starts.

It can be done via an [initializer](https://developer.android.com/topic/libraries/app-startup) that does this for you automatically. The initialization provider can be set up directly in your app's manifest file:

```xml
<provider
  android:name="androidx.startup.InitializationProvider"
  android:authorities="${applicationId}.androidx-startup"
  android:exported="false"
  tools:node="merge">
    <meta-data android:name="app.rive.runtime.kotlin.RiveInitializer"
      android:value="androidx.startup" />
</provider>
```



Otherwise, this can be achieved by calling the initializer in code:

```kotlin
AppInitializer.getInstance(applicationContext)
  .initializeComponent(RiveInitializer::class.java)
```



If you want to initialize Rive yourself, this can be done in code:

```kotlin
Rive.init(context)
```

## 3. Add RiveAnimation to your layout

The simplest way to get a Rive animation into your application is to include it as part of a layout. The following will consist of the Rive file loaded from the raw resources and auto-play its first animation. This assumes you have taken a downloaded `.riv` file (i.e `off_road_car_blog.riv`) and placed it in your raw resources folder.



{% code title="simple.xml" %}
```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:riveResource="@raw/off_road_car_blog" />

```
{% endcode %}



Another way to load a Rive file in is by referencing the URL where the asset lives (see Internet Permissions section below for an extra step in setup):

{% code title="simple.xml" %}
```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:riveUrl="https://cdn.rive.app/animations/vehicles.riv" />
```
{% endcode %}

\
When setting your context views, this might look like

```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class SimpleActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.simple)
    }
}
```

## Internet permissions

If you're retrieving Rive files over a network, your app will need permission to access the internet:

{% code title="AndroidManifest.xml" %}
```markup
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
{% endcode %}



Note that this isn't necessary if you include the files in your Android project and load these in as a raw resource.

## Resources

Github: [https://github.com/rive-app/rive-android](https://github.com/rive-app/rive-android)\
Examples: [https://github.com/rive-app/rive-android/tree/master/app/src/main/java/app/rive/runtime/example](https://github.com/rive-app/rive-android/tree/master/app/src/main/java/app/rive/runtime/example)
{% endtab %}

{% tab title="React Native" %}
## 1. Add the Rive dependency

```bash
npm install rive-react-native
# or for Yarn
yarn add rive-react-native
```

## 2. Pod install

`cd` inside the `ios` folder and run `pod install` (if deploying to iOS)

## 3. Add your rive component

{% code title="App.js" %}
```javascript
import Rive from 'rive-react-native';

function App() {
  return <Rive
      url="https://cdn.rive.app/animations/vehicles.riv"
      style={{width: 400, height: 400}}
  />;
}
```
{% endcode %}
{% endtab %}

{% tab title="Angular" %}
## 1. Install :

```
npm install ng-rive
```

## 2. Import `RiveModule`:

{% code title="animation.module.ts" %}
```typescript
import { RiveModule } from 'ng-rive';

@NgModule({
  declarations: [AnimationComponent],
  imports: [
    CommonModule,
    RiveModule,
  ],
})
export class AnimationModule { }
```
{% endcode %}

## 3. Add your .riv file in your assets

```
|-- assets
|   |--rive
|      |-- vehicles.riv
```

## 4. Use in template :

{% code title="animation.component.html" %}
```markup
<canvas riv="vehicles" width="500" height="500">
  <riv-animation name="idle" play></riv-animation>
</canvas>
```
{% endcode %}
{% endtab %}
{% endtabs %}

