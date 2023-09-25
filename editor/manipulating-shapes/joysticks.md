# Joysticks

## **Joystick Overview**

Joysticks make it easy to set up sophisticated rigs with simple controls. You can quickly animate body poses, eyes, mouths, hands, and more. You can even control joysticks with other joysticks.

They work by allowing you to pan through different timelines that are assigned to either the X or Y axis of the joystick. Once you’ve assigned the timelines, you can use the joystick to set keys to create animations.

Either watch the video, or read more below.

{% embed url="https://youtu.be/PvZYDAUnLXE" %}

## **Creating a new Joystick**

To create a new joystick, either find the Joystick Tool in the Bones Menu, or by hitting the J key. Once the tool is activated, click anywhere on the stage to add a new Joystick.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 13.49.22.gif" alt=""><figcaption><p>Creating a new Joystick</p></figcaption></figure>

## **Joystick Properties**

With the Joystick selected, you’ll see a number of properties that you can use in the Inspector.

### **Handle**

The Handle X and Y property describes the position that the handle is currently in. You’ll notice that these properties update as you move the handle around the Joystick.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 13.57.46.gif" alt=""><figcaption><p>Handle property</p></figcaption></figure>



### Position

The position property describes the position of the Joystick on the stage. For the most part, we won't need to update this property at all while animating.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 14.01.02.gif" alt=""><figcaption><p>Position property</p></figcaption></figure>

### Size

The size property describes the size of the joystick. We can modify this property to fit our needs by making the joystick longer/shorter or wider/skinnier.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 14.03.26.gif" alt=""><figcaption></figcaption></figure>

### Origin

The origin property exists because all objects in Rive that have a position must have an origin. You likely won’t ever change this property.

### Draw in World Space

This toggle tells the joystick whether the joystick will scale with the zoom level or not. This can be a very helpful joystick when we use many different joysticks in a file attached to a character or object.

### X & Y dropdown

The X & Y dropdowns allow us to assign timelines to the different axis of the Joystick.

For example, I’ve got a timeline that moves a ball on the Y axis. You can see that the timeline only has two keys which move the balls position and a few others that control the scale.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 14.28.17.gif" alt=""><figcaption></figcaption></figure>

When the timeline is assigned to the Y axis of the Joystick, notice that the joystick becomes a slider that only allows you to move the handle in the Y direction.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 14.31.15.gif" alt=""><figcaption></figcaption></figure>

As we move the joystick up and down, you’ll notice that the ball also moves up and down. Keep in mind that we are scrubbing through the assigned timeline. We can now use this joystick to set keys on a new timeline to create animations.

**Invert Toggle**

After you add a timeline to an axis, you’ll notice a toggle that appears next to it. This allows you to invert the direction that the joystick scrubs through the timeline.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-25 at 14.45.40.gif" alt=""><figcaption><p>Invert toggle</p></figcaption></figure>

### Handle Source

The handle source allows you to constrain the position of the handle to another objects position properties. This is useful when you want a control on the artboard to control the panning of a timeline.

## Joystick considerations

Joysticks are a powerful tool that allows you to create complex deformations, but like with many things in the Rive editor there are a few things to keep in mind as you set up these controls.

### Conflicting properties

The most important consideration is that when you have multiple animations assigned to the joystick, those timelines will blend together. If, for example, both timelines Y property of an object, these joysticks will conflict and prevent that object from moving in the Y direction.

Be sure to separate animated properties to prevent any conflicts.

### Creating Complex Deformations

You can add as many keys to a joystick timeline as you’d like. By doing this, we can create incredibly complex deformations, but remember, these deformations will often bloat the size of your file, so use them sparingly.
