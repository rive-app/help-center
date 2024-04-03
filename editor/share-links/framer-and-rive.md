---
description: Learn more about the Rive integration into Framer
---

# Framer and Rive

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/framer-and-rive/docNJ1eVCEmG).
{% endhint %}

## Overview

Framer allows users to build functional websites fast in a low/no-code platform. Integrate Rives into Framer sites to build and control animations and interactive content without having to write custom code with [runtimes](https://help.rive.app/runtimes/overview). See more below on how to get started.

## Getting Started

{% embed url="https://www.youtube.com/watch?v=jM8DQmPw0pc" %}
Demo of the Rive and Framer integration
{% endembed %}

With the Framer integration, you get control over Rives with options such as:

* Playback controls
* Configuration options that affect the Rive layout on a canvas
* State machine inputs

### 1. Make a Share Link

To start, generate a new share link in the Rive editor and copy the _Framer code_ snippet from the Rive editor.

![Framer code snippet generated from the Rive editor](<../../.gitbook/assets/Screen Shot 2022-08-15 at 6.26.22 PM.png>)

### 2. Paste Code Snippet into Framer

In your Framer project on the left-side panel, click on the _Assets_ tab, and in the _Code_ section, add a new Code component file.

![Adding a Code component in Framer](<../../.gitbook/assets/CleanShot 2022-08-12 at 14.59.58.gif>)

In the new code file, replace all of the boilerplate code with the copied Framer code snippet from Rive and save the file. You should see the preview of your Rive file playing the animation or state machine set when generating a share link in the Rive editor.

![Rive Framer snippet pasted in a new component](<../../.gitbook/assets/CleanShot 2022-08-12 at 18.37.51.gif>)

You'll now have access to embed this newly created Rive component in any of your Framer pages. Drag and drop the component into your page and size as needed.

![Adding a Rive code component to a page](<../../.gitbook/assets/CleanShot 2022-08-14 at 12.08.15.gif>)

### 3. Control your Rive file in Framer

When you drop in a Rive component, you'll notice on the properties panel (right-hand side) some new options that are specific to the Rive component. The view will likely look different for every Rive component, as some properties are dynamically generated. See more below on configuration options when it comes to controlling Rives.

![](<../../.gitbook/assets/Screen Shot 2022-08-15 at 6.28.40 PM.png>)

#### Controlling Rives: Layout

There are two settings for controlling how animations are rendered on the canvas display:

* `Fit` - How Rive content is fitted into the view size
* `Alignment` - How Rive content aligns with respect to view bounds

Learn more about the different options [here](https://help.rive.app/runtimes/layout).

#### Controlling Rives: Animation Playback Control

By default, Rives will autoplay the animation/state machine specified when the share link was generated. However, you have the ability to control when Rives will play and/or pause if needed. There are two playback control options:

* `Playback` - Play or pause Rives in the Framer Preview/Published mode
* `Play on Canvas` - Play or pause Rives in the Framer canvas mode

#### Controlling Rives: State Machine Inputs

If you generate a share link with a state machine, you get access to all of the associated state machine inputs to manipulate on the Framer page. This means you can connect state machine input values to Framer component variants via interactions, such as users clicking or hovering a button.

For example, say we want to activate the switch example below to turn on when a user hovers a "Hover Me" button below the canvas. To accomplish this, we'll [create a new Framer component](https://www.framer.com/learn/component-creation/) by grouping the Rive component with the button.

![Grouped Rive and button component](<../../.gitbook/assets/Screen Shot 2022-08-15 at 1.39.34 PM.png>)

In this Primary variant, we keep the default state machine input `isPressed` to `false` on the Rive component in the Properties panel. Next, we'll create another variant of this group, `SwitchOn`,  where `isPressed` is set to "Yes" (`true`).&#x20;

![Adding a variant where isPressed is set to "Yes"](<../../.gitbook/assets/Screen Shot 2022-08-15 at 1.55.26 PM.png>)

Finally, we'll create a transition from the button in the `Primary` variant to the `SwitchOn` variant when a `MouseEnter` interaction occurs.

![Connecting button interactions to the SwitchOn variant](<../../.gitbook/assets/CleanShot 2022-08-15 at 16.08.38.gif>)

This effectively connects the hover state of the button to the `true` value of the `isPressed` state machine input, and changes the Rive component's state machine accordingly.

![Button toggling a Rive state machine input](<../../.gitbook/assets/CleanShot 2022-08-15 at 16.19.31 (2).gif>)

While this is one way to connect state machines to user interaction outside the Rive canvas, there are plenty of other scenarios as well. Check out Framer's [learn page](https://www.framer.com/learn/) to discover more.

{% hint style="info" %}
Note that in the example scenario above, a similar outcome could have been achieved through using [Listeners](https://help.rive.app/editor/state-machine#listeners) in the Rive editor, but because we want to advance a state machine from outside of the Rive canvas, we need to manipulate state machine inputs manually.
{% endhint %}
