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

Once you've decided on an artboard, you'll need to add the animations or state machine you want the nested artboard to use in the Inspector.

![](<../../.gitbook/assets/2021-10-14 13.44.55.gif>)

When selecting animations to add to a Nested Artboard, you'll be prompted to select whether the animation is a Simple or Remap animation.&#x20;



#### Simple

Simple animations allow you to key start, end, speed, and mixing values on the timeline. &#x20;

#### Remap

Remap animations allow you to speed up, slow down, or play an animation in reverse by setting keys on the timeline.



### Animations and Mixing

As you add additional animations to a nested artboard, animations begin to mix together. This mixing is important, especially when multiple animations share keyed properties. Without adjusting this value, your nested artboard may not playback your animations in the way you want.



By default, any animation added to a nested artboard starts with a mix value of 100%. You can adjust this value in design mode, or in a specific animation by setting keys. **Note that an animation that has a non-zero mix value will always be mixing with other animations, regardless if it has a play key set or not.**



To ensure the correct animation is playing, ensure that you key the mix value for the desired animation to 100% and all other animations have a mix of 0%.
