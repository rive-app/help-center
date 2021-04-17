# Mixing animations

{% tabs %}
{% tab title="Web" %}
We play and mix _load_ and _vibration_ together at different stages in time as an example progress indicator, which we’re playing at one-fifth speed. Once progress is complete, we play the _end_ animation.

```javascript
if (loadInstance.time < 1.0) {
  loadInstance.advance(elapsedTime * 0.2);
  loadInstance.apply(artboard, 1.0);
} else {
  endInstance.advance(elapsedTime);
  endInstance.apply(artboard, 1.0);
}
if (loadInstance.time > 0.5 && loadInstance.time < 1.0) {
  vibrationInstance.advance(elapsedTime);
  vibrationInstance.apply(artboard, loadInstance.time);
}
```
{% endtab %}

{% tab title="Flutter" %}

{% endtab %}

{% tab title="iOS" %}

{% endtab %}

{% tab title="Android" %}

{% endtab %}
{% endtabs %}

