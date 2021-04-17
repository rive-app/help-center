# Add a runtime

{% tabs %}
{% tab title="Web" %}
### 1. Import the runtime package

To use the library, run `npm install rive-canvas` and include it within the `<body>` of your HTML file, adapting the path to suit your project's setup.

```markup
<script src="/node_modules/rive-canvas/rive.js"></script> 
```

### 2. Instantiate the Rive engine

Then instantiate the Rive engine and load the WASM file\(s\) within your JavaScript.

```javascript
Rive({
  locateFile: (file) => '/node_modules/rive-canvas/' + file,
}).then((rive) => {
  // Rive's ready to rock 'n roll
  // Let's load up a Rive animation file...
});
```

### CDN alternative

As with all npm packages, you can use the freely available CDN via unpkg.com.

```markup
<script src="https://unpkg.com/rive-canvas@0.6.5/rive.js"></script>
```

```javascript
Rive({
  locateFile: (file) => 'https://unpkg.com/rive-canvas@0.6.5/' + file,
}).then((rive) => {
  // Rive's ready to rock 'n roll
  // Let's load a Rive animation file...
});
```
{% endtab %}

{% tab title="Flutter" %}
### 1. Add the package

To get started, add the Rive package and your Rive file to your `pubspec.yaml` file.

```dart
dependencies:
  rive: ^0.6.2

flutter:
  assets:
    - assets/truck.riv
```

### 2. Install

Install the package from the command line.

```bash
$ flutter pub get
```

Alternatively, your editor might support `flutter pub get`. Check the docs for your editor to learn more.

### 3. Import the package

Now in your Dart code, import the library.

```dart
import 'package:rive/rive.dart';
```
{% endtab %}

{% tab title="iOS" %}
Stay tuned for information on our upcoming iOS runtime!
{% endtab %}

{% tab title="Android" %}
Stay tuned for information on our upcoming Android runtime!
{% endtab %}
{% endtabs %}



