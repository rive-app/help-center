# Nested Artboards

Nested Artboards can speed up your workflow by allowing you to reuse common animations. Any changes made to the source Artboard will be reflected across any of the Nested Artboards.

## Creating a Nested Artboard

Create an instance of an artboard by selecting the Nested Artboard tool and clicking in your active artboard.

![](<../../.gitbook/assets/2021-10-14 13.36.13.gif>)

{% hint style="info" %}
The Nested Artboard will display as a blank group on the stage until it is configured in the Inspector.
{% endhint %}

## Configuring a Nested Artboard

![](<../../.gitbook/assets/2021-10-14 13.40.20.gif>)

With the Nested Artboard selected, find the Artboard dropdown in the Inspector and select the desired Artboard you would like to nest. Notice that once the artboard is selected an instance of it now displays on the stage.

### Simple vs Remap Animations

Once you've decided on an artboard, you'll need to add the animations you want the nested artboard to use in the Inspector.

![](<../../.gitbook/assets/2021-10-14 13.44.55.gif>)

When selecting animations to add to a Nested Artboard, you'll be prompted to select whether the animation is a Simple or Remap animation.&#x20;

#### Simple

Simple animations allow you to key start, end, speed, and mixing values on the timeline.&#x20;

#### Remap

Remap animations allow you to speed up, slow down, or play an animation in reverse by setting keys on the timeline.





