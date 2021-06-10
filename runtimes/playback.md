---
description: Playing and pausing animations
---

# Playback

Rive lets you specify what artboard to use, what animations and state machines to mix and play, and to control the play/pause state of each animation.

## Choosing an artboard

When a Rive object is instantiated, the artboard to use can be specified. If no artboard is given, the default artboard is used. Only one artboard can be used at a time.

{% tabs %}
{% tab title="web" %}
```javascript
new rive.Rive({
    src: 'https://cdn.rive.app/animations/truck.riv',
    canvas: document.getElementById('canvas'),
    artboard: 'Truck Motion',
    autoplay: true
});
```
{% endtab %}

{% tab title="Flutter" %}
```dart
RiveAnimation.network(
    'https://cdn.rive.app/animations/truck.riv',
    artboard: 'Truck Motion'
);
```
{% endtab %}
{% endtabs %}

## Choosing starting animations

Starting animations can also be chosen when Rive is instantiated. A list of animation names can be provided.

{% tabs %}
{% tab title="web" %}
```javascript
// Play the idle animation
new rive.Rive({
    src: 'https://cdn.rive.app/animations/truck.riv',
    canvas: document.getElementById('canvas'),
    animations: ['idle', 'curves'],
    autoplay: true
});

// play and mix the idle and curves animations
new rive.Rive({
    src: 'https://cdn.rive.app/animations/truck.riv',
    canvas: document.getElementById('canvas'),
    animations: ['idle', 'curves'],
    autoplay: true
});
```
{% endtab %}

{% tab title="Flutter" %}
```dart
// Play the curves animation
RiveAnimation.network(
    'https://cdn.rive.app/animations/truck.riv',
    animation: 'curves'
);
```
{% endtab %}
{% endtabs %}

