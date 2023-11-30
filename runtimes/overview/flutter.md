---
description: Flutter runtime for Rive
---

# Flutter

## Overview

This guide documents how to get started using the Flutter runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-flutter).\
\
This library contains an API for Flutter apps to easily integrate their Rive assets.

## Quick Start

See our [quick start example](https://zapp.run/edit/rive-car-wash-zf160614f170?entry=lib/main.dart\&file=lib/main.dart) that shows how to play a Rive animation in Flutter.

## Getting Started

Follow the steps below for a quick start on integrating Rive into your Flutter apps.

### 1. Add the Rive package dependency

Check out Rive's [pub.dev](https://pub.dev/packages/rive) page to get the latest version.

{% code title="pubspec.yaml" %}
```yaml
dependencies:
  rive: 0.11.11
```
{% endcode %}

### 2. Import the Rive package

Import the Rive runtime library in the file you're looking to integrate Rive animations into.

```dart
import 'package:rive/rive.dart';
```

### 3. Add a RiveAnimation widget

There are a few different methods to integrating Rive Animations into your apps easily:

#### Via URL

```dart
RiveAnimation.network(
  'https://cdn.rive.app/animations/vehicles.riv',
)
```

#### Via Asset Bundle

Make sure you add the Rive file to your asset bundle and reference it in `pubspec.yaml`

```dart
RiveAnimation.asset(
  'assets/vehicles.riv',
)
```

In `pubspec.yaml`:

```yaml
...

# To add assets to your application, add an assets section, like this:
assets:
    - assets/vehicles.riv
```

#### Via Relative Path

```dart
RiveAnimation.file('path/to/local/file.riv')
```

#### Via Direct

If you use the same Rive file multiple times in your application, you may want to create a single `RiveFile` instance for that `.riv`, and feed it directly to `RiveAnimation` so that the Rive file is only loaded once.

```dart
final riveFile = await RiveFile.asset('assets/truck.riv');

RiveAnimation.direct(riveFile)
```

### Complete example

{% code title="main.dart" %}
```dart
import 'package:flutter/material.dart';
import 'package:rive/rive.dart';

void main() => runApp(MaterialApp(
      home: MyRiveAnimation(),
    ));

class MyRiveAnimation extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: Center(
        child: RiveAnimation.network(
          'https://cdn.rive.app/animations/vehicles.riv',
          fit: BoxFit.cover,
        ),
      ),
    );
  }
}
```
{% endcode %}

See subsequent runtime pages to learn how to control animation playback, state machines, and more.

{% hint style="info" %}
Using Impeller and noticing visual bugs in your app? See [our notes on rendering](../renderer/#note-on-rendering-in-flutter) with Impeller for some triaging tips
{% endhint %}

## Resources

Github: [https://github.com/rive-app/rive-flutter](https://github.com/rive-app/rive-flutter)\
API Docs: [https://pub.dev/documentation/rive/latest/](https://pub.dev/documentation/rive/latest/)\
Examples:

* Demo app: [https://github.com/rive-app/rive-flutter/tree/master/example/lib](https://github.com/rive-app/rive-flutter/tree/master/example/lib)
* Find the Dog (game): [https://github.com/rive-app/find-the-dog](https://github.com/rive-app/find-the-dog)
