---
description: Reading and modifying text at runtime
---

# Text

For more information on designing and animating Text, please refer to the [editor's text section](text.md).

Please ensure you're on the correct runtime version with support for Rive Text; see [Feature Support](feature-support.md).

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

#### Reading Text

To read a given text run text value at any given time, reference the `.getTextRunValue()` API on the Rive instance:

```typescript
public getTextRunValue(textRunName: string): string | undefined
```

Supply the text run name, and you'll have the Rive text run value returned, or `undefined` if the text run could not be queried.

#### Setting Text

To set a given text run value at any given time, reference the `.setTextRunValue()` API on the Rive instance:

```typescript
public setTextRunValue(textRunName: string, textRunValue: string): void
```

Supply the text run name and a second parameter, `textValue`, with a String value that you want to set the new text value for if the text run can be successfully queried on the active artboard.

#### Example Usage

```typescript
import rive from '@rive-app/canvas'

const r = new rive.Rive({
  src: "my-rive-file.riv"
  artboard: "my-artboard-name",
  autoplay: true,
  stateMachines: "State Machine 1",
  onLoad: () => {
    console.log("My design-time text is, ", r.getTextRunValue("MyRun"));
    r.setTextRunValue("MyRun", "New text value");
  },
})
```

### Low-level API usage

Get a reference to the Rive `Artboard`, find a text run by a given **name**, and get/update the text value property.

```javascript
import RiveCanvas from '@rive-app/canvas-advanced';


const bytes = await (
  await fetch(new Request('./my-rive-file.riv'))
).arrayBuffer();
const myRiveFile = await rive.load(new Uint8Array(bytes));

const artboard = myRiveFile.defaultArtboard();
const textRun = artboard.textRun("MyRun"); // Query by the text run name
console.log(`My design-time text is ${textRun.text}`);
textRun.text = "Hello JS Runtime!";

...
```
{% endtab %}

{% tab title="React" %}
#### Reading Text

To read a given text run text value at any given time, reference the `.getTextRunValue()` API on the `rive` instance returned from `useRive`:

```typescript
public getTextRunValue(textRunName: string): string | undefined
```

Supply the text run name, and you'll have the Rive text run value returned, or `undefined` if the text run could not be queried.

#### Setting Text

To set a given text run value at any given time, reference the `.setTextRunValue()` API on the `rive` instance returned from `useRive`:

```typescript
public setTextRunValue(textRunName: string, textRunValue: string): void
```

Supply the text run name and a second parameter, `textValue`, with a String value that you want to set the new text value for if the text run can be successfully queried on the active artboard.

#### Example Usage

```tsx
import { useRive } from '@rive-app/canvas';

const MyTextComponent = () => {
  const {rive, RiveComponent} = useRive({
    src: "my-rive-file.riv",
    artboard: "New Artboard",
    stateMachines: "State Machine 1",
    autoplay: true,
  });
  
  // Cannot query for the text run immediately, you have to wait until `rive`
  // has value and has instantiated before querying or setting text run values
  useEffect(() => {
    if (rive) {
      console.log("Rive text was initially: ", rive.getTextRunValue("MyRun"));
      rive.setTextRunValue("MyRun", "New Text!!");
      console.log("Rive text is now: ", rive.getTextRunValue("MyRun");
    }
  }, [rive]);
  
  return (
    <RiveComponent />
  );
};
```
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
#### Reading Text

To read a given text run text value at any given time, reference the `.getTextRunValue()` API on the `RiveViewModel`:

```swift
open func getTextRunValue(_ textRunName: String) -> String?
```

Supply the text run name and you'll have the Rive text run value returned, or `nil` if the text run could not be queried.

#### Setting Text

To set a given text run value at any given time, reference the `.setTextRunValue()` API on the `RiveViewModel`:

```swift
open func setTextRunValue(_ textRunName: String, textValue: String) throws
```

Supply the text run name and a second parameter, `textValue`, with a String value that you want to set the new text value for.&#x20;

{% hint style="warning" %}
If the supplied `textRunName` Rive text run cannot be queried on the active artboard, Rive will throw a `RiveError.textValueRunError` that you may need to catch and handle gracefully in your application.&#x20;
{% endhint %}

#### Example Usage

```swift
@State private var userInput: String = ""
@State private var rvm = RiveViewModel(fileName: "my-rive-file")

var body: some View {
    VStack(spacing: 20) {
        Text("Enter text:")
            .font(.headline)
        TextField("Enter text...", text: $userInput)
            .textFieldStyle(RoundedBorderTextFieldStyle())
            .padding()
            .onChange(of: userInput, perform: { newValue in
                if (!newValue.isEmpty) {
                    try! rvm.setTextRunValue("MyTextRunName", textValue: userInput)
                }
            })
        rvm.view()
    }
}
```
{% endtab %}

{% tab title="Android" %}
#### Reading Text via RiveAnimationView

To read a given text run text value at any given time, reference the `.getTextRunValue()` API on the `RiveAnimationView`:

```kotlin
fun getTextRunValue(textRunName: String): String? = try
```

Supply the text run name and you'll have the Rive text run value returned, or `null` if the text run could not be queried.

#### Setting Text via RiveAnimationView

To set a given text run value at any given time, reference the `.setTextRunValue()` API on the `RiveAnimationView`:

```kotlin
fun setTextRunValue(textRunName: String, textValue: String)
```

Supply the text run name and a second parameter, `textValue`, with a String value that you want to set the new text value for.&#x20;

{% hint style="warning" %}
If the supplied `textRunName` Rive text run cannot be queried on the active artboard, Rive will throw a `RiveException` that you may need to catch and handle gracefully in your application.&#x20;
{% endhint %}

#### Reference to Rive TextRun

You can also choose to query the active Artboard for the Rive text run reference and get/set a text property manually.

```kotlin
private val textRun by lazy(LazyThreadSafetyMode.NONE) {
    yourRiveAnimationView.artboardRenderer?.activeArtboard?.textRun("name")
}
```

#### Example Usage

```kotlin
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.util.Log
import android.widget.EditText
import androidx.appcompat.app.AppCompatActivity
import app.rive.runtime.kotlin.RiveAnimationView

class DynamicTextActivity : AppCompatActivity(), TextWatcher {
    private val animationView by lazy(LazyThreadSafetyMode.NONE) {
        findViewById<RiveAnimationView>(R.id.dynamic_text)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.dynamic_text)
        val editText = findViewById<EditText>(R.id.text_run_value)
        editText.addTextChangedListener(this)
    }

    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        // get the current value of the reference
        animationView.getTextRunValue("name")
    }

    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        // update the reference
        animationView.setTextRunValue("name", s.toString())
    }
}
```
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

