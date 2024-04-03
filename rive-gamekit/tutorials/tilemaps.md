---
description: Some techniques to tile your Rive games
---

# Tilemaps

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/tilemaps/docIji41KXIU).
{% endhint %}

In this tutorial, we’ll explore a simple 2D-level concept and demonstrate a few approaches to tile your game. In principle, it works the same as a traditional sprite tilemap system.

> 💡 With the Rive Renderer you have the flexibility to make every scene component a vector element. Meaning you can make your world fully interactive and animated.

The picture below demonstrates a sample tilemap with a number of different Rive artboards. The small artboards represent a single tile, while the medium artboards are composited from the small ones using [Nested Artboards](https://help.rive.app/editor/fundamentals/nested-artboards). The same applies to the final large artboard that is made from the four medium-sized ones.

Any change applied to the nested artboards will reflect in the parent artboards. This is extremely beneficial at design time as it’ll allow you to quickly see what the larger game scene looks like.

<figure><img src="../../.gitbook/assets/CleanShot 2023-04-03 at 17.06.17@2x.png" alt=""><figcaption></figcaption></figure>

Let’s pretend the biggest artboard in the picture above is the final desired world size. It has a size of **4096** x **4096** and is comprised of **64** smaller artboards (**8** x **8**).

The player, size **300** x **300**, is allowed to move freely in the world. If you’re intending to create a mobile game with this world size, it’ll be a waste of rendering resources to draw the whole world at once (the largest artboard) - as most of the scene won’t be in the camera view.

Take a look at the following code that randomly selects one of the smaller tiles and lays them out on the scene, in a 8 x 8 grid:

```dart
import 'dart:math';
import 'package:flutter/material.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;

class SceneObject {
  final rive.Artboard artboard;
  final rive.AABB bounds;
  final rive.Vec2D position;

  SceneObject(
    this.artboard, {
    required this.bounds,
    required this.position,
  });
}

class TilemapPainterDemo extends rive.RenderTexturePainter {
  final rive.File riveFile;
  final List<rive.Artboard> artboards = [];
  final List<SceneObject> sceneObjects = [];
  late rive.AABB tileSize;
  late Size worldSize;

  final random = Random();

  TilemapPainterDemo(this.riveFile) {
    // You can reuse the artboard reference for each tile. But if your tile
    // as a state machine that needs to be operated independently, you'll
    // need to reinitialize the artboard to get a unique state machine.
    artboards.add(riveFile.artboard('tile1')!);
    artboards.add(riveFile.artboard('tile2')!);
    artboards.add(riveFile.artboard('tile3')!);
    artboards.add(riveFile.artboard('tile4')!);
    artboards.add(riveFile.artboard('tile5')!);

    const rows = 8;
    const columns = 8;

    // 5 rows
    for (var i = 0; i < rows; i++) {
      // 5 columns
      for (var j = 0; j < columns; j++) {
        final artboard = artboards[random.nextInt(artboards.length)];
        sceneObjects.add(
          SceneObject(
            artboard,
            bounds: artboard.bounds,
            position: rive.Vec2D.fromValues(
              j * artboard.bounds.width,
              i * artboard.bounds.height,
            ),
          ),
        );
      }
    }

    tileSize = artboards.first.bounds; // assuming all tiles are the same size
    worldSize = Size(tileSize.width * columns, tileSize.height * rows);
  }

  @override
  Color get background => const Color(0xffffffff);

	@override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    final renderer = rive.Renderer.make();

    _drawTiles(renderer);

    return true;
  }

  void _drawTiles(rive.Renderer renderer) {
    for (var sceneObject in sceneObjects) {
      renderer.save();
      renderer.translate(sceneObject.position.x, sceneObject.position.y);
      sceneObject.artboard.draw(renderer);
      renderer.restore();
    }
  }

  @override
  void dispose() {
    for (var artboard in artboards) {
      artboard.dispose();
    }
    riveFile.dispose();
    super.dispose();
  }
}
```

The randomly generated tilemap will look something like this:

<figure><img src="../../.gitbook/assets/CleanShot 2023-04-04 at 06.44.07@2x.png" alt=""><figcaption><p>Tilemap - 9 tiles visible</p></figcaption></figure>

About nine tiles are visible in the screenshot above, but 64 are drawn.

To illustrate this, let’s simulate “zooming out the camera” by applying a scale transform:

```dart
@override
bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
  final renderer = rive.Renderer.make();

  renderer.save();
  // Apply a transform to make the whole game scene smaller.
  renderer.transform(rive.Mat2D.fromScale(0.2, 0.2));

  _drawTiles(renderer);

  renderer.restore();
  return true;
}
```

Now all 64 tiles are visible.

<figure><img src="../../.gitbook/assets/CleanShot 2023-04-04 at 07.17.23@2x.png" alt=""><figcaption><p>Tilemap - all 64 tiles visible</p></figcaption></figure>

The size you choose for your artboards and when to render them will be dependent on your particular game and the screen sizes you target.

For example, in the code given above, you could simply not draw the tile if its position is bigger than the provided `size` in the `paint` method. However, if you add a moving player that the camera follows, this calculation becomes more complex, even more so if you’re applying other types of transformations to the renderer, such as the zoom above.

### AABB Tree

There are a variety of efficient techniques to cull objects that are not visible in the camera; one such technique is an AABB Tree. See our documentation if you’re unfamiliar with the concept and for additional examples.

The following continues with the code above and demonstrates how to use an AABB Tree to ensure only the visible tiles in the camera view are drawn.

Here is the complete code. You can read the comments and see if you can understand what is going on, after we'll walk through it step by step.

```dart
import 'dart:math';

import 'package:flutter/material.dart';
import 'package:rive_gamekit/rive_gamekit.dart' as rive;
import 'package:zombie_skins_gamekit/aabb_tree.dart';

class SceneObject {
  final rive.Artboard artboard;
  final rive.AABB bounds;
  final rive.Vec2D position;
  int proxyId = -1;

  SceneObject(
    this.artboard, {
    required this.bounds,
    required this.position,
  });
}

class TilemapPainterDemo extends rive.RenderTexturePainter {
  final rive.File riveFile;
  final List<rive.Artboard> artboards = [];
  final List<SceneObject> sceneObjects = [];
  late rive.AABB tileSize;
  late Size worldSize;

  final random = Random();

  // The AABB tree is used to efficiently query for objects that intersect with
  // a given AABB (the camera's view in this example).
  late AABBTree<SceneObject> _tree;
  // The view transform is used to transform the world into the camera's view.
  final rive.Mat2D _viewTransform = rive.Mat2D();
  // The inverse view transform is used to transform the camera's view into the
  // world.
  final rive.Mat2D _inverseViewTransform = rive.Mat2D();

  TilemapPainterDemo(this.riveFile) {
    // STEP 1: Create an AABBTree
    _tree = AABBTree();

    artboards.add(riveFile.artboard('tile1')!);
    artboards.add(riveFile.artboard('tile2')!);
    artboards.add(riveFile.artboard('tile3')!);
    artboards.add(riveFile.artboard('tile4')!);
    artboards.add(riveFile.artboard('tile5')!);

    const rows = 8;
    const columns = 8;

    for (var i = 0; i < rows; i++) {
      for (var j = 0; j < columns; j++) {
        final artboard = artboards[random.nextInt(artboards.length)];
        // STEP 2: Create an object to store information about the tile for
        // easy reference.
        final sceneObject = SceneObject(
          artboard,
          bounds: artboard.bounds,
          position: rive.Vec2D.fromValues(
            j * artboard.bounds.width,
            i * artboard.bounds.height,
          ),
        );

        // STEP 3: Add the object to the tree
        // The proxy ID is used to reference the object later if it moves or is
        // deleted.
        final proxyID = _tree.createProxy(
            artboard.bounds.translate(sceneObject.position), sceneObject);
        sceneObject.proxyId = proxyID;

        sceneObjects.add(sceneObject);
      }
    }

    tileSize = artboards.first.bounds; // assuming all tiles are the same size
    worldSize = Size(tileSize.width * columns, tileSize.height * rows);
  }

  @override
  Color get background => const Color(0xffffffff);

  @override
  bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
    final renderer = rive.Renderer.make();

    // Modify the camera's zoom and translation to simulate a camera moving
    // around the world.
    late double cameraZoom = 1;
    late rive.Vec2D cameraTranslation = rive.Vec2D.fromValues(0, 0);

    // STEP 4: Create a view transform (Mat2D) that represents the visible area
    // of the world - depending on the camera zoom and translation.
    // The camera's view is a rectangle that is centered on the camera's
    // translation and is scaled by the camera's zoom.
    _viewTransform[0] = cameraZoom;
    _viewTransform[1] = 0;
    _viewTransform[2] = 0;
    _viewTransform[3] = cameraZoom;
    _viewTransform[4] = -cameraTranslation.x * cameraZoom;
    _viewTransform[5] = -cameraTranslation.y * cameraZoom;

    // STEP 5: Create an AABB that represents the camera's view according to 
    // the window size. This is used to determine which tiles are visible on
    // screen.
    rive.Mat2D.invert(_inverseViewTransform, _viewTransform);
    final cameraAABB = rive.AABB.fromPoints(
      [
        rive.Vec2D.fromValues(0, 0),
        rive.Vec2D.fromValues(size.width, 0),
        rive.Vec2D.fromValues(size.width, size.height),
        rive.Vec2D.fromValues(0, size.height),
      ],
      transform: _inverseViewTransform,
    );

    renderer.save();
    renderer.transform(_viewTransform);

    // STEP 6: Query the AABB Tree to find all the objects who's bounds intersect
    // with the cameraAABB. These will be the tiles that are visible on screen.
    // Finally, draw the tiles.
    final visibleSceneObjects = <SceneObject>[];
    _tree.query(cameraAABB, (id, object) {
      visibleSceneObjects.add(object);
      return true;
    });
    _drawTiles(renderer, visibleSceneObjects);

    renderer.restore();
    return true;
  }

  var numberOfVisibleTiles = 0;
  void _drawTiles(rive.Renderer renderer, List<SceneObject> objects) {
    // This is just to show how many tiles are visible for demo purposes
    if (numberOfVisibleTiles != objects.length) {
      numberOfVisibleTiles = objects.length;
      print('number of visible tiles: ${objects.length}');
    }

    for (var sceneObject in objects) {
      renderer.save();
      renderer.translate(sceneObject.position.x, sceneObject.position.y);
      sceneObject.artboard.draw(renderer);
      renderer.restore();
    }
  }

  @override
  void dispose() {
    for (var artboard in artboards) {
      artboard.dispose();
    }
    riveFile.dispose();
    super.dispose();
  }
}
```

The important bits can be broken down into seven steps:

1. Create an AABBTree, with a generic **SceneObject** type. This is used to store information about all of the scene objects and to efficiently query for objects that are within a provided AABB space.
2. Create all the tile objects, which store information about the tile, artboard and position for easy reference. This could be any object that stores the needed information to render your scene.
3. Add all the objects to the AABBTree using the `createProxy` method. This takes in an AABB and a SceneObject.
4. Create a view transform (Mat2D) that represents the visible area of the world (camera) - depending on the camera zoom and translation.
5. Create an AABB that represents the camera's view according to the window size. This is used to determine which tiles are visible on screen.
6. Query the AABB Tree to find all the objects who's bounds intersect with the `cameraAABB`. These will be the tiles that are visible on screen. Once you have these, you can draw them to the renderer.

Running the above code you'll see a print that indicates how many tiles are currently visible on screen. This will print a different result if you resize the window, zoom in or out, or moving the camera's translation around. Try updating the `cameraZoom` and `cameraTranslation` and performing a hot reload. Or add [keyboard and mouse interactions](adding-keyboard-and-mouse-interaction.md) to move the camera.
