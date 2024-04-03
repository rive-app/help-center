# Interpolation (Easing)

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/interpolation-easing/docP9A6iGEsI).
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=087zael11_M&list=PLujDTZWVDSsFGonP9kzAnvryowW098-p3&index=26" %}

When you set two keys on a property, the value in between those keys is automatically calculated. This is called interpolation. Interpolation settings can be customized to create dramatically different results.

You can set the easing on your keys by either using the Interpolation panel to the right of the Timeline, or by using the[ Graph Editor](interpolation-easing.md#using-the-graph-editor), which you can toggle on via the shortcut near the Timeline options.

## Using the Interpolation Panel

The Interpolation Panel appears to the right of the timeline when you select any number of keys on the timeline.

![](<../../.gitbook/assets/Screen Shot 2023-04-03 at 4.50.51 PM.png>)

The interpolation graph is a visual representation of how the value will change over time from the selected key to the next with the x-axis representing time and the y-axis representing the change in the chosen property.

You can choose which interpolation type to use by selecting any of the icons above the graph.

### **Linear**

![Linear Interpolation](<../../.gitbook/assets/2023-04-03 16.53.48.gif>)

Linear is the default interpolation type, and it creates a constant rate of change from one key value to the next.

### **Cubic**

![](<../../.gitbook/assets/2023-04-03 17.01.12.gif>)

Cubic interpolation uses a curve to interpolate between key values. It gives you two handles that can be dragged to customize the curve.

You can drag the handles as far as you want on the Y-axis. If you drag the handles outside of the graph, the graph view will update to ensure that the handles are always in view.

The default cubic curve creates a gentle curve from the first key to the next, which results in the value changing slowly at the start and end, and changing the most in the middle.

### **Hold**

![](<../../.gitbook/assets/2023-04-03 17.07.53.gif>)

Hold doesn't interpolate values between keys. It simply holds the current value until the next key is reached, where the next value is set instantly.\


### Interpolation field

A text field below the preview graph represents the interpolation in a numerical format. A total of four values (typically between 0 and 1) represent the position of the handles – two for the inward curve, and two for the outward curve. You can see how these values change by dragging the handles within the preview window.

Use this field if you wish to set specific easing values, perhaps defined in a design language for a specific brand, for instance. The field also makes it easy to copy and paste values across files and tools.

When inputting values manually, use a comma or a space to separate each of the four values.&#x20;

![](../../.gitbook/assets/interpolation\_field.gif)

## The Graph editor

{% embed url="https://www.youtube.com/watch?v=9mQUqa4ud5c&list=PLujDTZWVDSsFGonP9kzAnvryowW098-p3&index=28" %}

Rive's Graph editor visually represents how an object's properties change over time. Using this graph, we can edit the rate of change and the values being interpolated.&#x20;



### Enabling the Graph editor

Use the Graph Editor button on the timeline to enable the Graph Editor. You'll notice that the Graph editor replaces the timeline.



Note that only objects that are selected will appear on the graph editor.&#x20;



### Using the Graph Editor

The graph editor gives a visual representation of the current interpolation. You have two ways to edit the interpolation on the graph.



#### Using  Cubic Interpolation

If you use the cubic interpolation in the interpolation panel, you'll need to adjust your interpolation in the interpolation panel.



#### Using Cubic value

Using the cubic value option in the interpolation panel will enable you to adjust the interpolation directly on the graph.\
\
Unlike cubic interpolation, cubic values can effect the actual value of an animated property.













