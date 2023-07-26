---
description: Reading and modifying text at runtime
---

# Text

For more information designing and animating Text, please refer to the [editor's text section](text.md).

Please ensure you're on the correct runtime version with support for Rive Text, see [Feature Support](feature-support.md).

### Read/Update Text Runs at Runtime

If you intend to update a **text run** at runtime it's important to manually enter a _unique name_ for the run in the editor:

<figure><img src="../.gitbook/assets/CleanShot 2023-07-26 at 14.51.22@2x.png" alt=""><figcaption><p>Manually enter a <strong>name</strong> for a <strong>text run</strong></p></figcaption></figure>

This makes it possible for the run to be "discoverable" at runtime by its name.

{% hint style="danger" %}
If the name is not set manually in the editor the name will not be part of the exported _.riv_ (to reduce file size).
{% endhint %}

{% tabs %}
{% tab title="Web" %}
### High-level API usage

:eyes: Coming soon

### Low-level API usage

Get a reference to the Rive `Artboard`, find a text run by a given **name**, and get/update the text value property.

```javascript
const artboard = myRiveFile.defaultArtboard();
const textRun = artboard.textRun("MyRun"); // Query by the text run name
console.log(`My design-time text is ${textRun.text}`);
textRun.text = "Hello JS Runtime!";
```
{% endtab %}

{% tab title="React" %}
:eyes: coming soon
{% endtab %}

{% tab title="React Native" %}
:eyes: coming soon
{% endtab %}

{% tab title="Flutter" %}
Find the `TextValueRun` component with a given **name** on a Rive `Artboard` and update the text value.

To make this more convenient you can create an extension.

```dart
extension _TextExtension on Artboard {
  TextValueRun? textRun(String name) => component<TextValueRun>(name);
}
```

Get a reference to the `Artboard`, find the text run named _"MyRun"_, and update the text value:

```dart
RiveAnimation.asset(
  'assets/hello_world_text.riv',
  animations: const ['Timeline 1'],
  onInit: (artboard) {
    final textRun = artboard.textRun('MyRun')!; // find text run named "MyRun"
    print('Run text used to be ${textRun.text}');
    textRun.text = 'Hi Flutter Runtime!';
  },
)
```

Or get a reference to the artboard by calling: `riveFile.mainArtboard`, see [Alternative Widget Setup](overview/flutter/alternative-widget-setup.md).
{% endtab %}

{% tab title="iOS/macOS" %}
:eyes: coming soon
{% endtab %}

{% tab title="Android" %}
:eyes: coming soon
{% endtab %}
{% endtabs %}

### Semantics for Accessibility

As Rive Text does not make use of the underlying platform text APIs, additional steps need to be taken to ensure it can be read by screen readers.

The following code snippets provide guidance on how to add semantic labels to your Rive animations. Please see the respective platform/SDKs developer documentation for additional information regarding accessibility concerns.

{% tabs %}
{% tab title="Flutter" %}
See Flutter's [documentation on Accessibility](https://docs.flutter.dev/accessibility-and-localization/accessibility) for more information. In this example you'll use the [Semantics widget](https://api.flutter.dev/flutter/widgets/Semantics-class.html) to provide a label that reflects the current value of the Rive Text component.

````dart
...

String semanticLabel = '';

...

Semantics(
  label: semanticLabel,
  child: RiveAnimation.asset(
    'assets/hello_world_text.riv',
    animations: const ['Timeline 1'],
    onInit: (artboard) {
      final run = artboard.component<TextValueRun>("MyRun");
      setState(() {
        // Calling setState is needed to ensure the Semantics widget
        // rebuilds with the new value.
        semanticLabel = run?.text ?? "";
      });
    },
  ),
),
```
````

Or you can read the `.riv` file yourself and instance the artboard, see [Alternative Widget Setup](overview/flutter/alternative-widget-setup.md).
{% endtab %}
{% endtabs %}

**Additional Resources:**

* [Accesible web animations: ARIA live regions](https://rive.app/blog/accesible-web-animations-aria-live-regions)

