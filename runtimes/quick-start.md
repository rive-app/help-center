---
description: Add Rive to your app or web page in a couple of minutes
---

# Quick Start



{% tabs %}
{% tab title="web" %}
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
        <canvas id="canvas"></canvas>

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
## 1. Add Rive React package to package.json

{% code title="package.json" %}
```javascript
{
  "dependencies": {
    "rive-react": "latest"
  }
}
```
{% endcode %}

## 2. Import the Rive library

```javascript
import Rive from 'rive-react';
```

## 3. Use the Rive component

```javascript
export const Simple = () => <Rive 
    src="https://cdn.rive.app/animations/vehicles.riv"
/>;
```
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

## 3. Update iOS build target

{% code title="ios/Podfile" %}
```yaml
platform :ios, '10'
```
{% endcode %}

## 4. Create iOS bridging header

* open the iOS project file in XCode `open ios/<name>.xcodeproj`
* create a new empty swift file. 
* confirm "Create Bridging Header"

## 5. Add your rive component

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
{% endtabs %}

