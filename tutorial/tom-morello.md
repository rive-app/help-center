# Tom Morello

## Intro

In this tutorial, we'll look at an advanced file and look into some of the techniques used. While we won't build the file step by step, we will examine the design, rig, animations, and state machine. For a more in-depth look, dive into the source file and look around.

![](../.gitbook/assets/2021-08-12-12.56.13.gif)

## Design

### 1. Use simple shapes

Notice the majority of the character uses simple shapes.

![Use simple shapes](../.gitbook/assets/2021-08-12-13.15.25%20%281%29.gif)

The arms and legs have additional vertices added to account for the knees and elbows.

![Use additional vertices for knees and elbows](../.gitbook/assets/2021-08-12-13.18.26.gif)



[Strokes](../editor/fundamentals/fill-and-stroke/#stroke) with [rounded end caps](../editor/fundamentals/fill-and-stroke/#cap) are used to create the fingers.

![Use stokes for fingers](../.gitbook/assets/2021-08-12-13.24.07.gif)

Notice the guitar cable is split into multiple shapes to give the cable a 3D effect.

![](../.gitbook/assets/2021-08-12-13.36.30.gif)



### 2. Rim lighting

Rim lighting helps your character stand out in the composition.

![](../.gitbook/assets/screen-shot-2021-08-12-at-2.14.48-pm.png)

Duplicate shapes and change their color to easily light one or multiple sides.

![](../.gitbook/assets/2021-08-12-15.05.23.gif)

Use gradient strokes on your shapes. As the light falls off, turn down the opacity or select a darker color.

![](../.gitbook/assets/2021-08-12-15.16.37.gif)

### 3. Background & effects

Use gradients with multiple stoppers to create a base for the background.

![](../.gitbook/assets/2021-08-12-15.26.52.gif)

Use blend modes to create unique effects such as lens flares, color shifts, and framing.

![Lens flare with Overlay mode](../.gitbook/assets/2021-08-12-15.36.35.gif)

![Color shift with Screen mode](../.gitbook/assets/2021-08-12-15.39.08.gif)

![Framing with Color Burn mode](../.gitbook/assets/2021-08-12-15.46.27.gif)

## Rigging

Start with a basic skeleton.

![](../.gitbook/assets/2021-08-16-09.32.59.gif)

Add in additional bones for accessories.

![](../.gitbook/assets/2021-08-16-09.52.06.gif)

Ensure the bones have the proper hierarchical relationships.

![](../.gitbook/assets/2021-08-16-12.41.45%20%281%29.gif)

Set up [IK constraints](../editor/constraints/ik-constraint.md) to make controlling the legs easier.

![](../.gitbook/assets/2021-08-16-12.51.42.gif)

For the best performance, [bind and weight](../editor/manipulating-shapes/bones/#2-binding) only the shapes that will be influenced by two or more bones. 

![](../.gitbook/assets/2021-08-16-13.09.57.gif)

Use [parent-child relationships](../editor/manipulating-shapes/bones/#1-hierarchical-relationships) for shapes or groups that only use a single bone.

![](../.gitbook/assets/2021-08-16-13.14.20%20%281%29.gif)

Ensure all objects are nested under a Root group before animating. This group will allow you to control the transform properties of the entire composition, even if you've already added some animations.

![](../.gitbook/assets/2021-08-16-13.20.51.gif)



## Animation

Animate the idle first. Let the control bone do most of the work.

![](../.gitbook/assets/2021-08-16-13.45.18.gif)

Add secondary motion to make the animation more interesting.

![](../.gitbook/assets/2021-08-16-13.45.55.gif)

If you are planning on using the animation in the [state machine](../editor/state-machine.md), be sure to key all objects \(bones, shapes, paths, ect...\) in their current position. This will ensure the character can return to the proper pose when switching between states.

![](../.gitbook/assets/2021-08-16-13.57.27.gif)

Use the bones to help you achieve the proper poses, then edit the timing.

![](../.gitbook/assets/2021-08-16-14.20.15.gif)

Once you are happy with the main poses and timing, add in secondary motion.

![](../.gitbook/assets/2021-08-16-14.38.23.gif)

## State Machine

Create a new state machine with the plus button in the animations list.

![](../.gitbook/assets/2021-08-16-14.51.22.gif)

Drag your animations onto the graph.

![](../.gitbook/assets/2021-08-16-15.00.10.gif)

Add [transitions](../editor/state-machine.md#create-transitions).

![](../.gitbook/assets/2021-08-16-15.05.09.gif)

Add an [input](../editor/state-machine.md#inputs).

![](../.gitbook/assets/2021-08-16-15.07.50.gif)

Configure transition [conditions](../editor/state-machine.md#conditions).

![](../.gitbook/assets/2021-08-16-15.10.19.gif)

Test and make any changes necessary.

![](../.gitbook/assets/2021-08-16-15.11.20.gif)











