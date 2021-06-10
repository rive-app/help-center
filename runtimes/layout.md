---
description: Controlling the layout of your Rive animation
---

# Layout

Rive provides a number of ways in which to control how your animations are laid out in the canvas or view used to display it. Rive lets you control the fit, alignment and offset of rendered content.

## The `Layout` object

Most runtimes have a `Layout` object. You typically provide layout data when instantiating a Rive object, and can update at any time.

{% tabs %}
{% tab title="web" %}
```markup
<div>
    <canvas id="canvas" width="800" height="600"></canvas>
</div>
<script src="https://unpkg.com/rive-js"></script>
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

{% tab title="Flutter" %}
Rive's Flutter runtime doesn't have a `Layout` object; instead fit and alignment can be passed in through the `RiveAnimation` and `Rive` constructors. The fit and alignment options behave like their counterparts in Flutter \(see [`BoxFit`](https://api.flutter.dev/flutter/painting/BoxFit-class.html) and [`Size`](https://api.flutter.dev/flutter/dart-ui/Size-class.html) as examples\).

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
{% endtabs %}

## Fit

Fit determines how the Rive content will be fitted to the view. There are a number of options available:

* `Cover`: Rive will cover the view, preserving aspect ratio. If the Rive content has a different ratio to the view, then the Rive content will be clipped.
* `Contain`: Rive content will be contained within the view, preserving aspect ratio. If the ratios differ, then a portion of the view will be unused.
* `Fill`: Rive content will fill the available view. If the aspect ratios differ, then the Rive content will be stretched.
* `FitWidth`: Rive content will fill to the width of the view. This may result in clipping or unfilled view space.
* `FitHeight`: Rive content will fill to the height of the view. This may result in clipping or unfilled view space.
* `None`: Rive content will render to the size of its artboard, which may result in clipping or unfilled view space.
* `ScaleDown`: Rive content is scaled down to the size of the view, preserving aspect ratio. This is equivalent to `Contain` when the content is larger than the canvas. If the canvas is larger, then `ScaleDown` will not scale up.

## Alignment

Alignment determines how the content aligns with respect to the view bounds. The following options are available:

* `Center`
* `TopLeft`
* `TopCenter`
* `TopRight`
* `CenterLeft`
* `CenterRight`
* `BottomLeft` 
* `BottomCenter`
* `BottomRight`

## Bounds

The bounds for the area in which the Rive content will render can be set by providing the minimum and maximum x and y coordinates. These coordinates are relative to the view in which the Rive content is contained, and all must be provided. These will override alignment settings.

* `minX`
* `minY`
* `maxX`
* `maxY`



