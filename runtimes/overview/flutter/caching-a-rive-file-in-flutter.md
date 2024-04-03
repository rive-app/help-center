---
description: Techniques and considerations to cache a Rive File in Flutter
---

# Caching a Rive File

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/caching-a-rive-file/docrLMDw15AJ).
{% endhint %}

Under most circumstances a `.riv` file should load quickly and managing the `RiveFile` yourself is not necessary. But if you intend to use the same `.riv` file in multiple parts of your application, or even on the same screen, it might be advantageous to load the file once and keep it in memory.

### Example Usage

The following is a basic example to illustrate how to preload a Rive file, and pass the data directly to the `RiveAnimation.direct()` constructor:

```dart
class PreloadRive extends StatefulWidget {
  const PreloadRive({super.key});

  @override
  State<PreloadRive> createState() => _PreloadRiveState();
}

class _PreloadRiveState extends State<PreloadRive> {
  RiveFile? _file; // You can maintain this reference to have a cached version

  @override
  void initState() {
    super.initState();
    preload();
  }

  Future<void> preload() async {
    rootBundle.load('assets/little_machine.riv').then(
      (data) async {
        // Load the RiveFile from the binary data.
        setState(() {
          _file = RiveFile.import(data);
        });
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return (_file == null)
        ? const SizedBox.shrink()
        : RiveAnimation.direct(_file!);
  }
}
```

### Other Considerations

#### Managing State

How the `RiveFile` is kept alive in state and shared to other widgets is up to you and your preferred state management solution. One approach can be to wait for the Rive file to load in `main`, or during the startup screen, and using the [Provider](https://pub.dev/packages/provider) package to expose the data to the whole application.

Or if the animation is only needed in a nested section of your application, then it might be preferable to delay loading the animation until necessary.

#### Memory

By managing the RiveFile yourself you can have finer control over the memory used in your application. This will be especially beneficial if the same Rive file is being used in multiple parts of your application, or used simultaneously in multiple `RiveAnimation` widgets.

You can make use of Flutter DevTools’ [memory tooling](https://docs.flutter.dev/tools/devtools/memory#memory-view-guide) for additional investigation if needed or desired.

#### Network Assets

The easiest way to load a Rive animation over the Internet is by using `RiveAnimation.network(url)`. However, similar considerations apply to a network asset regarding memory and sharing a Rive file across multiple widgets/pages.

The following can be used to load a Rive file over the network:

```dart
final riveFile = await RiveFile.network('YOUR:URL');
```

The reference can then be kept alive in memory and the `riveFile` can be passed to `RiveAnimation.direct`.
