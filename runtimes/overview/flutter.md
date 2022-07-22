---
description: Flutter runtime for Rive
---

# Flutter

## Overview

This guide documents how to get started using the Flutter runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-flutter).\
\
This library contains an API for Flutter apps to easily integrate their Rive assets.

## Getting Started

Follow the steps below for a quick start on integrating Rive into your Flutter apps.

### 1. Add the Rive package dependency

Check out Rive's [pub.dev](https://pub.dev/packages/rive) page to get the latest version.

{% code title="pubspec.yaml" %}
```yaml
dependencies:
  rive: 0.9.0
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

Make sure you add the Rive file to your asset bundle and reference it in `pub.dev`

```dart
RiveAnimation.asset(
  'assets/vehicles.riv',
)
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

## Resources

Github: [https://github.com/rive-app/rive-flutter](https://github.com/rive-app/rive-flutter)\
Examples: [https://github.com/rive-app/rive-flutter/tree/master/example/lib](https://github.com/rive-app/rive-flutter/tree/master/example/lib)\
API Docs: [https://pub.dev/documentation/rive/latest/](https://pub.dev/documentation/rive/latest/)
