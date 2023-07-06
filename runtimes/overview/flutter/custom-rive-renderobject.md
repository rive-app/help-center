---
description: Extend RiveRenderObject to perform more advanced operations.
---

# Custom Rive RenderObject

It is possible to have finer control over your Rive animation at runtime by extending `RiveRenderObject`. This allows you to override low-level methods such as `advance`, `beforeDraw`, and `draw` to have more control and optionally perform additional operations. See below for example usage.

{% hint style="info" %}
Note that this is a low-level API, and under most circumstances, it is preferable to make use of `RiveAnimation` or `Rive` widgets instead.
{% endhint %}

### Example Code

The following is a basic example that demonstrates how to paint a Rive animation and advance a state machine using a custom [RenderObject](https://api.flutter.dev/flutter/rendering/RenderObject-class.html).

```dart
import 'package:flutter/material.dart';
import 'package:rive/math.dart';
import 'package:rive/rive.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: MyRiveWidget(),
    );
  }
}

class MyRiveWidget extends StatefulWidget {
  const MyRiveWidget({Key? key}) : super(key: key);

  @override
  State<MyRiveWidget> createState() => _MyRiveWidgetState();
}

class _MyRiveWidgetState extends State<MyRiveWidget> {
  Artboard? _riveArtboard;

  Future<void> _load() async {
    // You need to manage adding the controller to the artboard yourself,
    // unlike with the RiveAnimation widget that can handle a lot of this logic
    // for you by simply providing the state machine (or animation) name.
    final file = await RiveFile.asset('assets/little_machine.riv');
    final artboard = file.mainArtboard;
    final controller = StateMachineController.fromArtboard(
      artboard,
      'State Machine 1',
    );
    artboard.addController(controller!);
    setState(() => _riveArtboard = artboard);
  }

  @override
  void initState() {
    super.initState();
    _load();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: _riveArtboard == null
            ? const SizedBox()
            : CustomRiveRenderObjectWidget(
                artboard: _riveArtboard!,
                fit: BoxFit.contain,
              ),
      ),
    );
  }
}

class CustomRiveRenderObjectWidget extends LeafRenderObjectWidget {
  final Artboard artboard;
  final BoxFit fit;
  final Alignment alignment;

  const CustomRiveRenderObjectWidget({
    super.key,
    required this.artboard,
    this.fit = BoxFit.contain,
    this.alignment = Alignment.center,
  });

  @override
  RenderObject createRenderObject(BuildContext context) {
    return CustomRiveRenderObject(artboard as RuntimeArtboard)
      ..artboard = artboard
      ..fit = fit
      ..alignment = alignment;
  }

  @override
  void updateRenderObject(
      BuildContext context, covariant CustomRiveRenderObject renderObject) {
    renderObject
      ..artboard = artboard
      ..fit = fit
      ..alignment = alignment;
  }

  @override
  void didUnmountRenderObject(covariant CustomRiveRenderObject renderObject) {
    renderObject.dispose();
  }
}

class CustomRiveRenderObject extends RiveRenderObject {
  CustomRiveRenderObject(super.artboard) {
    _artboardReference = artboard;
  }

  late final RuntimeArtboard _artboardReference;

  @override
  bool advance(double elapsedSeconds) {
    // The super method will update the animation and advance the artboard.
    // You can either advance the artboard yourself, or call the super method.
    return _artboardReference.advance(elapsedSeconds, nested: true);
		// Example showing how to advance the artboard at twice the speed.
    return super.advance(elapsedSeconds * 2);
  }

  @override
  void beforeDraw(Canvas canvas, Offset offset) {
    // Called before `draw`. Can be used to perform clipping, for example.
    super.beforeDraw(canvas, offset);
  }

  @override
  void draw(Canvas canvas, Mat2D viewTransform) {
    // Here you can tap into the draw method and perform custom operations.
    super.draw(canvas, viewTransform);
  }
}
```

The artboard can be controlled and configured as it normally would, through a `StateMachineController` (or any other animation controller).

### Example Usage

* [Dynamically update component colors at runtime](https://github.com/HayesGordon/rive\_flutter\_runtime\_color\_change\_example) - Make use of a custom Rive render object to change a shape's fill color, accessing it by name. This is helpful for when a color's opacity is animating, but the color needs to be changed at runtime.
* [Track a Rive component in Flutter](https://github.com/luigi-rosso/rive\_flutter\_painting\_context/) - Track a Rive component’s position at runtime and overlay a Flutter widget or perform additional painting operations.

### Additional Documenation

For additional information on RenderObjects, see the official Flutter examples:

* [RenderObject documentation](https://api.flutter.dev/flutter/rendering/RenderObject-class.html)
* [How to build a RenderObject](https://www.youtube.com/watch?v=cq34RWXegM8)

