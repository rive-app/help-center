---
description: Advancing and Performance Considerations
---

# Advancing State Machines

## Advancing State Machines

In the render loop, as we’ve seen in previous sections, the essential goal of each frame is to:

1. Make the `Renderer`
2. Advance relevant `StateMachine` instances by an elapsed amount of seconds since the last frame paint
3. Draw relevant `Artboard` instances with the renderer

In many of our examples previously, we’ve called `.advance(elapsedSeconds)` on a single `StateMachine` instance, which moved the animations forward over time. With the Rive GameKit, there are multiple ways to advance state machines in a performant way, as well as draw the artboard.

* `StateMachine` class
  *   `void advance(double elapsedSeconds)`

      * **Description:** Advances the timeline animations on a singular `StateMachine` instance.
      * **When to use:** If you have just a few `StateMachine` instances in your scene, it should be fine to simply call this and separately render your `Artboard`
      * **Example:**

      ```dart
      bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
        // Advance a single state machine by elapsedSeconds
        stateMachine.advance(elapsedSeconds);
      }
      ```
* `Rive` class
  *   `static void batchAdvance(Iterable<StateMachine> stateMachines, double elapsedSeconds)`

      * **Description:** Advances the timeline animations on a list of `StateMachine` instances by splitting the work up among multiple threads in a worker thread pool
      * **When to use:** If you have a large amount of `StateMachine` instances in your scene that you want to advance all at the same time, this can be a more performant solution. If the state machine’s associated `Artboard` does not need to be drawn with the `Renderer` but still needs to be advanced, use this method. If you do need to render the associated `Artboard`, see the `batchAdvanceAndRender` method below.
      * **Example:**

      ```dart
      final Set<rive.Artboard> allZombieArtboards = {};
      final Set<rive.StateMachine> allZombieStateMachines = {};

      void populateZombies() {
        for (int i = 0; i < 1000; i++) {
          var artboard = riveFile.artboard("Zombie Man")!;
          allZombieArtboards.add(artboard);
          allZombieStateMachines.add(artboard.stateMachine("Motion")!);
        }
      }

      bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
        // Advance a list of state machines by elapsedSeconds
        rive.Rive.batchAdvance(allZombieStateMachines, elapsedSeconds);

        // Make a renderer.
        final renderer = rive.Renderer.make();

        // Only draw half the zombies
        for (int i = 500; i < 1000; i++) {
          var artboard = allZombieArtboards.elementAt(i);
          renderer.save();
          renderer.transform(
            rive.Mat2D.fromTranslate(size.width / 2, size.height / 2)
          );
          artboard.draw(renderer);
          renderer.restore();
        }
      }
      ```
  *   `static void batchAdvanceAndRender(Iterable<StateMachine> stateMachines, double elapsedSeconds, Renderer renderer)`

      * **Description:** The same as `batchAdvance()` but also renders each of the state machine’s associated `Artboard` instances with the passed-in `Renderer` instance. The renderer also makes a transformation before drawing using the `Artboard`'s `renderTransform` property, which is a `Mat2D`. You can set this property at any time.
      * **When to use:** Same as for using `batchAdvance()` but also for performantly drawing the `Artboard` with the `Renderer`.
      * **Example:**

      ```dart
      final Set<rive.StateMachine> allZombieStateMachines = {};

      // Set transformation to translate to a random location on the screen
      rive.Mat2D setRandomTranslation(rive.Artboard artboard) { . . . }

      void populateZombies() {
        for (int i = 0; i < 1000; i++) {
          var artboard = riveFile.artboard("Zombie Man")!;
          artboard.renderTransform = setRandomTranslation(artboard);
          allZombieStateMachines.add(artboard.stateMachine("Motion")!);
        }
      }

      bool paint(rive.RenderTexture texture, Size size, double elapsedSeconds) {
        // Make a renderer.
        final renderer = rive.Renderer.make();  

        // Advance a list of state machines and render their artboards
        rive.Rive.batchAdvanceAndRender(
          allZombieStateMachines,
          elapsedSeconds,
          renderer
        );
      }
      ```

## Other Performance Considerations

In addition to the few `advance()` functions above, there are other techniques that you should be considerate of at design time to keep your game performant.

* Don't use the **even-odd** fill rule. If you need to cut a hole in a shape, make sure to wind the path the opposite direction
* Don't enable the artboard clip option
* Don't nest an artboard inside a clip
* While clipping should be performant in your games when drawing, try to use it as a last resort if you can achieve similar effects another way
