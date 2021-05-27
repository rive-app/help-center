---
description: Get up and running with Rive in your app or web page in a couple of minutes
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

{% tab title="React" %}
The following shows how to create a simple Rive component in [React](https://reactjs.org/); the steps are broadly applicable to other web frameworks.

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
## Coming soon
{% endtab %}

{% tab title="iOS" %}
## Coming soon
{% endtab %}

{% tab title="Android" %}
## Coming soon
{% endtab %}
{% endtabs %}

## 



