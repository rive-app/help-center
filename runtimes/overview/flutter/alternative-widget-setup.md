# Alternative Widget Setup

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/alternative-widget-setup/docNlDD0H0rp).
{% endhint %}

The easiest way to integrate Rive animations and state machines into your application is via the `RiveAnimation` widget mentioned throughout the runtime help docs. However, there are cases where you may want to set up animation or state machine controllers associated with the Rive widget before the Artboard is rendered. Check out below for other ways to integrate Rive into your Flutter applications:\


## Alternative Methods

### File loading

If you want to load in the `.riv` file from the `rootBundle`, you'll need to import the data yourself. The main pattern here is:

1. Load in the bytes of the `.riv` file
2. Use the `RiveFile` class to parse the data and get a reference to the file
3. Create a reference to the Artboard you want to display, from that file
4. (Optional) Associate controllers to an Artboard (i.e `StateMachineController`)
5. Render the `Rive` widget with the artboard reference

#### 1. Load in the bytes from .riv

```dart
rootBundle.load('assets/new_file.riv').then(
  (data) async {
    ...
  }
);
```

#### 2. Use RiveFile to parse bytes

```dart
(data) async {
    // Load the RiveFile from the binary data.
    final file = RiveFile.import(data);
},
```

#### 3. Create an Artboard reference

```dart
// The artboard is the root of the animation and gets drawn in the
// Rive widget
final riveArtboard = file.mainArtboard;

```

#### 4. Associate controllers to an Artboard

If you're looking to just play a specific animation:

```dart
var controller =
  StateMachineController.fromArtboard(riveArtboard, 'SomeStateMachineName');
if (controller != null) {
  riveArtboard.addController(controller);
}
```

If you're looking to play a state machine:

```dart
var controller =
  StateMachineController.fromArtboard(riveArtboard, 'SomeStateMachineName');
if (controller != null) {
  riveArtboard.addController(controller);
}
```

#### 5. Render the Rive widget

```dart
Widget build(BuildContext context) {
  return Center(
    child: riveArtboard == null
      ? const SizedBox()
      : SizedBox(
          width: 250,
          height: 250,
          child: Rive(
            artboard: riveArtboard!.instance(),
          ),
        )
  );
}
```

{% hint style="info" %}
**Important**: Note above that when connecting the Artboard to the `Rive` widget, you will need to call `.instance()` on it. This will allow any nested artboards to render within the canvas space appropriately
{% endhint %}

#### Complete Example

Altogether, this pattern might look like the following snippet below:

```dart
class _ExampleStateMachineState extends State<ExampleStateMachine> {
  Artboard? _riveArtboard;

  @override
  void initState() {
    super.initState();

    rootBundle.load('assets/rocket.riv').then(
      (data) async {
        final file = RiveFile.import(data);

        final artboard = file.mainArtboard;
        var controller =
            StateMachineController.fromArtboard(artboard, 'Button');
        if (controller != null) {
          artboard.addController(controller);
        }
        setState(() => _riveArtboard = artboard);
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      width: 250,
      height: 250,
      child: Rive(
        artboard: _riveArtboard!.instance(),
      ),
    );
  }
}
```

