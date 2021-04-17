# Tips and best practices

How you rig your design is important. A smart rig allows you to create fewer keys, making your animation easy to work with and keeping your timeline tidy.

## Use bones to animate multiple vertices together

With Rive you can bind vertices and bezier handles to bones. You can connect different bones to different vertices to control parts of a shape.

![](../../../.gitbook/assets/use_bones_3.png)

In this page-turn example, we've connected the bezier handles at the top and bottom of the page to a single bone.

![](../../../.gitbook/assets/uso_bones_4.gif)

This allows you to easily deform the page with just a few bones acting as controls.

## The way you weight vertices or handles is important

Weighting a vertex and its handles differently allows you to create interesting deformations.

![](../../../.gitbook/assets/uso_bones_5.gif)

In this example, the top and bottom bezier handles \(on the back shape of the orange\) are weighted differently from their vertices. This causes the bezier handles to move at a different speed as the connected bone is scaled, creating a 3D effect.

![](../../../.gitbook/assets/uso_bones_6.gif)

Notice how the vertices and handles on the back shape of the orange move as the bone's scale changes.

