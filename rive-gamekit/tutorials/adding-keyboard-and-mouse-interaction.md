# Adding Keyboard and Mouse Interaction

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/adding-keyboard-and-mouse-interaction/dochb2K8yEKw).
{% endhint %}

As the Rive GameKit is built on top of Flutter, it’s possible to add a wide range of peripheral inputs. This tutorial demonstrates how to add keyboard and mouse interaction to your games.

There are a number of commonly used Flutter widgets that handle keyboard and mouse input:

1. [Focus](https://api.flutter.dev/flutter/widgets/Focus-class.html) and [FocusNode](https://api.flutter.dev/flutter/widgets/FocusNode-class.html): This is used to manage focus on a widget. It provides callbacks for when the widget gains or loses focus, which can be useful for handling keyboard input.
2. [MouseRegion](https://api.flutter.dev/flutter/widgets/MouseRegion-class.html): This widget provides callbacks for handling mouse events, such as hovering, entering, and leaving.
3. [Listener](https://api.flutter.dev/flutter/widgets/Listener-class.html): A widget that calls callbacks in response to common pointer events.
4. [Shortcuts](https://api.flutter.dev/flutter/widgets/Shortcuts-class.html): This widget can be used to define keyboard shortcuts for specific actions in your app. It provides callbacks for when a shortcut is triggered.
5. [RawKeyboardListener](https://api.flutter.dev/flutter/widgets/RawKeyboardListener-class.html): This widget listens for raw keyboard events, such as key presses, and provides callbacks for handling those events.
6. [GestureDetector](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html): This widget can handle a variety of mouse and touch events, including clicks, double-clicks, scrolling, panning, etc.
7. [MouseTracker](https://api.flutter.dev/flutter/rendering/MouseTracker-class.html): This widget tracks the relationship between mouse devices and annotations and triggers mouse events and cursor changes accordingly.

Let’s explore the input mechanism for the Centaur Game.

<figure><img src="../../.gitbook/assets/rive-gamekit-keyboard and mouse movement.gif" alt=""><figcaption><p>Rive GameKit - Keyboard and Mouse control</p></figcaption></figure>

This game supports the following controls:

* Keyboard input - move the player left and right
* Mouse pointer location - the direction the player should point at
* Mouse scroll - zoom in and out
* Mouse click - fire arrow

Below is an excerpt from the game code that handles input:

```dart
...

return Focus(
  focusNode: FocusNode(
    canRequestFocus: true,
    onKeyEvent: (node, event) {
      if (event is KeyRepeatEvent) {
        return KeyEventResult.handled;
      }
      double speed = 0;
      if (event is KeyDownEvent) {
        speed = 1;
      } else if (event is KeyUpEvent) {
        speed = -1;
      }
      if (event.logicalKey == LogicalKeyboardKey.keyA) {
        _centaurPainter!.move -= speed;
      } else if (event.logicalKey ==
          LogicalKeyboardKey.keyD) {
        _centaurPainter!.move += speed;
      }
      return KeyEventResult.handled;
    },
  )..requestFocus(),
  child: MouseRegion(
    onHover: (event) => _centaurPainter!.aimAt(
      event.localPosition * window.devicePixelRatio,
    ),
    child: Listener(
      behavior: HitTestBehavior.opaque,
      onPointerDown: _centaurPainter!.pointerDown,
      onPointerMove: (event) => _centaurPainter!.aimAt(
        event.localPosition * window.devicePixelRatio,
      ),
      onPointerSignal: (event) {
        if (event is PointerScrollEvent) {
          _centaurPainter!.zoom(event.scrollDelta.dy / 1000);
        }
      },
      child: _renderTexture.widget(_centaurPainter!),
    ),
  ),
),

...
```

The input mechanisms can be broken down as follows:

1. **Focus** and **FocusNode** are responsible for keyboard input - **keyA** and **keyB** move left and right, respectively. The `event` argument provides additional information, such as **KeyRepeatEvent**, **KeyDownEvent**, and more.
2. **FocusNode** ensures the **Focus** widget receives keyboard focus by calling `requestFocus`.
3. The **MouseRegion** is used to get the mouse position on screen which determines where the character is aiming at. This is only triggered by a mouse cursor, not touch events on mobile.
4. The **Listener** is responsible for receiving pointer events. These pointer events can also be touch events (on mobile or touch-supported devices).
   1. `onPointerDown`: on touch or on click
   2. `onPointerMove`: called when a pointer is on screen and moving, for example when moving your finger or mouse after touching/clicking
   3. `onPointerSignal`: additional pointer events, for example detecting scroll events.
   4. `HitTestBehavior.opaque`: ensures that opaque targets can be hit by hit tests, which enables the drawable area for the Rive `RenderTexture` to be hit.

{% hint style="warning" %}
Note that pointer events are multiplied by the device’s pixel ratio, as the GameKit window size takes the pixel ratio into account.
{% endhint %}

### Conclusion

You’re free to use any input mechanisms that Flutter supports. For additional examples, see:

* Joel Game
* Goblin Slayer Game
