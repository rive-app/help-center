---
description: Fundamentals Guide
---

# Fundamentals

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/fundamentals/docM72mRgBW7).
{% endhint %}

To get started using the Rive GameKit, follow along with each section below to go through the core steps of loading in a Rive file, rendering its contents, and building the render loop where we’ll display a Zombie animation.

{% hint style="info" %}
**Just want to see the code?** Skip to the [example code](fundamentals.md#example-code) section for the full code example.
{% endhint %}

<figure><img src="../.gitbook/assets/CleanShot 2023-03-31 at 00.02.35.gif" alt=""><figcaption><p>Drawing a Zombie with the Rive GameKit</p></figcaption></figure>

## Getting Started - Flutter

If you’re new to Flutter, follow the [official Flutter documentation](https://docs.flutter.dev/get-started/install) to set up your development environment and create your first Flutter app. Make sure to follow all the getting started steps and ensure that running `flutter doctor` produces no issues on your end.

To add the Rive GameKit to your Flutter app, open the `pubspec.yaml` file and add the `rive_gamekit` package. Your dependencies should look something like this:

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  # Add this
  rive_gamekit:
    hosted: https://onepub.dev/api/xuppsdavuh
    version: ^0.0.8
```

Next, add your Rive file assets to your application. In `pubspec.yaml` add the following line:

```yaml
flutter:
  uses-material-design: true
  # Add this
  assets:
    - assets/
```

Within the root of your project, create an **/assets** folder, and add your **.riv** files to it. You can create your own animations on [rive.app](https://rive.app/) or get inspiration from the [community](https://rive.app/community/).

{% hint style="warning" %}
Currently, you can only make use of vector-based graphics for the Rive GameKit
{% endhint %}

When using the Rive GameKit API’s import it from the `package:rive_gamekit` package. For the code snippets below, we’ll alias the API with `rive`.

```dart
import 'package:rive_gamekit/rive_gamekit.dart' as rive;
```

## Reading a Rive File

{% hint style="info" %}
Download the following Rive file below for the rest of the tutorials
{% endhint %}

{% file src="../.gitbook/assets/zombie.riv" %}

The Rive GameKit provides an API for you to load your Rive file bytes. First, load the file from your **assets/** folder. Once the asset is loaded, get the contents of the file as bytes and pass it to the Rive API to parse it. Once the file is parsed, you can use the Rive API to query the file data for specific Artboards, State Machines, and more, which we’ll get into in a bit.

See below for an example of how to load in a Rive file; we’ll use an example called `zombie.riv` (see the asset above to try it out).

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  void initState() {
    super.initState();
    loadRiveFile();
  }

  Future<void> loadRiveFile() async {
    // Specify the .riv file to load
    final data = await rootBundle.load('assets/zombie.riv');
    final bytes = data.buffer.asUint8List();
    final file = rive.File.decode(bytes);
    // Pass file onto Painter class which will handle file manipulation
  }

  ...
}
```

## Setting up the RenderTexture

The other bit we want to set up is the `RenderTexture` from the GameKit, which is what will allow Rive to draw onto the surface of the app.

You’ll create an instance of a `RenderTexture` via the following line and include this in the render block:

```dart
final rive.RenderTexture _renderTexture = rive.GameKit.instance.makeRenderTexture();
```

When rendering the `RenderTexture` in the `build()` method, you’ll supply it with a Widget class that extends `RenderTexturePainter`, which we’ll go more into in the next section.

Another important part here is to override the `dispose()` method so we can call `dispose()` on the class that extends `RenderTexturePainter`. This appropriately cleans up any underlying C++ objects instanced to free up memory appropriately when the main class gets disposed of.

See below for an example of how to set up the `RenderTexture` instance:

```dart
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class _MyAppState extends State<MyApp> {
  final rive.RenderTexture _renderTexture =
    rive.GameKit.instance.makeRenderTexture();

  // GamePainter is a class that extends rive.RenderTexturePainter
  // See next section for implementation
  GamePainter? _myRivePainter;

  @override
  void initState() {
    super.initState();
    loadRiveFile();
  }

  Future<void> loadRiveFile() async {
    // Specify the .riv file to load
    final data = await rootBundle.load('assets/zombie.riv');
    final bytes = data.buffer.asUint8List();
    final file = rive.File.decode(bytes);
    if (file != null) {
      // Calling setState will ensure that the `build` method is called again.
      setState(() {
        _myRivePainter = GamePainter(file);
      });
    }
  }

  @override
  void dispose() {
    _myRivePainter?.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text("Rive Renderer")),
        body: Center(
          child: _myRivePainter == null
              ? const SizedBox() // It's not yet loaded - display nothing
              : _renderTexture.widget(_myRivePainter!),
        ),
      ),
    );
  }
}
```

## Setting up the RenderTexturePainter

The actual painting context that provides the commands to paint into the `RenderTexture` with the Rive Renderer is a class you create that extends Rive’s `RenderTexturePainter`. This is where you will mainly draw Rive animations onto your texture and coordinate the start of your game scene.

In this class, you can provide it a constructor that takes in the parsed Rive file from above and creates instance(s) of Artboards, State Machines, Inputs, and more.

The class will be responsible for implementing 3 methods:

* `bool paint(RenderTexture texture, Size size, double elapsedSeconds)`
  * Responsible for making the Rive Renderer and drawing to the surface
  * Responsible for coordinating how the Artboards and State Machines “advance” over each frame in a render loop
* `void dispose()`
  * Cleans up any created instances of Rive `File`, `Artboard`, and/or `StateMachine` types
* `Color background`
  * Provide a Color to paint for the background

The general starting outline should look like the below snippet. Note that we’re also creating a `Renderer` inside our `paint()` method. We’ll use the renderer to draw on the texture.

```dart
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

/// This class is responsible for rendering your Rive animations.
///
/// You tap into each call to paint and lay out your artboards and drive
/// the state machines.
class GamePainter extends rive.RenderTexturePainter {
  final rive.File riveFile;

  GamePainter(this.riveFile) {}

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer
    final renderer = rive.Renderer.make();

    // True: something changed that requires repainting for the next frame
    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    super.dispose();
  }

  @override
  Color get background => Colors.white;
}
```

The responsibility of the `RenderTexture` is to call the `paint()` method on the `RenderTexturePainter` class.

## Artboards

{% hint style="info" %}
If you’re not familiar with Artboards in Rive, see our [docs](../editor/fundamentals/artboards.md) on Artboards
{% endhint %}

Next, we’ll extract an `Artboard` from the Rive file in our `GamePainter` class. You can grab a reference to the Artboard by name, or by picking the default Artboard from a file:

* `riveFile.artboard("name-of-artboard")`
* `riveFile.defaultArtboard()`

When we create an instance of an `Artboard`, we can query it for specific components in the draw hierarchy, `StateMachine` references, as well as pass it a Renderer object so the Artboard can draw itself onto the renderer.

With the Rive GameKit, you can also instance multiple of the same `Artboard`. This is nice because you can effectively create multiple independent entities with their own state machine, advance each Artboard separately from another, etc. For example, if you have an Artboard called **Zombie**, you can create one, a dozen, or even hundreds and thousands of **Zombie** instances that can all be painted on the screen at a given frame.

Building on top of our `GamePainter` class, create a class variable of type `Artboard`.

```dart
class GamePainter extends rive.RenderTexturePainter {
  final rive.File riveFile;
  late rive.Artboard artboard;

  GamePainter(this.riveFile) {
    // NOTE that the ! operator will throw if any of these values are null.
    // Make sure these exist in your Rive file, or perform null safety
    // checks.
    artboard = riveFile.artboard("Zombie man")!;
  }

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer.
    final renderer = rive.Renderer.make();

    // Draw artboard contents by passing in the renderer
    artboard.draw(renderer);		

    // True: something changed that requires repainting for the next frame
    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    artboard.dispose();
    super.dispose();
  }

  @override
  Color get background => Colors.white;
}
```

<figure><img src="../.gitbook/assets/Screen Shot 2023-03-31 at 12.03.52 AM.png" alt=""><figcaption><p>Artboard drawn without state machine advancement</p></figcaption></figure>

If you run your app now, you should see a static vector blob. This is because we’re not actually advancing any animations yet, and is just the “design mode” of the Artboard. Next, we’ll start rendering the state machine, which will display animations as intended for our zombie.

## State Machines

{% hint style="info" %}
If you’re not familiar with State Machines in Rive, see our [docs](../editor/state-machine/) on State Machines
{% endhint %}

Now that we have an instance of an `Artboard`, we can query it for a `StateMachine` which we can use to advance animations over time in the `paint()` method, as well as grab references to State Machine inputs. Similar to querying for an Artboard, you can query for a State Machine by name or by picking the default State Machine on the artboard:

* `.stateMachine("name-of-state-machine")`
* `.defaultStateMachine()`

When we create an instance of a `StateMachine`, we can “advance” over each frame a set amount of time, which is to actually play the animation states in the render loop. Later on, we’ll go over what it means to advance the state machine by a set amount of time (`elapsedSeconds` in the case below). We can also query the State Machine for inputs that we can control programmatically. The zombie example has a state machine called “Motion” that starts off in an animation state that shows the zombie walking. Building on the `GamePainter` class, we’ll add the following to add the state machine as part of the paint/render loop:

```dart
class GamePainter extends rive.RenderTexturePainter {
  final rive.File riveFile;
  late rive.Artboard artboard;
  late rive.StateMachine stateMachine;

  GamePainter(this.riveFile) {
    // NOTE that the ! operator will throw if any of these values are null.
    // Make sure these exist in your Rive file, or perform null safety
    // checks.
    artboard = riveFile.artboard("Zombie man")!;
    stateMachine = artboard.stateMachine("Motion")!;
  }

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer.
    final renderer = rive.Renderer.make();

    // Advance the state machine by elapsedSeconds
    stateMachine.advance(elapsedSeconds);

    // Draw artboard contents by passing in the renderer
    artboard.draw(renderer);		

    // True: something changed that requires repainting for the next frame
    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    artboard.dispose();
    stateMachine.dispose();
    super.dispose();
  }

  @override
  Color get background => Colors.white;
}
```

<figure><img src="../.gitbook/assets/CleanShot 2023-03-31 at 00.02.35.gif" alt=""><figcaption><p>Zombie state machine animations</p></figcaption></figure>

If you run the app now, you should see a walking zombie! This displays the intentional keyed properties at each frame as intended when building the animations from the Rive editor. Next, we’ll explore how to control the state machine programmatically using state machine inputs.

## Inputs

State Machine inputs will allow us to transition between different animation states based on a model defined at animate time. We can get references to these inputs from the `StateMachine` instance we created above and then bind them to data in our app or any defined values.

### Number Inputs

Number inputs allow you to set any `double` value type.

### Boolean Inputs

Boolean inputs allow you to set any `bool` value type.

### Trigger Inputs

Trigger inputs should be treated as a value-less action to take. You can invoke the `.fire()` command on this input type. Think of it like a “jump” command on a character or a “shoot” command for a weapon.

If you inspect the Zombie Rive file in the editor, you’ll see a number of different state machine inputs on the **Motion** state machine that help define several properties and actions for the Zombie, such as the pose of the zombie (defined by a number input), the type of skin a zombie has (defined by a number input), whether the zombie has died (defined by a boolean input), and more. As you can tell, state machines can be quite comprehensive in the models defined for a single Artboard, all of which can be driven at runtime in various ways.

For the purpose of this example, let’s add a number input to change the skin of the zombie by referencing the **numSkins** input in this State Machine.

```dart
class GamePainter extends rive.RenderTexturePainter {
  final rive.File riveFile;
  late rive.Artboard artboard;
  late rive.StateMachine stateMachine;
  late rive.NumberInput skinType;

  GamePainter(this.riveFile) {
    // NOTE that the ! operator will throw if any of these values are null.
    // Make sure these exist in your Rive file, or perform null safety
    // checks.
    artboard = riveFile.artboard("Zombie man")!;
    stateMachine = artboard.stateMachine("Motion")!;
    skinType = stateMachine.number("numSkins")!;
    skinType.value = 2;
  }

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer.
    final renderer = rive.Renderer.make();

    // Advance the state machine by elapsedSeconds
    stateMachine.advance(elapsedSeconds);

    // Draw artboard contents by passing in the renderer
    artboard.draw(renderer);

    // True: something changed that requires repainting for the next frame
    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    artboard.dispose();
    stateMachine.dispose();
    super.dispose();
  }

  @override
  Color get background => Colors.white;
}
```

🕹️ Try changing the `skinType.value` to a different value, like `1` or `3`. The state machine is set up to enumerate different skins for the zombie based on a set number of values.

## Components

Another core class from GameKit is the ability to get/set a few position and transform properties of a specific component within the draw hierarchy or even the world transform. You can query for a specific component on the `Artboard` instance by name. This is convenient if you need to adjust some transform properties of a specific component in the draw hierarchy dynamically rather than defining them at animate time.

To grab a component, call the following API on the `Artboard` instance with the name of the component you want to reference:

```dart
artboard = riveFile.artboard("Zombie man")!;
final characterComponent = artboard.component("Character")!;
```

### Position

You can get or set the `x` and `y` position values of a component (in local space) as a property on the `Component` instance. For example:

```dart
final characterComponent = artboard.component("Character")!;
double x = characterComponent.x;
double y = characterComponent.y;
debugPrint("Current X and Y positions ($x $y)");
```

### Rotation

You can get or set the `rotation` value of a component as a property on the `Component` instance. For example:

```dart
final characterComponent = artboard.component("Character")!;
double rotationValue = characterComponent.rotation;
debugPrint("Current rotation value ($rotationValue)");
```

### Scale

You can get or set the `scaleX` and `scaleY` scale values of a component as a property on the `Component` instance. For example:

```dart
final characterComponent = artboard.component("Character")!;
double scaleX = characterComponent.scaleX;
double scaleY = characterComponent.scaleY;
debugPrint("Current Scale X and Y values ($scaleX $scaleY)");
```

### World Transform

You can get or set the `worldTransform` value of a component as a property on the `Component` instance as well. World transform is represented as a `Mat2D` type, which is an array of 6 numeric values. The world transform matrix defines transformations such as translation, rotation, and scaling of the component in relation to the “world” coordinate system around it. One benefit of setting the `worldTransform` would be to position a `Component` in its world space with respect to other components in the same world space.

In the [Centaur game example](https://github.com/rive-app/rive-gamekit-examples/blob/main/examples/fundamentals/centaur-world-transform.dart), we set the `worldTransform` of a node that follows a user’s cursor in the world scene in order to have a centaur’s gaze and bow follow along.

## Elapsed Time

In the `paint()` method of our `GamePainter` class, we are passed in an `elapsedSeconds` parameter of type `double`. This value is the time since the last paint, represented in seconds. Normally, you could pass the `elapsedSeconds` to the `.advance()` method of your State Machine instance to play animations back at 1x speed. However, you could also use a multiple of `elapsedSeconds` to manipulate the speed at which the state machine advances.

For example, the following shows advancing a state machine by the `paint()` method’s given `elapsedTime`.

```dart
@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  . . .
  stateMachine.advance(elapsedSeconds);
  . . .
}
```

<figure><img src="../.gitbook/assets/CleanShot 2023-03-30 at 23.58.16.gif" alt=""><figcaption><p>State machine advancing by elapsed seconds</p></figcaption></figure>

We can also advance by 2x speed by multiplying `elapsedSeconds` by 2. Imagine a play head being moved 2x as many frames away on the state machine for a given animation. Thus, Rive would draw twice as many frames in the same time it takes to paint.

```dart
@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  . . .
  stateMachine.advance(elapsedSeconds * 2);
  . . .
}
```

<figure><img src="../.gitbook/assets/CleanShot 2023-03-30 at 23.55.59.gif" alt=""><figcaption><p>State machine advancing by double the elapsed seconds</p></figcaption></figure>

## Advancing Animations

As you saw above, we advance state machines by a set amount of time (in seconds). This is how we tell animations what point in the timeline to draw to, as well as all the frames in between. There are currently three main ways to advance state machines with the Rive GameKit:

* `.advance()` - On a single `StateMachine` instance
* `.batchAdvance()` - On the `Rive` class, which allows you to advance multiple state machines in a batch on multiple threads
* `.batchAdvanceAndRender()` - On the `Rive` class, which allows you to still advance multiple state machines in a batch on multiple threads but also renders each of their associated artboards

You can read up on the main differences in when to use which `advance` method for better performance and best practices on the [Advancing State Machines](advancing-state-machines.md) page.

## Disposing

Disposing of created instances for our Rive File, Artboards, and State Machines properly cleans up any allocated memory when we’re done with them. For example, when the user wins or loses a game, and you’re ready to be done with the `RenderTexture`, simply call `.dispose()` on any unneeded instances.

In our `GamePainter` example, we override the class’ `dispose()` method and delete all our instances there:

```dart
@override
void dispose() {
  riveFile.dispose();
  artboard.dispose();
  stateMachine.dispose();
  super.dispose();
}
```

Note that you do not need to dispose just in the `dispose()` method of your `RenderTexturePainter` class. You can dispose (and are encouraged to do so) as soon as you no longer need an instance of a `StateMachine` and/or `Artboard`. An example would be if you have hundreds of zombie artboards on screen, and after a zombie dies and is offscreen, you can dispose of the artboard and associated state machine instance as they are no longer needed, all while the class is still painting your other zombies, heroes, etc.

{% hint style="info" %}
**Note:** You do not need to call \`.dispose()\` on every Rive object, such as state machine inputs, or components.
{% endhint %}

## Example Code

Here's the code that we followed to render a walking zombie:

{% tabs %}
{% tab title="MyApp" %}
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  final rive.RenderTexture _renderTexture =
      rive.GameKit.instance.makeRenderTexture();
  GamePainter? _zombiePainter;

  @override
  void initState() {
    super.initState();

    load();
  }

  Future<void> load() async {
    var data = await rootBundle.load('assets/zombie.riv');
    var bytes = data.buffer.asUint8List();
    var file = rive.File.decode(bytes);
    if (file != null) {
      setState(() {
        _zombiePainter = GamePainter(file);
      });
    }
  }

  @override
  void dispose() {
    super.dispose();
    _zombiePainter?.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: ColoredBox(
          color: const Color(0xFF507FBA),
          child: Center(
            child: _zombiePainter == null
                ? const SizedBox()
                : _renderTexture.widget(_zombiePainter!),
          ),
        ),
      ),
    );
  }
}
```
{% endtab %}

{% tab title="GamePainter" %}
```dart
class GamePainter extends rive.RenderTexturePainter {
  final rive.File riveFile;
  late rive.Artboard artboard;
  late rive.StateMachine stateMachine;
  late rive.NumberInput skinType;

  GamePainter(this.riveFile) {
    // NOTE that the ! operator will throw if any of these values are null.
    // Make sure these exist in your Rive file, or perform null safety
    // checks.
    artboard = riveFile.artboard("Zombie man")!;
    stateMachine = artboard.stateMachine("Motion")!;
    skinType = stateMachine.number("numSkins")!;
    skinType.value = 2;
  }

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer.
    final renderer = rive.Renderer.make();

    // Advance the state machine by elapsedSeconds
    stateMachine.advance(elapsedSeconds);

    // Draw artboard contents by passing in the renderer
    artboard.draw(renderer);

    // True: something changed that requires repainting for the next frameR
    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    artboard.dispose();
    stateMachine.dispose();
    super.dispose();
  }

  @override
  Color get background => Colors.white;
}
```
{% endtab %}
{% endtabs %}
