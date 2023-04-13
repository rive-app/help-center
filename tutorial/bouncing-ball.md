# Bouncing ball

## Intro

In this tutorial, we'll show you how to build a bouncing ball in Rive. This tutorial is broken into three parts, design, rigging, and animation. Feel free to jump around and go directly to the information you are looking for.

{% embed url="https://www.youtube.com/watch?v=zqaoTmtreFA" %}

## Design

### 1.Create a new file

Create a new file and choose an Artboard size.

![Create a new file](../.gitbook/assets/2021-08-09-13.16.56.gif)

Select the Artboard and modify its Fill in the [Inspector](../editor/fundamentals/interface-overview/inspector.md).

![](../.gitbook/assets/2021-08-10-15.26.21.gif)

### 2. Create the ball

Use the [ellipse tool](../editor/fundamentals/procedural-shapes.md) to create the base for the ball.

![](../.gitbook/assets/2021-08-10-15.29.11.gif)

Select the shape and change the [fill type](../editor/fundamentals/fill-and-stroke/#fill-type) from solid to radial. Reposition the stoppers and select some colors.

![Change fill to radial and adjust colors and stoppers](../.gitbook/assets/2021-08-09-13.38.44.gif)

Add an additional Fill in the Inspector. Adjust the stoppers and choose some colors.

![Add linear gradient and customize](../.gitbook/assets/2021-08-09-13.49.12.gif)

### 3. Add details

Using the [pen tool](../editor/fundamentals/pen-tool/), create a line across the ball. Duplicate and rotate that line to create a cross.&#x20;

![](../.gitbook/assets/2021-08-10-15.37.00.gif)

Select a line on the Stage, remove the Fill, and add a [Stroke](../editor/fundamentals/fill-and-stroke/#stroke). Change the thickness and color, then change the blend mode from normal to multiply.&#x20;

![](../.gitbook/assets/2021-08-10-15.41.03.gif)

Drag the [path layer](../editor/fundamentals/shapes-and-paths/#path-layer) of the vertical line onto the horizontal line [shape layer](../editor/fundamentals/shapes-and-paths/#shape-layer). Be sure to delete the empty shape layer in the Inspector.

![](../.gitbook/assets/2021-08-10-15.44.29.gif)

Add an ellipse, then duplicate it. Drag both of the ellipse path layers onto the shape layer we are using for the lines.

![](../.gitbook/assets/2021-08-10-15.47.40.gif)

Select the shape layer and add [clipping](../editor/manipulating-shapes/clipping.md) in the Inspector. When prompted, select the ball layer as the clipping source.&#x20;

![](../.gitbook/assets/2021-08-09-14.36.28.gif)

Change the [origin](../editor/fundamentals/origin-and-freeze.md) of the detail shape.

![](../.gitbook/assets/2021-08-10-12.39.42.gif)

To add rim lighting, duplicate the ball shape and remove the fill. Add a stroke and change the thickness. Scale the ellipse down slightly.

![Rim light part 1](<../.gitbook/assets/2021-08-09-16.00.11 (1) (1).gif>)

Change the fill to linear and adjust the position of the stoppers. Customize the colors and turn the [opacity](../editor/fundamentals/interface-overview/inspector.md#appearance-properties) down.

![Rim light part 2](<../.gitbook/assets/2021-08-09-16.07.05 (1).gif>)

Add another stroke and copy the thickness from the first stroke. Change the color.

![](../.gitbook/assets/2021-08-10-15.53.57.gif)

Open the stroke options and [enable trim path](../editor/manipulating-shapes/trim-path.md#enable-trim-path). Adjust the [start](../editor/manipulating-shapes/trim-path.md#start-and-end) and [offset](../editor/manipulating-shapes/trim-path.md#offset) until the stroke is on the top of the ball.&#x20;

![](../.gitbook/assets/2021-08-10-15.56.08.gif)

Give the stroke [rounded endcaps](../editor/fundamentals/fill-and-stroke/#cap), then change the blend mode to overlay.

![](../.gitbook/assets/2021-08-10-15.59.11.gif)

### 4. Create shadow

Create a shadow with a new ellipse. Use a radial gradient, customize the colors, then use opacity to create a feathered edge. Scale down the ellipse and position it below the ball.

![](../.gitbook/assets/2021-08-10-16.01.03.gif)

## Rigging

### 1. Group the main basketball shapes

Select all shapes that make up the basketball and create a [group](../editor/fundamentals/groups/). Do this with `CMD+G` or `CTRL+G`.

![](../.gitbook/assets/2021-08-10-16.06.17.gif)

### 2. Create a group for the shadow

Create a group for the shadow.

![](../.gitbook/assets/2021-08-10-16.09.49.gif)

### 3. Create a root group

Group all objects on the stage in a "root" group. We won't use this group when animating, but it is beneficial if you need to change the entire composition's position, scale, or rotation.

![](../.gitbook/assets/2021-08-10-16.11.13.gif)

## Animation

### 1.Key initial motion

Switch the editor to [Animate mode](../editor/animate-mode/) with the `Tab` key.

![](../.gitbook/assets/2021-08-09-17.34.22.gif)

Key the initial position of the ball group. Notice a key is automatically created when you transform the group.

![](../.gitbook/assets/2021-08-09-17.41.13.gif)

Move the playhead forward to 30f, then key the ball in the down position. Next, move the playhead to the end of the timeline then copy and paste the first key to this location.

![](../.gitbook/assets/2021-08-09-17.45.22.gif)

Change the animation from one shot to loop, then preview the animation.

![](../.gitbook/assets/2021-08-09-17.49.59.gif)

### 2. Add easing

Select the first key and change the [easing](../editor/animate-mode/interpolation-easing.md) from [linear](../editor/animate-mode/interpolation-easing.md#linear) to [cubic](../editor/animate-mode/interpolation-easing.md#cubic). Drag the right handle down to create an ease-in curve.

![Create ease-in curve](../.gitbook/assets/2021-08-10-10.09.33.gif)

Select the second key and change the easing from linear to cubic. Drag the left handle up to create an ease-out curve.

![Create ease-out curve](../.gitbook/assets/2021-08-10-10.10.14.gif)

Preview the animation and adjust the curves.

### 3. Squash and Stretch

Key initial scale at the beginning of the timeline.

![](../.gitbook/assets/2021-08-10-10.50.37.gif)

Stretch the ball as it falls.

![](../.gitbook/assets/2021-08-10-11.53.44.gif)

Squash the ball as it hits the ground.

![](../.gitbook/assets/2021-08-10-12.01.36.gif)

Copy and paste the second scale key where the ball starts to rise again.

![](../.gitbook/assets/2021-08-10-12.08.55.gif)

Paste the first scale key to the end of the timeline.

![](../.gitbook/assets/2021-08-10-12.11.14.gif)

Add additional position keys to account for the squash and stretch.

![](../.gitbook/assets/2021-08-10-12.20.09.gif)

Adjust easing for all keys.

![](../.gitbook/assets/2021-08-10-12.30.46.gif)

### 4. Rotate detail shapes

Key initial rotation.

![](../.gitbook/assets/2021-08-10-12.48.05.gif)

Rotate shape slightly where the ball hits the ground. Copy the rotation key and paste it where the ball begins rising.

![](../.gitbook/assets/2021-08-10-12.48.27.gif)

Move playhead to end of timeline and set a rotation key at 180 degrees.

![](../.gitbook/assets/2021-08-10-12.48.58.gif)

### 5. Animate shadows scale and opacity

Key the initial X scale of the shadow group.&#x20;

![](../.gitbook/assets/2021-08-10-13.09.43.gif)

Increase the X scale of the shadow group as the ball hits the ground.

![](../.gitbook/assets/2021-08-10-13.33.29.gif)

Copy and paste first scale key to end of timeline.

![](../.gitbook/assets/2021-08-10-13.45.23.gif)

Key initial opacity of shadow group.

![](../.gitbook/assets/2021-08-10-13.46.59.gif)

Increase opacity as ball hits the ground. Maintain this opacity while the ball is on the ground.

![](../.gitbook/assets/2021-08-10-13.47.19.gif)

Copy and paste first opacity key to the end of the timeline.

![](../.gitbook/assets/2021-08-10-13.47.35.gif)

Ensure shadow group easing matches the ball group.

![](../.gitbook/assets/2021-08-10-14.05.33.gif)

Preview the animation and make any adjustments necessary.

![](../.gitbook/assets/2021-08-10-14.09.31.gif)
