# Example



{% tabs %}
{% tab title="Web" %}
```jsx
<html>

<head>
  <title>Rive Canvas Example</title>
  <link rel="stylesheet" href="/stylesheets/style.css">
</head>

<body>
  <h1>Rive Context 2D Example</h1>
  <p>For more info, check out the <a href="https://www.npmjs.com/package/rive-canvas"> Rive web canvas package</a>.</p>
  <canvas id="riveCanvas" width=512 height=512></canvas>
</body>

<script src="https://unpkg.com/rive-canvas@0.0.10/rive.js"></script>

<!-- Simple script to run a looping animation -->
<script>
  ; (function () {
    // first, instantiate the Rive engine and load the WASM file(s)
    Rive({
      locateFile: (file) => 'https://unpkg.com/rive-canvas@0.0.10/' + file,
    }).then((rive) => {
      // Rive's ready to rock 'n roll
      // Let's load up a Rive animation file, typically ending in '.riv'
      const req = new Request('./rive/loader.riv');
      fetch(req).then((res) => {
        return res.arrayBuffer();
      }).then((buf) => {
        // we've got the raw bytes of the animation, let's load them into a Rive
        // file
        const file = rive.load(new Uint8Array(buf));
        // get the default artboard, where the animations we want to interact
        // with live in this file
        const artboard = file.defaultArtboard();
        // now we can access the animations; let's get one called 'vibration'
        const vibrationAnim = artboard.animation('vibration');
        const vibrationInstance = new rive.LinearAnimationInstance(vibrationAnim);
        // let's grab our canvas
        const canvas = document.getElementById('riveCanvas');
        const ctx = canvas.getContext('2d');
        // nw we can create a Rive renderer and wire it up to our 2D context
        const renderer = new rive.CanvasRenderer(ctx);
        // advance the artboard to render a frame
        artboard.advance(0);
        // Let's make sure our frame fits into our canvas
        ctx.save();
        renderer.align(rive.Fit.contain, rive.Alignment.center, {
          minX: 0,
          minY: 0,
          maxX: canvas.width,
          maxY: canvas.height
        }, artboard.bounds);
        // and now we can draw our frame to our canvas
        artboard.draw(renderer);
        ctx.restore();

        // track the last time a frame was rendered
        let lastTime = 0;

        // okay, so we have an animation and a renderer; how do we play an
        // animation? First, let's set up our animation loop with
        // requestFrameAnimation
        function draw(time) {
          // work out how many seconds have passed since a previous frame was
          // drawn
          if (!lastTime) {
            lastTime = time;
          }
          const elapsedTime = (time - lastTime) / 1000;
          lastTime = time;

          // advance our animation by the elapsed time
          vibrationInstance.advance(elapsedTime);
          // apply the animation to the artboard 
          vibrationInstance.apply(artboard, 1.0);
          // advance the artboard
          artboard.advance(elapsedTime);

          // render the animation frame
          // first, clear the canvas
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          // let's resize it to fit the canvas
          ctx.save();
          renderer.align(rive.Fit.contain, rive.Alignment.center, {
            minX: 0,
            minY: 0,
            maxX: canvas.width,
            maxY: canvas.height
          }, artboard.bounds);
          // and now we can draw our frame to our canvas
          artboard.draw(renderer);
          ctx.restore();

          // and kick off the next frame
          requestAnimationFrame(draw);
        }
        // now kick off the animation
        requestAnimationFrame(draw);
      });
    });
  })();
</script>

</html>
```
{% endtab %}

{% tab title="Flutter" %}
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:rive/rive.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Rive Flutter Demo',
      home: Scaffold(
        body: MyRiveAnimation(),
      ),
    );
  }
}

class MyRiveAnimation extends StatefulWidget {
  @override
  _MyRiveAnimationState createState() => _MyRiveAnimationState();
}

class _MyRiveAnimationState extends State<MyRiveAnimation> {
  final riveFileName = 'assets/truck.riv';
  Artboard _artboard;

  @override
  void initState() {
    _loadRiveFile();
    super.initState();
  }

  // loads a Rive file
  void _loadRiveFile() async {
    final bytes = await rootBundle.load(riveFileName);
    final file = RiveFile();

    if (file.import(bytes)) {
      // Select an animation by its name
      setState(() => _artboard = file.mainArtboard
        ..addController(
          SimpleAnimation('idle'),
        ));
    }
  }

  /// Show the rive file, when loaded
  @override
  Widget build(BuildContext context) {
    return _artboard != null
        ? Rive(
            artboard: _artboard,
            fit: BoxFit.cover,
          )
        : Container();
  }
}
```
{% endtab %}
{% endtabs %}

