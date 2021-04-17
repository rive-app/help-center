# Load a file

With the runtime added to your project, it's time to load an animation file!

{% tabs %}
{% tab title="Web" %}
### 1. Load a Rive file

With the Rive engine instantiated, we can load in our Rive \(.riv\) animation file. Here, we're loading our `loader.riv` file locally.

```javascript
const req = new Request('/rive/loader.riv');
fetch(req).then((res) => {
  return res.arrayBuffer();
}).then((buf) => {
  // we've got the raw bytes of the animation,
  // let's load them into a Rive file
  const file = rive.load(new Uint8Array(buf));
  // ...
})
```

### 2. Grab an artboard

In Rive, animations are tied to artboards. A Rive file can have multiple artboards, but if you're just working with one you can grab it via `defaultArtboard`

```javascript
const artboard = file.defaultArtboard();
```

Alternatively, you can reference a specific artboard.

```javascript
const artboard = file.artboard('my_artboard');
```

### 3. Reference an animation

Artboards hold references to animations. Reference a particular animation via its name set within the editor.

```javascript
const myAnim = artboard.animation('animation_name');
```

Its good practice to create an instance of your animation.

```javascript
const myAnimInstance = new rive.LinearAnimationInstance(myAnim);
```

An animation instance is useful if you want Rive to manage state, which manages the animation's time, direction, and looping.

We’re now ready to display and run the animation.
{% endtab %}

{% tab title="Flutter" %}
### 1. Define an Artboard and Rive filename

Start by creating a new `StatefulWidget` and defining an `Artboard` object. In this instance, we've also declared the Rive filename as a variable.

```dart
class MyRiveAnimation extends StatefulWidget {
  @override
  _MyRiveAnimationState createState() => _MyRiveAnimationState();
}

class _MyRiveAnimationState extends State<MyRiveAnimation> {
  final riveFileName = 'assets/truck.riv';
  Artboard _artboard;
}
```

### 2. Create a load method

Define a method to load your animation into a `RiveFile`. Once imported we can access any of the file’s artboards and play the animations they contain. In this example, there’s one artboard, so we can fetch that with the `file.mainArtboard` property.

```dart
// loads a Rive file
void _loadRiveFile() async {
  final bytes = await rootBundle.load(riveFileName);
  final file = RiveFile();

  if (file.import(bytes)) {
    // Select an animation by its name
    setState(() => _artboard = file.mainArtboard
      ..addController(
        SimpleAnimation('idle'),
      )
    );
  }
}
```

Note that we load raw bytes. This gives you flexibility in where you source your Rive data: as assets, over a network, from a local database, or by other means.

### 3. Load the file

As we're using a `StatefulWidget`, we can now load the file directly inside the widget on initialization.

```dart
@override
void initState() {
  _loadRiveFile();
  super.initState();
}
```

Check out the [example](example.md) page to see these steps combined into a single file.
{% endtab %}

{% tab title="iOS" %}
Stay tuned for information on our upcoming iOS runtime!
{% endtab %}

{% tab title="Android" %}
Stay tuned for information on our upcoming Android runtime!
{% endtab %}
{% endtabs %}



