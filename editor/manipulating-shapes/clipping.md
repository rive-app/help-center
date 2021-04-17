# Clipping

Clipping allows you to cut one shape out from another. 

To use clipping, select the shape you want to clip and hit the plus button next to the Clipping options in the Inspector. Now, select the path you want to use as a clipping mask.

![](../../.gitbook/assets/clipping_20px.gif)

You can add as many clipping paths to a shape as you'd like.

## Multiple shapes as source

![](../../.gitbook/assets/clipping_group.png)

You can use groups as a source for clipping to create different effects. In this example, we are adding clipping to a custom shape and using the ball as the source to create the lighting effect from the lamp. 

![Use group as clipping source](../../.gitbook/assets/clipping_group.gif)

To achieve this effect, we need to select the shape we are using for lighting, add clipping, and select the ball as the source.

![Reverse path direction](../../.gitbook/assets/clipping_fiix.gif)

In the event that you have shapes that aren't clipping, or only partially clipping, be sure to check the winding of that shape. In most cases, reversing the direction of the path with fix this problem.

 

## Inverse clipping

Clipping is typically used to hide a part of your graphics. In the example below, we're using an ellipse to show only part of our jewel graphic.

![](../../.gitbook/assets/clipping_jewel.png)

You occasionally may want to invert the clipping, so that only the graphics outside of the clipping paths are drawn.

![](../../.gitbook/assets/jewel-inversed.png)

This is achieved using a clipping path that looks like the gray shape in the image below.

![](../../.gitbook/assets/jewel-inversed-path.png)

To create this shape, draw a rectangle the size of the artboard. Add both the rectangle path and ellipse path to the same shape layer in your Hierarchy.

![](../../.gitbook/assets/screen-shot-2020-09-23-at-5.07.20-pm.png)

Note that your shape might not show a hole as ours does. That's because you need to set the Fill Rule of your shape to Even-Odd. This setting doesn't affect your Clipping Path, but it helps explain how the Even-Odd operation works, which will be useful later!

![](../../.gitbook/assets/screen-shot-2020-09-23-at-5.09.49-pm.png)

Select the group containing the jewel and use the "plus" icon in the Clipping section of the Inspector to add your clipping shape.

![](../../.gitbook/assets/screen-shot-2020-09-23-at-5.25.23-pm.png)

Open the Clip Options and set the Operation to Even-Odd.

![](../../.gitbook/assets/screen-shot-2020-09-23-at-5.20.32-pm.png)

Be sure to hide the visibility of your clipping shape so it's not covering your graphic.

![](../../.gitbook/assets/screen-shot-2020-09-23-at-5.22.43-pm.png)

