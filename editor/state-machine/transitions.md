# Transitions

Transitions supply the logical map for the state machine to follow. There are a number of considerations and configurable properties for transitions that we will cover below. Note that we’ll briefly discuss Inputs as well, so be sure to read more about those as well here.

## **Creating a new Transition**

To create a transition, place your mouse near the state you want to leave until you notice the ellipse appear. Click and drag the ellipse to the state you want to transition to. Once you’ve connected two states, you’ll notice an ellipse with an arrow icon indicating the transition's direction.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.01.56.gif" alt=""><figcaption><p>Creating a transition</p></figcaption></figure>

Note that you can create multiple transitions from one state to another. Each of these transitions can require a different condition to be met, which will fire the transition, thus giving you the ability to make “or” conditions.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.05.05.gif" alt=""><figcaption><p>Creating an "or" transition</p></figcaption></figure>

## **Configuring a Transition**

Once you’ve added a transition, selecting the direction indicator will allow you to configure the transition. There are three different sections to the transition panel, the transition properties, conditions, and interpolation.

### **Transition properties**

The transition properties allow you to customize how a transition occurs.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.08.54@2x.png" alt=""><figcaption><p>Transition properties</p></figcaption></figure>

### **Duration**

The duration property describes how long it takes for a transition to occur.

The duration is set to zero by default, meaning the transition happens immediately. So, when we transition between these two animations, it appears as though the object snaps from one side of the artboard to the other.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.10.49.gif" alt=""><figcaption><p>Duration of 0ms</p></figcaption></figure>

If we increase the duration, you’ll notice that the higher the number, the longer the transition takes.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.14.55.gif" alt=""><figcaption><p>Duration of 500ms</p></figcaption></figure>

In a way, transitions act as their own animation. The starting properties (coming from the state your state machine is leaving from) will be interpolated to the ending properties (the starting properties of the state your state machine is going to). If the starting properties are the first key on a timeline, and the ending properties are the second key, the duration is the timing between those two keys. Transitions are much more complex than this, but thinking about transitions this way will help you diagnose issues with your state machine.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.18.50.gif" alt=""><figcaption><p>Interpolation on a Transition</p></figcaption></figure>

Much like keys on our timeline, we can change the interpolation, which we’ll discuss more below.

### **Exit Time**

Exit Time tells the state machine how much of the state must play before transitioning.

By default, Exit Time is unchecked. If you want to enable the Exit Time, use the check box. Once the setting is enabled, you can use either a time value or percent.&#x20;

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.24.13.gif" alt=""><figcaption><p>Using Exit Time</p></figcaption></figure>

For example, if you want the state machine to play the entire animation before transitioning, you can either enter the duration of the animation, or use 100%.

### **Pause when exiting**

The Pause When Exiting option pauses the animation you are leaving from during the transition.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.31.25.gif" alt=""><figcaption><p>Pause when exiting</p></figcaption></figure>

As we discussed in the duration section, when a transition happens, properties from the first state are mixed with the first key of the next state. In reality, the animation your state machine leaves from continues to play as the transition happens.

### Conditions

Conditions are the rules for our transitions. Without conditions, our transitions would continuously fire and our state machine would likely look either glitchy, or only play a single animation. Conditions require us to define some inputs, which you can read more about [here](inputs.md).

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.37.07@2x.png" alt=""><figcaption><p>Conditions</p></figcaption></figure>

#### **Adding a new condition**

To add a new condition to a transition, hit the plus button next to the Conditions section.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.38.20.gif" alt=""><figcaption><p>Adding a new Condition</p></figcaption></figure>

Each new condition provides a dropdown showing all of the inputs you’ve added to the State Machine. The configuration options will be different depending on the input type you select.

Note that you can add multiple conditions to a single transition to create an “and” transition.

**Configuring a Boolean**

When you configure a boolean, you can decide if the transition happens when said boolean is either true or false.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.42.26.gif" alt=""><figcaption><p>Configure a boolean</p></figcaption></figure>

**Configuring a Number**

When you configure a number input, you can tell the transition to happen when a numerical condition happens such as equalling a specific number, being greater than or less than a specific number.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.45.30.gif" alt=""><figcaption><p>Configure number input</p></figcaption></figure>

**Configuring a Trigger**

When you add a trigger input to a transition, you are telling the transition to fire when that trigger occurs.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.49.06.gif" alt=""><figcaption><p>Configuring triggers</p></figcaption></figure>

### Interpolation

You can add interpolation to your transition at the bottom of the Transitions Panel. By default, the interpolation is set to linear, but you can use the cubic and hold interpolations.

Note that the interpolation between states is most effective when your transition duration is longer.

If you are unfamiliar with the basics of Interpolation, read more [here](transitions.md#interpolation).
