---
description: Playing and pausing animations
---

# Animation Playback

Rive lets you specify what artboard to use, what animations and state machines to mix and play and control the play/pause state of each animation.

The term _animations_ may collectively refer to both animations and state machines. In this section, we explore how to deal with specific animation playback, rather than state machines.

{% hint style="info" %}
If you are trying to coordinate multiple animations' playback at runtime, consider using a state machine instead to do this for you!
{% endhint %}

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

{% tab title="Angular" %}
```markup
<canvas riv="vehicles" width="500" height="500" artboard="Truck">

</canvas>
```

_If no artboard name is provided, the default artboard is used._
{% endtab %}

{% tab title="React Native" %}
```jsx
export default function App() {
  return (
    <View>
      <Rive resourceName="truck_v7" artboardName="Jeep" autoplay />
    </View>
  );
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

{% tab title="iOS" %}
### SwiftUI

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(
            fileName: "dancing_banana", 
            artboardName: "Banana"
        ).view()
    }
}
```

### UIKit

```swift
class AnimationViewController: UIViewController {
    @IBOutlet weak var riveView: RiveView!

    var bananaVM = RiveViewModel(
        fileName: "dancing_banana",
        artboardName: "Banana",
    )
    
    override func viewDidLoad() {
        bananaVM.setView(riveView)
    }
}
```
{% endtab %}

{% tab title="Android" %}
#### Via XML

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    app:riveAutoPlay="true"
    app:riveArtboard="Square"
    app:riveResource="@raw/artboard_animations" />
```

#### Via Kotlin

```kotlin
animationView.setRiveResource(
    R.raw.artboard_animations,
    artboardName = "Square",
    autoplay = true
)
```
{% endtab %}
{% endtabs %}

## Choosing starting animations

Starting animations can also be chosen when Rive is instantiated. The first animation on the artboard may play if one is not provided, or a state machine is not set.

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
```
{% endtab %}

{% tab title="React" %}
```javascript
// Play the idle animation
export const Simple = () => (
  <Rive src="https://cdn.rive.app/animations/vehicles.riv" animations="idle" />
);

// With `useRive` Hook:
export default function Simple() {
  const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    animations:{['idle']},
    autoplay: true,
  });

  return <RiveComponent />;
}
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

{% tab title="React Native" %}
Currently, with the React Native runtime, you can set one animation to autoplay at the start. Despite this, see below in the playback sections to see how you can mix multiple animations on playback functions.



```jsx
export default function App() {
  return (
    <View>
      <Rive
        resourceName="truck_v7"
        artboardName="Jeep"
        autoplay
        animationName="idle"
      />
    </View>
  );
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

{% tab title="iOS" %}
By default `RiveViewModel` will automatically play the animation or state machine you've given it.

### SwiftUI

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(
            fileName: "dancing_banana", 
            animationName: "Charleston",
            artboardName: "Banana"
        ).view()
    }
}
```

### UIKit

```swift
class AnimationViewController: UIViewController {
    @IBOutlet weak var riveView: RiveView!

    var bananaVM = RiveViewModel(
        fileName: "dancing_banana",
        animationName: "Charleston",
        artboardName: "Banana"
    )
    
    override func viewDidLoad() {
        bananaVM.setView(riveView)
    }
}
```
{% endtab %}

{% tab title="Android" %}
With the Android runtime, specify **one** animation with the `riveAnimation` property

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    app:riveAutoPlay="true"
    app:riveArtboard="Square"
    app:riveAnimation="rollaround"
    app:riveResource="@raw/artboard_animations" />
```

Or

```kotlin
animationView.setRiveResource(
    R.raw.artboard_animations,
    artboardName = "Square",
    animationName = "rollaround",
    autoplay = true
)
```
{% endtab %}
{% endtabs %}

## Controlling playback

Playback of each animation and state machine can be separately controlled. You can play and pause playback using the `play` , `pause` and `stop` methods, either passing in the names of the animations you want to affect or passing in nothing which will affect all instanced animations.

{% tabs %}
{% tab title="Web" %}
#### Invoking Playback Controls

With the web runtime, you can provide callback functions to receive notification when certain animation events have occurred:

* `onLoad` when a rive file has been loaded and initialized; it's now ready for playback
* `onPlay` when one or more animations play; provides a list of animations
* `onPause` when one or more animations pause; provides a list of animations
* `onStop` when one or more animations are stopped; provides a list of animations
* `onLoop` when an animation loops; provides the animation name



See the following codepen link to try out the below code: [https://codepen.io/zplata/pen/yLPqLRa](https://codepen.io/zplata/pen/yLPqLRa)



```javascript
const idleButton = document.getElementById("idle");
const wipersButton = document.getElementById("wipers");
const loopDiv = document.getElementById("loop");

const truck = new rive.Rive({
  src: "https://cdn.rive.app/animations/vehicles.riv",
  artboard: "Jeep",
  canvas: document.getElementById("canvas"),
  layout: new rive.Layout({ fit: "fill" }),
  // Listen for play events to update button text
  onPlay: (event) => {
    const names = event.data;
    names.forEach((name) => {
      if (name === "idle") {
        idleButton.innerHTML = "Stop Truck";
      } else if (name === "windshield_wipers") {
        wipersButton.innerHTML = "Stop Wipers";
      }
    });
  },
  // Listen for pause events to update button text
  onPause: (event) => {
    const names = event.data;
    names.forEach((name) => {
      if (name === "idle") {
        idleButton.innerHTML = "Start Truck";
      } else if (name === "windshield_wipers") {
        wipersButton.innerHTML = "Start Wipers";
      }
    });
  },
  onLoop: (event) => {
    loopDiv.innerHTML = `Looped Animation: ${event.data.animation}`;
  }
});

idleButton.onclick = (_) =>
  truck.playingAnimationNames.includes("idle")
    ? truck.pause("idle")
    : truck.play("idle");

wipersButton.onclick = (_) =>
  truck.playingAnimationNames.includes("windshield_wipers")
    ? truck.pause("windshield_wipers")
    : truck.play("windshield_wipers");
```
{% endtab %}

{% tab title="React" %}
#### Invoking Playback Controls

Very similarly to Web, you can pass in Rive params and callbacks for certain animation events. See the Web tab for some examples of callbacks you can set. Additionally, you can use the `rive` object returned from the `useRive` hook to invoke playback controls.\
\
See the example below here: [https://codesandbox.io/s/animation-playback-controls-rive-react-7yo76i](https://codesandbox.io/s/animation-playback-controls-rive-react-7yo76i)\


```javascript
import { useState } from "react";
import { useRive, Layout, Fit } from "@rive-app/react-canvas";

export default function App() {
  const [truckButtonText, setTruckButtonText] = useState("Start Truck");
  const [wiperButtonText, setWiperButtonText] = useState("Start Wipers");

  // animation will show the first frame but not start playing
  const { rive, RiveComponent } = useRive({
    src: "https://cdn.rive.app/animations/vehicles.riv",
    artboard: "Jeep",
    layout: new Layout({ fit: Fit.Cover }),
    // Listen for play events to update button text
    onPlay: (event) => {
      const names = event.data;
      names.forEach((name) => {
        if (name === "idle") {
          setTruckButtonText("Stop Truck");
        } else if (name === "windshield_wipers") {
          setWiperButtonText("Stop Wipers");
        }
      });
    },
    // Listen for pause events to update button text
    onPause: (event) => {
      const names = event.data;
      names.forEach((name) => {
        if (name === "idle") {
          setTruckButtonText("Start Truck");
        } else if (name === "windshield_wipers") {
          setWiperButtonText("Start Wipers");
        }
      });
    }
  });

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

{% tab title="React Native" %}
#### Invoking Playback Controls

To trigger animation playback controls, set a `ref` on the Rive component rendered. Once the `ref` is populated, you can trigger functions such as `play`, `pause`, etc. See the `ref` method docs for React Native [here](overview/react-native/rive-ref-methods.md).\


```tsx
import Rive, { RiveRef } from 'rive-react-native'

export default function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handlePlayPress = () => {
    riveRef?.current?.play();
  };
  
  const handlePausePress = () => {
    riveRef?.current?.pause();
  };

  return (
    <View>
      <Rive
        resourceName="truck_v7"
        animationName="idle"
        ref={riveRef}
      />

      <Button onPress={handlePlayPress} title="play">
      <Button onPress={handlePausePress} title="pause">
    </View>
  );
}
```
{% endtab %}

{% tab title="Flutter" %}
Flutter handles things a little differently compared to the other runtimes due to its reactive nature.

Every animation and state machine in Flutter has an underlying controller that manages the state of each animation. When you pass a list of animation names to the `RiveAnimation` widget, it creates and manages controllers for each.

In order to access controls for animations, you'll need to instantiate a `RiveAnimationController` for each animation and pass the controller to the `RiveAnimation` widget instead of its name. You can mix and match passing in controllers and names, but avoid passing in both for the same animation.

There are a number of controllers provided in the runtime that perform certain tasks. We'll cover these as they become relevant.

### Manually controlling a looping animation

[SimpleAnimation](https://pub.dev/documentation/rive/latest/rive/SimpleAnimation-class.html) is a basic controller that provides simple playback control of a single animation. With this controller, you can play, pause, and reset animations.

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

One-shot animations do not loop. [OneShotAnimation](https://pub.dev/documentation/rive/latest/rive/OneShotAnimation-class.html) is a controller that will automatically stop and reset a one-shot animation when it has played through so it can be repeatedly played as required.

The controller also provides two callbacks: `onStart` and `onStop` that will fire when the animation starts and stops playing respectively.

The example below demonstrates mixing animation names with controllers. The `idle` and `curves` animations are managed by the runtime and their looping animations will play continuously. `bounce` is a one-shot and is triggered when the button is tapped. The runtime will then play the `bounce` animation, mixing it cleanly with the looping animations.

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

{% tab title="iOS" %}
#### Invoking Playback Controls

After creating a `RiveViewModel` to display, you can invoke animation playback control methods on a reference to this view model.

Very often that will be all that is needed to display your Rive asset. However, we have some convenient controls for when you want more fine-grained control of when it plays and doesn't.

You can also choose the loop mode of the animation as additional parameters as needed.\
\
Along with playing animations, you similarly have the ability to pause, stop, and reset animation(s).

Playing without&#x20;

* `play(animationName: String? = nil, loop: Loop = .autoLoop, direction: Direction = .autoDirection)`
  * `animationName` - Name of the animation to play
  * `loop` - Loop mode to play the animation in. Possible values listed below:
    * `oneShot` - plays animation through once
    * `loop` - plays through animation and repeats from the set starting time
    * `pingPong` - plays animation from start -> end, then end -> start on repeat
    * `autoLoop` (default) - plays through the loop setting set on the animation
  * `direction` - Direction to play the animation in
    * `backwards` - plays through animation timeline backward
    * `forwards` - plays through animation timeline forwards
    * `autoDirection` - plays through direction set on the animation
* `pause()`
* `stop()`
* `reset()`

### Play

If you set autoplay to false you can play the active animation or state machine very simply.

```swift
simpleVM.play()
```

If there are multiple animations on the active artboard you can play a specific one.

```swift
simpleVM.play(animationName: "Fancy Animation")
```

### Pause/Stop/Reset

Based on certain events in your app you may want to adjust the playback further.

```swift
simpleVM.pause()
simpleVM.stop()
simpleVM.reset()
```

####

#### Player Delegates

This runtime allows for delegates that can be set on the `RiveViewModel`. You can use delegates to define functions that hook into when certain playback events are invoked. See the below class for how you can hook into the following playback events:

* played
* paused
* stopped
* advanced
* animation looped

```swift
class ToggleViewModel: RiveViewModel {
  private let onAnimation: String = "On"
  private let offAnimation: String = "Off"
  private let startAnimation: String = "StartOff"
  
  var action: ((Bool) -> Void)? = nil
  var isOn = false {
      didSet {
          stop()
          play(animationName: isOn ? onAnimation : offAnimation)
          action?(isOn)
      }
  }
  
  init() {
      super.init(fileName: "toggle", animationName: startAnimation, fit: .cover)
  }
  
  func view(_ action: ((Bool) -> Void)? = nil) -> some View {
      self.action = action
      return super.view().frame(width: 100, height: 50, alignment: .center)
  }

  // When an animation is played
  override func player(playedWithModel riveModel: RiveModel?) {
    if let animationName = riveModel?.animation?.name() {...}
  }
  // When an animation is paused
  override func player(pausedWithModel riveModel: RiveModel?) {
    if let animationName = riveModel?.animation?.name() {...}
  }
  // When an animation is stopped
  override func player(stoppedWithModel riveModel: RiveModel?) {
    if let animationName = riveModel?.animation?.name() {...}
  }
  // When an animation is looped
  override func player(loopedWithModel riveModel: RiveModel?, type: Int) {
    if let animationName = riveModel?.animation?.name() {...}
  }
  // When an animation is advanced
  override func player(didAdvanceby seconds: Double, riveModel: RiveModel?) {...}
}
```
{% endtab %}

{% tab title="Android" %}
#### Invoking Playback Controls

After setting the Rive Resource with your animation view, you can invoke animation playback control methods.\


Along with programmatically playing an animation, you can also choose the loop mode and direction of the animation as additional parameters as needed.\
\
You can additionally pause or stop an animation as well.

```kotlin
// Play one animation
animationView.play("rollaround")

// Set loop mode and direction
animationView.play("rollaround", Loop.ONE_SHOT, Direction.Backwards)

animationView.pause()
animationView.pause("bouncing")

animationView.stop()
animationView.stop("bouncing")
```

####

#### Animation Event Listeners

The Rive Android runtime also allows listener registration. Take a look at the events section in the [rive player](https://github.com/rive-app/rive-android/blob/master/app/src/main/java/app/rive/runtime/example/AndroidPlayerActivity.kt) for an example of how this works.

```kotlin
val listener = object : Listener {
    override fun notifyPlay(animation: PlayableInstance) {
        var text: String? = null
        if (animation is LinearAnimationInstance) {
            text = animation.name
        }
        ..
    }

    override fun notifyPause(animation: PlayableInstance) {
        var text: String? = null
        if (animation is LinearAnimationInstance) {
            text = animation.name
        }
        ..
    }

    override fun notifyStop(animation: PlayableInstance) {
        var text: String? = null
        if (animation is LinearAnimationInstance) {
            text = animation.name
        }
        ..
    }

    override fun notifyLoop(animation: PlayableInstance) {
        var text: String? = null
        if (animation is LinearAnimationInstance) {
            text = animation.name
        }
        ..
    }
}
animationView.registerListener(listener)
```

\
Check out this Activity example to see the usage of playback controls:\
[https://github.com/rive-app/rive-android/blob/master/app/src/main/java/app/rive/runtime/example/LoopModeActivity.kt](https://github.com/rive-app/rive-android/blob/master/app/src/main/java/app/rive/runtime/example/LoopModeActivity.kt)
{% endtab %}
{% endtabs %}

