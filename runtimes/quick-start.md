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
<script src="https://unpkg.com/rive-js"></script>
```

## 2. Create a canvas

Create a canvas where you want the Rive file to display:

```javascript
<canvas id="canvas" width="500" height="500"></canvas>
```

## 3. Create a Rive object

Create a new instance of a Rive object, providing the url of the Rive file you wish to show, the canvas on which you want it to render, and whether you want the default animation to play.

```javascript
<script>
    new rive.Rive({
        src: 'https://cdn.rive.app/animations/vehicles.riv',
        canvas: document.getElementById('canvas'),
        autoplay: true
    });
</script>
```

## Complete example

```javascript
<html>
    <head>
        <title>Rive Hello World</title>
    </head>
    <body>
        <canvas id="canvas" width="500" height="500"></canvas>

        <script src="https://unpkg.com/rive-js"></script>
        <script>
            new rive.Rive({
                src: 'https://cdn.rive.app/animations/vehicles.riv',
                canvas: document.getElementById('canvas'),
                autoplay: true
            });
        </script>
    </body>
</html>
```
{% endtab %}

{% tab title="React" %}
## Install the rive-react package

{% code %}
```bash
npm i --save rive-react
```
{% endcode %}

## Component

Rive React provides a basic component as it's default import for displaying simple animations.

```javascript
import Rive from 'rive-react'

export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" />
);
```

#### Props

- `src`: File path or URL to the .riv file to display.
- `artboard`: _(optional)_ Name to display.
- `animations`: _(optional)_ Name or list of names of animtions to play.
- `layout`: _(optional)_ Layout object to define how animations are displayed on the canvas.
- _All attributes and eventHandlers that can be passed to a `div` element can also be passed to the `Rive` component and used in the same manner._

## useRive Hook

For more advanced usage, the `useRive` hook is provided. The hook will return a component and a `Rive` object which gives you control of the current rive file.

```javascript
import { useRive } from 'rive-react';

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

#### Parameters

- `riveParams`: Set of parameters that are passed to the Rive.js `Rive` class constructor. `null` and `undefined` can be passed to conditionally display the .riv file.
- `opts`: Rive React specific options.

#### Return Values

- `RiveComponent`: A Component that can be used to display your .riv file. This component accepts the same attributes and event handlers as a `div` element.
- `rive`: A Rive.js `Rive` object. This will return as null until the .riv file has fully loaded.
- `canvas`: HTMLCanvasElement object, on which the .riv file is rendering.
- `setCanvasRef`: A callback ref that can be passed to your own canvas element, if you wish to have control over the rendering of the Canvas element.
- `setContainerRef`: A callback ref that can be passed to a container element that wraps the canvas element, if you which to have control over the rendering of the container element.
  _For the vast majority of use cases, you can just the returned `RiveComponent` and don't need to worry about `setCanvasRef` and `setContainerRef`._

#### riveParams

- `src?`: _(optional)_ File path or URL to the .riv file to use. One of `src` or `buffer` must be provided.
- `buffer?`: _(optional)_ ArrayBuffer containing the raw bytes from a .riv file. One of `src` or `buffer` must be provided.
- `artboard?`: _(optional)_ Name of the artboard to use.
- `animations?`: _(optional)_ Name or list of names of animations to play.
- `stateMachines?`: _(optional)_ Name of list of names of state machines to load.
- `layout?`: _(optional)_ Layout object to define how animations are displayed on the canvas. See [Rive.js](https://github.com/rive-app/rive-wasm#layout) for more details.
- `autoplay?`: _(optional)_ If `true`, the animation will automatically start playing when loaded. Defaults to false.
- `onLoad?`: _(optional)_ Callback that get's fired when the .rive file loads .
- `onLoadError?`: _(optional)_ Callback that get's fired when an error occurs loading the .riv file.
- `onPlay?`: _(optional)_ Callback that get's fired when the animation starts playing.
- `onPause?`: _(optional)_ Callback that get's fired when the animation pauses.
- `onStop?`: _(optional)_ Callback that get's fired when the animation stops playing.
- `onLoop?`: _(optional)_ Callback that get's fired when the animation completes a loop.
- `onStateChange?`: _(optional)_ Callback that get's fired when a state change occurs.

#### opts

- `useDevicePixelRatio`: _(optional)_ If `true`, the hook will scale the resolution of the animation based the [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio). Defaults to `true`. NOTE: Requires the `setContainerRef` ref callback to be passed to a element wrapping a canvas element. If you use the `RiveComponent`, then this will happen automatically.
- `fitCanvasToArtboardHeight`: _(optional)_ If `true`, then the canvas will resize based on the height of the artboard. Defaults to `false`.


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
import { Rive, Layout } from 'rive-js'

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
## 1. Add the CocoaPods dependency

{% code title="Podfile" %}
```ruby
  pod 'RiveRuntime', :git => 'git@github.com:rive-app/test-ios.git'
```
{% endcode %}

## 2. Create a UIViewController and import runtime

```swift
import UIKit
import RiveRuntime

class RiveViewController: UIViewController {

    override public func loadView() {
        super.loadView()
    }
}
```

## 3. Add a RiveView and a RiveFile

```swift
import UIKit
import RiveRuntime

class RiveViewController: UIViewController {
    let url = "https://cdn.rive.app/animations/vehicles.riv"

    override public func loadView() {
        super.loadView()

        let view = RiveView()
        guard let riveFile = RiveFile(httpUrl: url, with: view) else {
            fatalError("Unable to load RiveFile")
        }

        view.configure(riveFile)
        self.view = view
    }
}
```

## 4. Tidy up once the view is dismissed

```swift
override public func viewDidDisappear(_ animated: Bool) {
    (view as! RiveView).stop()
    super.viewDidDisappear(animated)
}
```
{% endtab %}

{% tab title="Android" %}
## 1. Add the Rive dependency

```groovy
dependencies {
    implementation 'app.rive:rive-android:0.1.1'
}
```

## 2. Add RiveAnimation to your layout

```markup
    <app.rive.runtime.kotlin.RiveAnimationView
        android:id="@+id/my_rive_animation"

        android:layout_width="match_parent"
        android:layout_height="match_parent"

        app:riveUrl="https://cdn.rive.app/animations/vehicles.riv"
        app:riveFit="COVER" />
```

## 3. Clean up when done

{% code title="MyRiveActivity.kt" %}
```kotlin
class MyRiveActivity : AppCompatActivity() {

    private val animationView by lazy(LazyThreadSafetyMode.NONE) {
        findViewById<RiveAnimationView>(R.id.my_rive_animation)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.my_rive)
    }

    override fun onDestroy() {
        super.onDestroy()
        animationView.destroy()
    }
}
```
{% endcode %}

## Internet permissions

This example requires your app to have permission to access the internet:

{% code title="AndroidManifest.xml" %}
```markup
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
{% endcode %}

This is needed only if you're retrieving Rive files over a network. If you include the files in your Android project, this isn't necessary.
{% endtab %}

{% tab title="React Native" %}
## 1. Add the Rive dependency

```bash
npm install --save rive-react-native
```

## 2. Create iOS bridging header

* open the iOS project file in XCode `open ios/<name>.xcodeproj`
* create a new empty swift file. 
* confirm "Create Bridging Header"

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

```text
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

```text
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

