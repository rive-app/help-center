# Trim Path

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/trim-path/docN5REOqqEq).
{% endhint %}

The Trim Path feature allows you to draw only a portion of the stroke on a vector shape. This can be used to create a variety of animations where a line needs to follow a path. Every stroke you create for a shape can have its own independent Trim Path.

![](../../.gitbook/assets/trimpath.gif)

## Enable trim path

To activate Trim Path, select a shape that has a stroke and click the stroke options in the inspector. Now, use the Trim Path drop-down menu and select either Sequential or Synced mode. Both modes enable Trim Path, but behave differently when used on a shape with multiple paths.

### Sequential

When Trim Path is set to Sequential, paths are animated sequentially. The order in which they animate is dictated by their order under the shape.

![](../../.gitbook/assets/sequential\_fixed.gif)

### Synced

Synced mode animates the trim path along all paths concurrently.

![](../../.gitbook/assets/synced.gif)

## Start and end

The trim of a stroke happens from a Start point to an End point. By default, all shapes have a Stroke that starts at 0% and ends at 100%. Change these values to modify the position of the Start and End points of the trim (which are represented by a percentage of the full length of the path).&#x20;

![](../../.gitbook/assets/start\_end.gif)

## Offset

Use Offset to easily move the trimmed portion of the path.

![](../../.gitbook/assets/offset.gif)
