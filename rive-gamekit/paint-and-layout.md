---
description: Basic techniques to paint and perform layout on the Rive texture area.
---

# Paint & Layout

{% hint style="info" %}
If you’re new to Flutter or the Rive GameKit, please read the [Fundamentals](fundamentals.md) documentation first.
{% endhint %}

If you’re familiar with Flutter’s [CustomPainter](https://api.flutter.dev/flutter/rendering/CustomPainter-class.html) then some of the content in this section will feel familiar to you.

### RenderTexturePainter

As discussed in the [Fundementals](../editor/fundamentals/) section, the **RenderTexturePainter** class is responsible for laying out, painting, and advancing your animations. Essentially all of your game logic is driven from this class - as the elapsed seconds ([delta time](https://en.wikipedia.org/wiki/Delta\_timing)) since the previous frame is provided within the `paint` method.

To recap, let's take a look at a simple example that draws a single artboard to the screen:

```dart
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class MyRivePainter extends rive.RenderTexturePainter {
  
  ...

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    // Make a renderer.
    final renderer = rive.Renderer.make();

    // Advance the state machine by the elapsed seconds. This is what
    // drives the animation.
    stateMachine.advance(elapsedSeconds);

    // Draw artboard content
    artboard.draw(renderer);

    return true;
  }

  @override
  Color get background => Colors.white;
}
```

In the `paint` method, you:

1. Create a new **Renderer** object and pass it into `arboard.draw`. This is what draws the artboard to the Rive texture.
2. Advance the current state machine with the elapsed time. This is what drives the animation forward.
3. Return **true**, which ensures that the paint method is called again on the next frame.

{% hint style="info" %}
The size provided in the **paint** method is equivalent to the Flutter window size multiplied by the device's pixel ratio.
{% endhint %}

In the `background` method you specify the **color** of the background. You can make this transparent if needed, `Colors.transparent`, or update it each frame if you want.

<figure><img src="../.gitbook/assets/CleanShot 2023-03-23 at 09.02.18@2x.png" alt=""><figcaption><p>Single artboard draw - no transformations applied</p></figcaption></figure>

### Coordinate System

The renderer makes use of a cartesian coordinate system where the positive x-axis extends towards the right, and the positive y-axis extends towards the bottom of the screen. The origin (0,0) is located at the top left corner of the screen.

How this zombie is drawn is dependent on the size and origin of the artboard, as defined in the Rive Editor. The size for this particular artboard is set to **500 x 594.7** and this is the size it’ll render in the GameKit world as well:

<figure><img src="../.gitbook/assets/CleanShot 2023-03-17 at 18.23.44.png" alt=""><figcaption><p>Artboard size - Rive Editor</p></figcaption></figure>

In order to programmatically change the size and positioning you will need to transform the **renderer** object before drawing the artboard. In this way you can layout your game scene.

### Translate, Scale, and Rotate

By applying translations, scales, and rotations you’ll be able to create almost any game! In this section we’ll explore some basic transformations.

{% hint style="info" %}
An understanding of linear algebra is essential for game development. If you’re already familiar with the core concepts then you can glance over this section. If you’d like to learn more, or if you need additional reference, take a look at this [series of videos](https://www.3blue1brown.com/topics/linear-algebra) made by [3Blue1Brown](https://www.3blue1brown.com/).
{% endhint %}

{% hint style="success" %}
If you'd like to follow along you can download the Rive file from this [community link](https://rive.app/community/4886-9870-goblin/).
{% endhint %}

#### **Translate**

We’ll start off by applying a simple translation that makes a goblin walk at the bottom of the screen:

<figure><img src="../.gitbook/assets/rive-gamekit goblin walking bottom.gif" alt=""><figcaption><p>Rive GameKit - translation example</p></figcaption></figure>

The complete painter code is:

```dart
import 'package:flutter/painting.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class DemoPainter extends rive.RenderTexturePainter {
  final rive.File riveFile;
  late rive.Artboard artboard;
  late rive.StateMachine stateMachine;
  late rive.AABB goblinSize;
  late rive.NumberInput directionInput;
  late rive.TriggerInput killGoblin;

  DemoPainter(this.riveFile) {
    artboard = riveFile.defaultArtboard()!;
    goblinSize = artboard.bounds;
    stateMachine = artboard.defaultStateMachine()!;
    directionInput = stateMachine.number('Direction')!;
    directionInput.value = 2.0;
  }

  double posX = 0;

  @override
  Color get background => const Color(0xffffffff);

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    stateMachine.advance(elapsedSeconds);

    final renderer = rive.Renderer.make();

    final posY = size.height - goblinSize.height;

    renderer.save();
    renderer.translate(posX, posY);
    artboard.draw(renderer);
    renderer.restore();

    // Set the goblin direction to left or right.
    if (directionInput.value == 2.0) {
      posX += 4.0;
    } else if (directionInput.value == 4.0) {
      posX -= 4.0;
    }

    // Switch the direction of the goblin when the bounds are hit
    if (posX >= (size.width - goblinSize.width)) {
      directionInput.value = 4.0;
    } else if (posX <= 0) {
      directionInput.value = 2.0;
    }

    return true;
  }

  @override
  void dispose() {
    riveFile.dispose();
    super.dispose();
  }
}
```

To apply translations you can call the `translate` method on the renderer and pass in an `x` and `y` value to specify the offset. In this example the offset is calculated using a combination of the window and goblin size.

Before applying any transformation you need to save the state the renderer object is in by calling `save`. Thereafter you can apply transformations on the saved renderer. Upon calling `restore` the transformations applied will be discarded. In this way you can easily apply transformations to multiple draw commands and configure your scene - we’ll see more examples of this later.

Something else to take note of is that the goblin’s direction is changed by updating a State Machine number input, called **Direction**. This is a specific input made for this animation:

<figure><img src="../.gitbook/assets/Rive Editor - number input example.gif" alt=""><figcaption><p>Rive Editor: Number input example</p></figcaption></figure>

#### Scale

Let’s make our goblin bigger by transforming the renderer.

<figure><img src="../.gitbook/assets/rive gamekit - scale transformation.gif" alt=""><figcaption><p>Rive GameKit - scale example</p></figcaption></figure>

The updated paint code:

```dart
...

@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  stateMachine.advance(elapsedSeconds);

  final renderer = rive.Renderer.make();

  const scale = 2.0; // Goblin's scale
  final posY = size.height - (goblinSize.height * scale);

  renderer.save();

  renderer.translate(posX, posY);
  renderer.transform(rive.Mat2D.fromScale(scale, scale)); // Update the transform
  artboard.draw(renderer);

  renderer.restore();

  if (directionInput.value == 2.0) {
    posX += 4.0;
  } else if (directionInput.value == 4.0) {
    posX -= 4.0;
  }

  if (posX >= (size.width - (goblinSize.width * scale))) {
    directionInput.value = 4.0;
  } else if (posX <= 0) {
    directionInput.value = 2.0;
  }

  return true;
}
```

The code above creates a scaled Matrix 2D using the `fromScale` method, and passes that into the `transform` method. The applied matrix transformation scales up the renderer by a factor of two.

The `posX` and `posY` calculations are also slightly different to account for the bigger goblin size.

#### Rotation

Let’s defy gravity and make the goblin walk upside down.

<figure><img src="../.gitbook/assets/rive gamekit - rotation example.gif" alt=""><figcaption><p>Rive GameKit - rotation example</p></figcaption></figure>

The updated code:

<pre class="language-dart"><code class="lang-dart"><strong>...
</strong><strong>
</strong><strong>@override
</strong>bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  stateMachine.advance(elapsedSeconds);

  final renderer = rive.Renderer.make();

  const scale = 1.0;
  final posY = goblinSize.height;

  renderer.save();

  renderer.translate(posX, posY);
  renderer.transform(rive.Mat2D.fromRotation(rive.Mat2D(), pi));
  renderer.transform(rive.Mat2D.fromScale(-scale, scale));

  artboard.draw(renderer);

  renderer.restore();

  if (directionInput.value == 2.0) {
    posX += 4.0;
  } else if (directionInput.value == 4.0) {
    posX -= 4.0;
  }

  if (posX >= (size.width - (goblinSize.width * scale))) {
    directionInput.value = 4.0;
  } else if (posX &#x3C;= 0) {
    directionInput.value = 2.0;
  }

  return true;
}
</code></pre>

The code creates a rotation matrix, using `fromRotation`. It also updates the `posY` calculation and sets the **x** scale to be negative - to ensure the position is correct and the goblin is not walking in reverse. Experiment with the above and see what you can create.

#### Save and Restore - Multiple Draws

Let’s draw two goblins at the same time - combining what we’ve learned so far.

<figure><img src="../.gitbook/assets/rive gamekit - multiple draws.gif" alt=""><figcaption></figcaption></figure>

Updated code:

```dart
@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  stateMachine.advance(elapsedSeconds);

  final renderer = rive.Renderer.make();

  const scale = 1.0;

  // Bottom Goblin
  {
    renderer.save();
    renderer.translate(posX, size.height - goblinSize.height);
    artboard.draw(renderer);
    renderer.restore();
  }

  // Top Goblin
  {
    renderer.save();
    renderer.translate(posX, goblinSize.height);
    renderer.transform(rive.Mat2D.fromRotation(rive.Mat2D(), pi));
    renderer.transform(rive.Mat2D.fromScale(-scale, scale));
    artboard.draw(renderer);
    renderer.restore();
  }

  if (directionInput.value == 2.0) {
    posX += 4.0;
  } else if (directionInput.value == 4.0) {
    posX -= 4.0;
  }

  if (posX >= (size.width - (goblinSize.width * scale))) {
    directionInput.value = 4.0;
  } else if (posX <= 0) {
    directionInput.value = 2.0;
  }

  return true;
}
```

By making use of **save** and **restore** we can apply transformations to specific draw commands and reset the renderer to the state it was in before. This ensures that we can create isolated transforms for specific draw commands. In this way you can create your whole game scene.

#### Combining Transforms

In the rotate example we made us of the following transformations:

```dart
 renderer.translate(posX, goblinSize.height);
 renderer.transform(rive.Mat2D.fromRotation(rive.Mat2D(), pi));
 renderer.transform(rive.Mat2D.fromScale(-scale, scale));
```

Instead of creating three separate renderer transformations, you can create a single transformation matrix by multiplying the values and passing it in once:

```dart
rive.Mat2D mTranslate = rive.Mat2D.fromTranslate(posX, goblinSize.height);
rive.Mat2D mRotate = rive.Mat2D.fromRotation(rive.Mat2D(), pi);
rive.Mat2D mScale = rive.Mat2D.fromScale(-scale, scale);

renderer.transform(mTranslate.mul(mRotate).mul(mScale));
```

Or you could create the Matrix2D yourself and set each value directly. Here is an example of a view transform that follows the player around on screen:

```dart
final rive.Mat2D _viewTransform = rive.Mat2D();

...

_viewTransform[0] = _cameraZoom;
_viewTransform[1] = 0;
_viewTransform[2] = 0;
_viewTransform[3] = _cameraZoom;
_viewTransform[4] = -_cameraTranslation.x * _cameraZoom + size.width / 2.0;
_viewTransform[5] = -_cameraTranslation.y * _cameraZoom + size.height / 2.0;
```

These examples demonstrate simple transformations and can become as complex as required for your game’s needs. See our game tutorials and examples for more in depth techniques.

#### Alignment

The Rive GameKit provides a convenient API to layout, size, and align your artboards.

As an example we’ll experiment with a tile map artboard, with a size of **2048 x 2048**. Imagine that this is your game’s ground and you need to make sure it fits correctly within your game window.

<figure><img src="../.gitbook/assets/rive editor - ground artboard.png" alt=""><figcaption><p>Rive Editor - Background tile example</p></figcaption></figure>

Running the following code:

```dart
import 'package:flutter/material.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class GameDemoPainter extends rive.RenderTexturePainter {
  final rive.File file;
  late final rive.Artboard artboard;

  GameDemoPainter(this.file) {
    artboard = file.artboard('main')!;
  }

  @override
  Color get background => Colors.white;

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    final renderer = rive.Renderer.make();
    artboard.draw(renderer);

    return true;
  }

  @override
  void dispose() {
    file.dispose();
    super.dispose();
  }
}
```

Renders:

<figure><img src="../.gitbook/assets/rive gamekit - background tile.png" alt=""><figcaption><p>Rive GameKit - Full background tile</p></figcaption></figure>

Part of the ground is cut off, as the window size is smaller than the artboard size. The opposite will be true if the window size is bigger than the artboard, then you will see the background color (as defined in the paint class).

What we want to do instead is transform the renderer so that it fits the artboard to the available size.

The `renderer.align` method makes this easy. Let’s update the code to the following:

```dart
@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  final renderer = rive.Renderer.make();
  
  final frameBounds = rive.AABB.fromMinMax(
    rive.Vec2D(),
    rive.Vec2D.fromValues(size.width, size.height),
  );

  renderer.save();
  renderer.align(
    BoxFit.contain,
    Alignment.center,
    frameBounds,
    artboard.bounds,
  );
  artboard.draw(renderer);
  renderer.restore();
  return true;
}
```

The `align` method takes in a [BoxFit](https://api.flutter.dev/flutter/painting/BoxFit.html), [Alignment](https://api.flutter.dev/flutter/painting/Alignment-class.html), and two [AABB](paint-and-layout.md#axis-aligned-bounding-box-aabb) (Axis-Aligned Bounding Box) bounds to specify the frame and content size.

The **frame bounds** is the size of our window (or drawable area) and the **content bounds** is the size of the content that you want to draw.

We’ll discuss AABBs more in a bit, as well as the `fromMinMax` function.

<figure><img src="../.gitbook/assets/rive gamekit - align and boxfit contain.png" alt=""><figcaption><p>Align and BoxFit Contain</p></figcaption></figure>

As you can see the artboard is now contained to the available screen size. Experiment with different values for **BoxFit**, **Alignment**, and **bounds**. Try setting the frame bounds to be half the size and see what happens.

For this example, to get our ground rendering correctly and not show any of the white background, set the BoxFit to **cover**.

```dart
renderer.align(
  BoxFit.cover,
  Alignment.center,
  frameBounds,
  artboard.bounds,
);
```

Now, regardless of the window size, the content will always fill the available size in the most optimal way.

<figure><img src="../.gitbook/assets/rive gamekit - resizing with align.gif" alt=""><figcaption></figcaption></figure>

You can also make use of the `computeAlignment` method which give you the `Mat2D` and then you can pass that into `renderer.transform` yourself - the `align` method combines these operations. This is useful if you want to apply other transformations on the computed matrix.

### Axis-Aligned Bounding Box (AABB)

In 2D game development, an [AABB](https://developer.mozilla.org/en-US/docs/Games/Techniques/3D\_collision\_detection) (axis-aligned bounding box) is a rectangular shape that is aligned with the X and Y axes of a 2D coordinate system. It can be used to represent the bounding box of a Rive artboard or other object in the game world.

An AABB is defined by two points: a minimum point and a maximum point. The minimum point represents the top-left corner of the rectangle, and the maximum point represents the bottom-right corner of the rectangle. These points are represented as 2D vectors, each containing two coordinates (X and Y). You already made use of this to calculate the bounding box of the window size:

```dart
final frameBounds = rive.AABB.fromMinMax(
  rive.Vec2D(),
  rive.Vec2D.fromValues(size.width, size.height),
);
```

The purpose of an AABB is to define the spatial extent of an object in the game world, which can be used for various calculations such as collision detection, visibility testing, and culling. By using an AABB to represent the bounding box of an artboard (or component), it is possible to quickly determine if two shapes are overlapping or intersecting, which is essential for implementing collision detection.

In addition to collision detection, an AABB can also be used to optimize rendering by performing culling - the process of removing objects from the rendering pipeline that are outside of the camera's view. By checking if an AABB is outside of the camera's view, it is possible to skip rendering objects that are not visible on the screen, which can improve performance.

Overall, an AABB is a simple and efficient way to represent the spatial extent of a 2D object in a game, and is widely used in 2D game development for collision detection and other spatial calculations.

The Rive GameKit tutorials have plenty of examples of AABBs in action. Take a look at the following resources:

* A simple example laying out multiple artboards in a grid.
* The Centaur Game uses AABBs to determine the game world size and to perform hit detection.
* To see more complex examples take a look at our game tutorials that make use of an AABB Tree to optimally perform hit detection and to only render game elements that are visible within the camera viewport: Joel and Goblin Slayer.

### **RenderPaint & RenderPath**

In this section you’ll learn how to create custom paint and path objects. These are best illustrated through an example. Take a look at the **Joel Game** video below and take note of the bullets and tree shadows:

<figure><img src="../.gitbook/assets/rive-gamekit-joel-demo.gif" alt=""><figcaption><p>Rive GameKit - Custom Paths and Paint</p></figcaption></figure>

#### **Custom Path and Paint**

The projectiles (bullets) are created at runtime using custom paths and paints. The code for the Projectile class looks like the following:

```dart
/// A single projectile fired by the hero.
class Projectile {
  final rive.RenderPath path = rive.Renderer.makePath();
 
  ... // redacted

  Projectile(this.position, this.direction)
      : end = position + direction * length {
    path.moveTo(position.x, position.y);
    path.lineTo(end.x, end.y);
  }

  bool advance(double seconds) {
    ... // redacted

    path.reset();
    path.moveTo(position.x, position.y);
    path.lineTo(end.x, end.y);

    return life > duration;
  }

  void dispose() {
    path.dispose();
  }

  ... // redacted
}
```

Take note of the **path** object that is created with `makePath`. This example is quite simple, it only creates a straight line. But you’re free to create complex path shapes using this API. All of the vector shapes created with Rive make use of these path operations underneath.

The **RenderPath** class is similar to [Flutter’s Path class](https://api.flutter.dev/flutter/dart-ui/Path-class.html).

Once you’ve created a path you can draw it:

```dart
renderer.drawPath(_projectilePath, _projectileStroke);
```

The `_projectileStroke` is the **RenderPaint** that determines what the rendered path will look like. The following is used in the Joel game to paint the projectiles:

```dart
final rive.RenderPaint _projectileStroke = rive.Renderer.makePaint()
  ..style = PaintingStyle.stroke
  ..blendMode = BlendMode.colorDodge
  ..color = const Color(0xFF53FD00)
  ..thickness = 15
  ..cap = StrokeCap.round;
```

The [PaintingStyle](https://api.flutter.dev/flutter/dart-ui/PaintingStyle.html), [BlendMode](https://api.flutter.dev/flutter/dart-ui/BlendMode.html) and [StrokeCap](https://api.flutter.dev/flutter/dart-ui/StrokeCap.html) classes are all Flutter code and you can see their documentation for additional information.

#### **Combining Paths**

The Joel game makes use of **RenderPath** and **RenderPaint** to efficiently draw the trees in the game. You’ll note that all of the trees are a continues shape and that they apply a blend to the artboards rendered below them.

This is done by combining all of the visible tree artboards together into a single path, using the `addToRenderPath` method, and then drawing that path, as one shape, with a particular paint.

```dart

final rive.RenderPaint _shadowPaint = rive.Renderer.makePaint()
  ..blendMode = BlendMode.multiply
  ..style = PaintingStyle.fill
  ..color = const Color(0x5524161B);

... // redacted

var shadowPath = rive.Renderer.makePath(true);

... // redacted

for (final artboard in shadowArtboards) {
  artboard.addToRenderPath(
    shadowPath,
    rive.Mat2D.fromTranslate(offset.x, offset.y), // Transform the path
  );
}

renderer.drawPath(shadowPath, _shadowPaint);
```

#### **Paint/Path Example - Draw a Star**

Let’s take a look at a complete example by drawing a star:

```dart
class GameDemoPainter extends rive.RenderTexturePainter {
  @override
  Color get background => Colors.white;

  /// A custom Path to paint stars.
  rive.RenderPath drawStar(Size size) {
    double degToRad(double deg) => deg * (pi / 180.0);

    const numberOfPoints = 5;
    final halfWidth = size.width / 2;
    final externalRadius = halfWidth;
    final internalRadius = halfWidth / 2.5;
    final degreesPerStep = degToRad(360 / numberOfPoints);
    final halfDegreesPerStep = degreesPerStep / 2;
    final path = rive.Renderer.makePath();
    final fullAngle = degToRad(360);
    path.moveTo(size.width, halfWidth);

    for (double step = 0; step < fullAngle; step += degreesPerStep) {
      path.lineTo(halfWidth + externalRadius * cos(step),
          halfWidth + externalRadius * sin(step));
      path.lineTo(halfWidth + internalRadius * cos(step + halfDegreesPerStep),
          halfWidth + internalRadius * sin(step + halfDegreesPerStep));
    }
    path.close();
    return path;
  }

  final _gradient = rive.RenderRadialGradient(
    rive.Vec2D.fromValues(500, 500),
    500,
    [Colors.red, Colors.blue],
    [0, 1],
  );
  late final _paint = rive.Renderer.makePaint()
    ..color = Colors.red
    ..gradient = _gradient;
  late final _path = drawStar(const Size(1000, 1000));

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    final renderer = rive.Renderer.make();

    renderer.drawPath(_path, _paint);

    return true;
  }
}
```

<figure><img src="../.gitbook/assets/Rive gamekit - draw a star.png" alt=""><figcaption><p>Rive GameKit - Custom Path and Paint example</p></figcaption></figure>
