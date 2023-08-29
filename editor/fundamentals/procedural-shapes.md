# Procedural Shapes

{% embed url="https://www.youtube.com/watch?v=XDbYmb7id9Y&list=PLujDTZWVDSsFGonP9kzAnvryowW098-p3&index=10" %}

## Creating a procedural shape

![](../../.gitbook/assets/create-tools\_b.png)

Under the Create Tools menu, you will find shape tools that are defined by procedural properties like width, height, corner radius, number of points, and more.

![](<../../.gitbook/assets/createing-procedural-shape (1).gif>)

Select the tool then click and drag anywhere inside an artboard. Hold `shift` to constrain the proportions of the shape.

### Convert a procedural path to a custom path

To edit the vertices of a procedural path, press `Enter` on your keyboard. This will covert the path into a custom path and allow you to modify the position of each vertex. Procedural properties (e.g. width, height,  number of points) will no longer be available. Keep in mind that any animations applied to these properties will also be removed once the procedural path is converted to a custom path.

### Origin of a procedural path

The [origin](../manipulating-shapes/origin-and-freeze.md) of a procedural path determines where its properties originate from. For example, changing the width on a rectangle with its origin in the middle (50% X and 50% Y) causes it to grow from its center.

![](../../.gitbook/assets/procedural\_center.gif)

Changing the width of a rectangle with its origin on the left side (0% X) causes it to grow from its left.

![](../../.gitbook/assets/procedural\_left.gif)

This is particularly useful when animating paths that have other procedural properties enabled, such as rounded corners.

