---
description: Controlling the layout of your Rive animation
---

# Layout

Rive provides a number of ways to control how your animations are laid out in the canvas or view used to display them. Rive lets you control the fit, alignment, and offset of rendered content.

## The `Layout` object

Most runtimes have a `Layout` object that enables you to configure `Fit` and `Alignment`. You typically provide layout data when instantiating a Rive object, but you can also update these values at any time.

{% tabs %}
{% tab title="Web" %}
See values and descriptions in the sections below for `Fit` and `Alignment`.



The runtime has named exports for `Layout`, `Fit`, and `Alignment`.

```markup
<div>
    <canvas id="canvas" width="800" height="600"></canvas>
</div>
<script src="https://unpkg.com/@rive-app/canvas@1.0.60"></script>
<script>

    // Fill the canvas, cropping Rive if necessary
    let layout = new rive.Layout({
        fit: rive.Fit.Cover,
    });

    // Fit to the width and align to the top of the canvas
    layout = new rive.Layout({
        fit: rive.Fit.FitWidth,
        alignment: rive.Alignment.TopCenter,
    });

    // Constrain the Rive content to (minX, minY), (maxX, maxY) in the canvas
    layout = new rive.Layout({
        fit: rive.Fit.Contain,
        minX: 50,
        minY: 50,
        maxX: 100,
        maxY: 100,
    });

    const r = new rive.Rive({
        src: 'https://cdn.rive.app/animations/vehicles.riv',
        canvas: document.getElementById('canvas'),
        layout: layout,
        autoplay: true
    });

    // Update the layout
    r.layout = new rive.Layout({ fit: rive.Fit.Fill });
</script>
```
{% endtab %}

{% tab title="React" %}
See values and descriptions in the sections below for `Fit` and `Alignment`.\
\
The runtime has named exports for `Layout`, `Fit`, and `Alignment`.

```javascript
import Rive, { Layout, Fit, Alignment } from '@rive-app/react-canvas';

export const Simple = () => (
  <Rive
    src="https://cdn.rive.app/animations/vehicles.riv"
    layout={new Layout({ fit: Fit.Contain, alignment: Alignment.TopCenter })}
  />
);
```

With the `useRive` hook:

```javascript
import { useRive, Layout, Fit, Alignment } from '@rive-app/react-canvas';

export default function Example() {
  const { RiveComponent } = useRive({
    src: 'my-file.riv',
    artboard: 'my-artboard',
    animations: 'my-animation',
    layout: new Layout({
      fit: Fit.Cover,
      alignment: Alignment.TopCenter,
    }),
    autoplay: true,
  });

  return <RiveComponent />;
}
```
{% endtab %}

{% tab title="Angular" %}
Angular runtime exposes the layout's options through inputs on the canvas.

```markup
<canvas riv="vehicles" width="500" height="500" fit="fitWidth" alignment="topCenter">
  <riv-animation name="idle" play></riv-animation>
</canvas>
```
{% endtab %}

{% tab title="React Native" %}
Set layout attributes for `Fit` and `Alignment` on the `Rive` component directly.

```jsx
import Rive, { Alignment, Fit } from 'rive-react-native';

export default function Simple() {
  return (
    <ScrollView>
      <Rive
        fit={Fit.Cover}
        alignment={Alignment.TopCenter}
        resourceName="truck_v7"
      />
    </ScrollView>
  );
};
```
{% endtab %}

{% tab title="Flutter" %}
Rive's Flutter runtime doesn't have a `Layout` object; instead fit and alignment can be passed in through the `RiveAnimation` and `Rive` constructors. The fit and alignment options behave like their counterparts in Flutter (see [`BoxFit`](https://api.flutter.dev/flutter/painting/BoxFit-class.html) and [`Size`](https://api.flutter.dev/flutter/dart-ui/Size-class.html) as examples).

Bounds options are absent; Flutter's layout engine and options are the preferred way to handle positioning Rive content.

```dart
// Fill the canvas, cropping Rive if necessary
var widget = const RiveAnimation.network(
  'https://cdn.rive.app/animations/vehicles.riv',
  fit: BoxFit.cover,
);

// Fit to the width and align to the top of the canvas
widget = const RiveAnimation.network(
  'https://cdn.rive.app/animations/vehicles.riv',
  fit: BoxFit.fitWidth,
  alignment: Alignment.topCenter,
);
```
{% endtab %}

{% tab title="iOS" %}
See values and descriptions in the sections below for `Fit` and `Alignment`.\
\
The runtime provides the following enums to set on layout parameters:\


* Fit
  * `.fitFill`
  * `.fitContain`
  * `.fitCover`
  * `.fitFitWidth`
  * `.fitFitHeight`
  * `.fitScaleDown`
  * `.fitNone`
* Alignment
  * `.alignmentTopLeft`
  * `.alignmentTopCenter`
  * `.alignmentTopRight`
  * `.alignmentCenterLeft`
  * `.alignmentCenter`
  * `.alignmentCenterRight`
  * `.alignmentBottomLeft`
  * `.alignmentBottomCenter`
  * `.alignmentBottomRight`



### SwiftUI

The following example shows how to set layout parameters and switch them at runtime:

```swift
struct SwiftLayout: View {
    @State private var fit = Fit.fitContain
    @State private var alignment = Alignment.alignmentCenter
    
    var body: some View {
        VStack {
            RiveViewModel(fileName: "off_road_car_blog", fit: fit, alignment: alignment).view()
        }
        HStack {
            Text("Some Fit Examples")
        }
        HStack {
            Button("Fill", action: {fit = .fitFill})
            Button("Contain", action: {fit = .fitContain})
            Button("Cover", action: {fit = .fitCover})
        }
        HStack {
            Text("Some Alignment Examples")
        }
        HStack {
            Button("Top Left", action: {alignment = .alignmentTopLeft})
            Button("Top Center", action: {alignment = .alignmentTopCenter})
            Button("Top Right", action: {alignment = .alignmentTopRight})
        }
    }
}
```

### UIKit

The following example shows how to set layout parameters and switch them at runtime:

```swift
class LayoutViewController: UIViewController {
    @IBOutlet weak var riveView: RiveView!
    var viewModel = RiveViewModel(fileName: "truck_v7")
    
    override func viewDidLoad() {
        viewModel.setView(riveView)
    }
    
    @IBAction func fitButtonTriggered(_ sender: UIButton) {
        setFit(name: sender.currentTitle!)
    }
    
    @IBAction func alignmentButtonTriggered(_ sender: UIButton) {
        setAlignment(name: sender.currentTitle!)
    }
    
    func setFit(name: String = "") {
        var fit: Fit = .fitContain
        switch name {
        case "Fill": fit = .fitFill
        case "Contain": fit = .fitContain
        case "Cover": fit = .fitCover
        case "Fit Width": fit = .fitFitWidth
        case "Fit Height": fit = .fitFitHeight
        case "Scale Down": fit = .fitScaleDown
        case "None": fit = .fitNone
        default: fit = .fitContain
        }
        viewModel.fit = fit
    }
    
    func setAlignment(name: String = "") {
        var alignment: Alignment = .alignmentCenter
        switch name {
        case "Top Left": alignment = .alignmentTopLeft
        case "Top Center": alignment = .alignmentTopCenter
        case "Top Right": alignment = .alignmentTopRight
        case "Center Left": alignment = .alignmentCenterLeft
        case "Center": alignment = .alignmentCenter
        case "Center Right": alignment = .alignmentCenterRight
        case "Bottom Left": alignment = .alignmentBottomLeft
        case "Bottom Center": alignment = .alignmentBottomCenter
        case "Bottom Right": alignment = .alignmentBottomRight
        default: alignment = .alignmentCenter
        }
        viewModel.alignment = alignment
    }
}
```
{% endtab %}

{% tab title="Android" %}
The animation view can be further customized as part of specifying layout attributes.

`fit` can be specified to determine how the animation should be resized to fit its container. The available choices are `FILL` , `CONTAIN` , `COVER` , `FIT_WIDTH` , `FIT_HEIGHT` , `NONE` , and `SCALE_DOWN` .

`alignment` informs how it should be aligned within the container. The available choices are `TOP_LEFT` , `TOP_CENTER` , `TOP_RIGHT` , `CENTER_LEFT` , `CENTER` , `CENTER_RIGHT` , `BOTTOM_LEFT` , `BOTTOM_CENTER` , and `BOTTOM_RIGHT` .

See more about these values and meanings in the sections below.

\
Specify the layout values in your Resource layout:

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:riveResource="@raw/off_road_car_blog"
    app:riveAlignment="TOP_CENTER"
    app:riveFit="FILL"
    />
```

Or in your Activity code:

```kotlin
animationView.fit = Fit.FILL
animationView.alignment = Alignment.CENTER
```


{% endtab %}
{% endtabs %}



## Fit

Fit determines how the Rive content will be fitted to the view. There are a number of options available:

* `Cover`: Rive will cover the view, preserving the aspect ratio. If the Rive content has a different ratio to the view, then the Rive content will be clipped.
* `Contain`: **(Default)** Rive content will be contained within the view, preserving the aspect ratio. If the ratios differ, then a portion of the view will be unused.
* `Fill`: Rive content will fill the available view. If the aspect ratios differ, then the Rive content will be stretched.
* `FitWidth`: Rive content will fill to the width of the view. This may result in clipping or unfilled view space.
* `FitHeight`: Rive content will fill to the height of the view. This may result in clipping or unfilled view space.
* `None`: Rive content will render to the size of its artboard, which may result in clipping or unfilled view space.
* `ScaleDown`: Rive content is scaled down to the size of the view, preserving the aspect ratio. This is equivalent to `Contain` when the content is larger than the canvas. If the canvas is larger, then `ScaleDown` will not scale up.

## Alignment

Alignment determines how the content aligns with respect to the view bounds. The following options are available:

* `Center` **(Default)**
* `TopLeft`
* `TopCenter`
* `TopRight`
* `CenterLeft`
* `CenterRight`
* `BottomLeft`&#x20;
* `BottomCenter`
* `BottomRight`

## Bounds

The bounds for the area in which the Rive content will render can be set by providing the minimum and maximum x and y coordinates. These coordinates are relative to the view in which the Rive content is contained, and all must be provided. These will override alignment settings.

* `minX`
* `minY`
* `maxX`
* `maxY`
