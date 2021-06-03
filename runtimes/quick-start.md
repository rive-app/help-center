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
        src: 'https://cdn.rive.app/animations/truck.riv',
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
                src: 'https://cdn.rive.app/animations/truck.riv',
                canvas: document.getElementById('canvas'),
                autoplay: true
            });
        </script>
    </body>
</html>
```
{% endtab %}

{% tab title="React & Vue" %}
The following shows how to create a simple Rive component in [React](https://reactjs.org/); the steps are broadly applicable to other web frameworks. See the example at the bottom of the page for [Vue](https://vuejs.org/).

## 1. Add Rive package to package.json

{% code title="package.json" %}
```javascript
{
  "dependencies": {
    "rive-js": "latest"
  }
}
```
{% endcode %}

## 2. Import the Rive library

```javascript
import { Rive } from 'rive-js';
```

## 3. Create a canvas

Create a canvas where the Rive file will display:

```javascript
return (
    <canvas ref={canvas} />
);
```

## 4. Create a Rive object

```javascript
useEffect(() => {
    rive.current = new Rive({
        src: asset,
        canvas: canvas.current,
        animation: animation,
        layout: new Layout({fit: fit, alignment: alignment}),
        autoplay: true,
    });

    return () => rive.current.stop();
});
```

## Complete example

{% code title="Animation.js" %}
```javascript
import React, { useRef, useEffect } from 'react';
import { Rive, Layout } from 'rive-js';

const Animation = ({ asset, animation, fit, alignment }) => {
    const canvas = useRef(null);
    const animationContainer = useRef(null);
    let rive = useRef(null);

    // Resizes the canvas to match the parent element
    useEffect(() => {
        
        let resizer = () => {
            const { width: w, height: h } =
                animationContainer.current.getBoundingClientRect();
            canvas.current.width = w;
            canvas.current.height = h;
        };
        resizer();
    });

    // Starts the animation
    useEffect(() => {
        rive.current = new Rive({
            src: asset,
            canvas: canvas.current,
            animation: animation,
            layout: new Layout({fit: fit, alignment: alignment}),
            autoplay: true,
        });

        return () => rive.current.stop();
    });

    return (
        <div ref={animationContainer} className="Rive-logo">
            <canvas ref={canvas} />
        </div>
    );
};

export default Animation;
```
{% endcode %}

{% code title="App.js" %}
```javascript
import Animation from './Animation.js';
import './App.css';

function App() {
  return (
    <div className="App">
        <Animation 
          asset="https://cdn.rive.app/animations/truck.riv"
          fit="cover"
          alignment="center" />
    </div>
  );
}

export default App;
```
{% endcode %}

## Vue.js 2 Example

{% code title="Rive.vue" %}
```javascript
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
{% endcode %}
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
  'https://cdn.rive.app/animations/truck.riv',
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
          'https://cdn.rive.app/animations/truck.riv',
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
    let url = "https://cdn.rive.app/animations/truck.riv"
    
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

        app:riveUrl="https://cdn.rive.app/animations/truck.riv"
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
{% endtab %}

{% tab title="React Native - Android" %}
## 1. Add the Rive dependency

```sh
npm install rive-react-native
```

## 2. Update Android minSdkVersion

{% code title="android/build.gradle" %}

```kotlin
buildscript {
   ext {
     ...
     minSdkVersion = 23
     ...
   }
}
```
{% endcode %}

## 3. Add your rive component

{% code title="App.js" %}
```js
import Rive from 'rive-react-native';

function App() {
  return <Rive
      url="https://cdn.rive.app/animations/truck.riv"
      style={{width: 400, height: 400}}
  />;
}
```
{% endcode %}
{% endtab %}

{% tab title="React Native - iOS" %}

## 1. Add the Rive dependency

```sh
npm install rive-react-native
```

## 2. Update iOS build target to at least 11.4

{% code title="ios/Podfile" %}
```ruby
platform :ios, '11.4'
```
{% endcode %}

## 3. Create Bridging header

- open the iOS prject file in xcode `open ios/<name>.xcodeproj`
- create a new empty swift file. 
- confirm "Create Bridging Header"

more resources on this: 
- https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift
- https://github.com/facebookarchive/react-native-fbsdk/issues/755

## 4. Add your rive component 

{% code title="App.js" %}
```js
import Rive from 'rive-react-native';

function App() {
  return <Rive
      url="https://cdn.rive.app/animations/truck.riv"
      style={{width: 400, height: 400}}
  />;
}
```
{% endcode %}
{% endtab %}

{% endtabs %}





