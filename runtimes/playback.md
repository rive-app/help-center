---
description: Playing and pausing animations
---

# Playback

Rive lets you specify what artboard to use, what animations and state machines to mix and play, and to control the play/pause state of each animation.

From here on we're going to use the term _animations_ collectively to refer to both animations and state machines. Where there are differences between the two, we'll explicitly call this out.

## Choosing an artboard

When a Rive object is instantiated, the artboard to use can be specified. If no artboard is given, the default artboard is used. Only one artboard can be used at a time.

{% tabs %}
{% tab title="Web" %}
```javascript
new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    artboard: 'Truck',
    autoplay: true
});
```
{% endtab %}

{% tab title="React" %}
```javascript
export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" artboard="Truck" />
);

// With `useRive` Hook:
export default function Simple() {
  const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    artboard: 'Truck',
    autoplay: true,
  });

  return <RiveComponent />;
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    artboard: 'Truck'
);
```
{% endtab %}

{% tab title="Angular" %}
```markup
<canvas riv="vehicles" width="500" height="500" artboard="Truck">

</canvas>
```

_If no artboard name is provided, the default artboard is used._
{% endtab %}
{% endtabs %}

## Choosing starting animations

Starting animations can also be chosen when Rive is instantiated. A list of animation names can be provided.

{% tabs %}
{% tab title="Web" %}
```javascript
// Play the idle animation
new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    animations: 'idle',
    autoplay: true
});

// play and mix the idle and curves animations
new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    animations: ['idle', 'curves'],
    autoplay: true
});
```
{% endtab %}

{% tab title="React" %}
```javascript
// Play the idle animation
export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" animations="idle" />
);

// play and mix the idle and curves animations
export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" animations={['idle', 'curves']} />
);

// With `useRive` Hook:
export default function Simple() {
  const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    animations:{['idle', 'curves']},
    autoplay: true,
  });

  return <RiveComponent />;
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
// Play the curves animation
RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    animations: ['curves'],
);

// Play and mix both the idle and curves animations
RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    animations: ['idle', 'curves'],
),
```
{% endtab %}

{% tab title="Angular" %}
```markup
<!-- Play the curves animation -->
<canvas riv="vehicles" width="500" height="500">
  <riv-animation name="curves" play></riv-animation>
</canvas>

<!-- Play and mix both the idle and curves animations -->
<canvas riv="vehicles" width="500" height="500">
  <riv-animation name="idle" play></riv-animation>
  <riv-animation name="curves" play></riv-animation>
</canvas>
```
{% endtab %}
{% endtabs %}

## Choosing starting state machines

A starting state machine can be specified when Rive is instantiated. A state machine name can be provided.

{% tabs %}
{% tab title="Web" %}
```javascript
new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    stateMachines: 'weather',
    autoplay: true
});
```
{% endtab %}

{% tab title="React" %}
```javascript
// State Machine require the useRive hook.
export default function Simple() {
  const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    stateMachines: "weather",
    autoplay: true,
  });

  return <RiveComponent />;
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    stateMachines: ['weather'],
)
```
{% endtab %}

{% tab title="Angular" %}
```markup
<canvas riv="vehicles" width="500" height="500">
  <riv-state-machine name="weather" play></riv-state-machine>
</canvas>
```
{% endtab %}
{% endtabs %}

## Controlling playback

Playback of each animation and state machine can be separately controlled. You can play and pause playback using the `play` , `pause` and `stop` methods, either passing in the names of the animations you want to effect, or passing in nothing which will effect all instanced animations.

You can also provide callback to receive notification when certain events have occurred:

* `onLoad` when a rive file has been loaded and initialized; it's now ready for playback
* `onPlay` when one or more animations play; provides a list of animations
* `onPause` when one or more animations pause; provides a list of animations
* `onLoop` when an animation loops; provides the animation name

{% tabs %}
{% tab title="Web" %}
```markup
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="user-scalable=no">
        <title>Manually Control Rive Animations</title>
        <link rel="stylesheet" href="styles.css">
    </head>
    <body class="parent">
        <div>
            <canvas id="canvas" width="500" height="500"></canvas>
        </div>
        <div>
            <button id="idle">Start Truck</button>
            <button id="wipers">Start Wipers</button>
        </div>

        <script src="https://unpkg.com/@rive-app/canvas@1.0.47"></script>
        <script>
            // animation will show the first frame but not start playing
            const truck = new rive.Rive({
                src: 'https://cdn.rive.app/animations/vehicles.riv',
                artboard: 'Jeep',
                canvas: document.getElementById('canvas'),
                layout: new rive.Layout({fit: 'fill'}),
            });

            const idleButton = document.getElementById('idle');
            const wipersButton = document.getElementById('wipers');

            idleButton.onclick = _ => 
            truck.playingAnimationNames.includes('idle') ?
                    truck.pause('idle') :
                    truck.play('idle');

            wipersButton.onclick = _ =>
                truck.playingAnimationNames.includes('windshield_wipers') ?
                    truck.pause('windshield_wipers') :
                    truck.play('windshield_wipers');

            // Listen for play events to update button text
            truck.on(rive.EventType.Play, (event) => {
                const names = event.data;
                names.forEach((name) => {
                    if (name === 'idle') {
                        idleButton.innerHTML = 'Stop Truck';
                    } else if (name === 'windshield_wipers') {
                        wipersButton.innerHTML = 'Stop Wipers';
                    } 
                });
            });

            // Listen for pause events to update button text
            truck.on(rive.EventType.Pause, (event) => {
                const names = event.data;
                names.forEach((name) => {
                    if (name === 'idle') {
                        idleButton.innerHTML = 'Start Truck';
                    } else if (name === 'windshield_wipers') {
                        wipersButton.innerHTML = 'Start Wipers';
                    }
                });
            });
        </script>
    </body>
</html>
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useState, useEffect } from "react";
import { useRive, Layout, Fit } from "@rive-app/react-canvas";

export default function App() {
  const [truckButtonText, setTruckButtonText] = useState("Start Truck");
  const [wiperButtonText, setWiperButtonText] = useState("Start Wipers");

  // animation will show the first frame but not start playing
  const { rive, RiveComponent } = useRive({
    src: "https://cdn.rive.app/animations/vehicles.riv",
    artboard: "Jeep",
    layout: new Layout({ fit: Fit.Cover }),
  });

  useEffect(() => {
    if (rive) {
      // Listen for play events to update button text
      rive.on("play", (event) => {
        const names = event.data;
        names.forEach((name) => {
          if (name === "idle") {
            setTruckButtonText("Stop Truck");
          } else if (name === "windshield_wipers") {
            setWiperButtonText("Stop Wipers");
          }
        });
      });

      // Listen for pause events to update button text
      rive.on("pause", (event) => {
        const names = event.data;
        names.forEach((name) => {
          if (name === "idle") {
            setTruckButtonText("Start Truck");
          } else if (name === "windshield_wipers") {
            setWiperButtonText("Start Wipers");
          }
        });
      });
    }
  }, [rive]);

  function onStartTruckClick() {
    if (rive) {
      if (rive.playingAnimationNames.includes("idle")) {
        rive.pause("idle");
      } else {
        rive.play("idle");
      }
    }
  }

  function onStartWiperClick() {
    if (rive) {
      if (rive.playingAnimationNames.includes("windshield_wipers")) {
        rive.pause("windshield_wipers");
      } else {
        rive.play("windshield_wipers");
      }
    }
  }

  return (
    <>
      <div>
        <RiveComponent style={{ height: "1000px" }} />
      </div>
      <div>
        <button id="idle" onClick={onStartTruckClick}>
          {truckButtonText}
        </button>
        <button id="wipers" onClick={onStartWiperClick}>
          {wiperButtonText}
        </button>
      </div>
    </>
  );
}
```
{% endtab %}

{% tab title="Flutter" %}
Flutter handles things a little differently to the other runtimes due to its reactive nature.

Every animation and state machine in Flutter has an underlying controller that manages the state of each animation. When you pass a list of animation names to the `RiveAnimation` widget, it creates and manages controllers for each.

On order to access control to animations, you'll need to instantiate a `RiveAnimationController` for each animation, and pass the controller to the widget instead of its name. You can mix and match passing in controllers and names, but don't pass in both for the same animation.

There are a number of controllers provided in the runtime that perform certain tasks. We'll cover these as they become relevant.

### Manually controlling a looping animation

`SimpleAnimation` is a basic controller that provides simple playback control of an animation. With this you can play, pause, and reset animations.

In the following example, the `isActive` property of `SimpleAnimation` is used to play and pause a single animation.

```dart
import 'package:flutter/material.dart';
import 'package:rive/rive.dart';

class PlayPauseAnimation extends StatefulWidget {
  const PlayPauseAnimation({Key? key}) : super(key: key);

  @override
  _PlayPauseAnimationState createState() => _PlayPauseAnimationState();
}

class _PlayPauseAnimationState extends State<PlayPauseAnimation> {
  /// Controller for playback
  late RiveAnimationController _controller;

  /// Toggles between play and pause animation states
  void _togglePlay() =>
      setState(() => _controller.isActive = !_controller.isActive);

  /// Tracks if the animation is playing by whether controller is running
  bool get isPlaying => _controller.isActive;

  @override
  void initState() {
    super.initState();
    _controller = SimpleAnimation('idle');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Animation Example'),
      ),
      body: Center(
        child: RiveAnimation.network(
          'https://cdn.rive.app/animations/vehicles.riv',
          controllers: [_controller],
          // Update the play state when the widget's initialized
          onInit: () => setState(() {}),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _togglePlay,
        tooltip: isPlaying ? 'Pause' : 'Play',
        child: Icon(
          isPlaying ? Icons.pause : Icons.play_arrow,
        ),
      ),
    );
  }
}
```

### Repeatedly playing a one-shot animation

One-shot animations do not loop. `OneShotAnimation` is a controller that will automatically stop and reset a one-shot animation when it has played through so it can be repeatedly played as required.

The controller also provides two callbacks: `onStart` and `onStop` that will fire when the animation starts and stops playing respectively.

The example below demonstrates mixing animation names with controllers. The `idle` and `curves` animations are managed by the runtime and as their looping animations will play continuously. `bounce` is a one-shot and is triggered when the button is tapped. The runtime will then play `bounce`, mixing it cleanly with the looping animations.

```dart
import 'package:flutter/material.dart';
import 'package:rive/rive.dart';

class PlayOneShotAnimation extends StatefulWidget {
  const PlayOneShotAnimation({Key? key}) : super(key: key);

  @override
  _PlayOneShotAnimationState createState() => _PlayOneShotAnimationState();
}

class _PlayOneShotAnimationState extends State<PlayOneShotAnimation> {
  /// Controller for playback
  late RiveAnimationController _controller;

  /// Is the animation currently playing?
  bool _isPlaying = false;

  @override
  void initState() {
    super.initState();
    _controller = OneShotAnimation(
      'bounce',
      autoplay: false,
      onStop: () => setState(() => _isPlaying = false),
      onStart: () => setState(() => _isPlaying = true),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('One-Shot Example'),
      ),
      body: Center(
        child: RiveAnimation.network(
          'https://cdn.rive.app/animations/vehicles.riv',
          animations: const ['idle', 'curves'],
          controllers: [_controller],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // disable the button while playing the animation
        onPressed: () => _isPlaying ? null : _controller.isActive = true,
        tooltip: 'Play',
        child: const Icon(Icons.arrow_upward),
      ),
    );
  }
}
```
{% endtab %}

{% tab title="Angular" %}
### Simple manipulation

You can apply simple manipulation on the `riv-animation` directive:

```markup
<canvas riv="vehicles" width="500" height="500">
  <riv-animation name="curves" [play]="playing" speed="0.5"></riv-animation>
</canvas>

<button (click)="playing = !playing">Toggle Player</button>
```

_If `speed` is negative, the animation goes in reverse._

### Advance manipulation

For more advances manipulations you can use the `riv-player` directive:

```markup
<canvas riv="vehicles" width="500" height="500">
  <riv-player #player="rivPlayer" name="curves" [time]="time" mode="one-shot"></riv-player>
</canvas>

<input type="range" step="0.1"
  (input)="time = $event.target.value"
  [min]="player.startTime"
  [max]="player.endTime"
/>
```

* The `time` input will let you specify a moment in ms in the animation.
* The `mode` input will force the mode "one-shot", "loop" or "ping-pong" (if undefined, default mode is used).

### Manipulate nodes

You can select a specific node in the animation with the `riv-node`, `riv-bone` & `riv-root-bone` directives :

```markup
<canvas riv="vehicles" (mouseover)="position = $event.x">
  <riv-node name="wheel" [x]="position" scaleX="0.5"></riv-node>
</canvas>
```

_If the property of the node is updated by the animation, the animation wins._
{% endtab %}
{% endtabs %}
