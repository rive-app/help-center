# Nested Artboards

Nested Artboards can speed up your workflow by allowing you to reuse animations and State Machines. Any changes made to the source Artboard will be reflected across any of the Nested Artboards



## **Creating a Nested Artboard**

To add a new Nested Artboard, use the Nested Artboard tool which you can find in the Artboards menu, or by using the shortcut N. Once the tool is activated, click anywhere on the desired artboard.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-25 at 15.07.38.gif" alt=""><figcaption><p>Adding a Nested Artboard</p></figcaption></figure>

You’ll notice at first that your Nested Artboard appears as a group. You’ll need to assign an artboard to it using the inspector before it will appear.



## **Configuring a Nested Artboard**

Once you’ve added a Nested Artboard, there are a number of ways to playback various animations and State Machines.

### **State Machines**

After assigning an artboard to a Nested Artboard, you’ll notice that the default state machine for that artboard is displayed in the Inspector.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-25 at 15.16.53.gif" alt=""><figcaption><p>By default, the State Machine is added</p></figcaption></figure>

If you’ve exposed any Inputs, you can access them using the options menu (when an animation is selected) or in the Inputs Panel (when a state machine is selected).

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-25 at 15.18.02.gif" alt=""><figcaption><p>Exposed Inputs appear in the Inspector and Inpust panel</p></figcaption></figure>

### **Adding an animation**

You can playback any animation associated with a Nested Artboard. You’ll need to add the desired animation to the Nested Artboard using the plus button in the Inspector.

<figure><img src="../../.gitbook/assets/2023-10-25 15.39.25.gif" alt=""><figcaption></figcaption></figure>

These animations can be used by themselves, mixed with it’s state machine, or other animations.

Note that before adding the animation, you’ll need to select where the animation is a Simple or Remap animation. We’ll discuss the differences below.

#### **Simple**

Simple animations are an easy way to enable an animation playback on a Nested Artboard.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-25 at 15.40.24.gif" alt=""><figcaption><p>Simple Animation</p></figcaption></figure>

A Simple animation lets you key when an animation begins playing on a timeline. You also have the option to change the animation's playback speed.

#### **Remap**

Remap animations allow you to key time values of an animation on the timeline. This let’s you stretch, shrink, or even play an animation in reverse.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-25 at 15.58.20.gif" alt=""><figcaption></figcaption></figure>

Note that the time value is in percent, with 0% representing the start of the timeline and 100% representing the end.

### **Mix value**

As you add additional animations to a nested artboard, animations begin to mix together. This mixing is important, especially when multiple animations share keyed properties. Without adjusting this value, your nested artboard may not playback your animations in the way you want.

By default, any animation added to a nested artboard starts with a mix value of 100%. You can adjust this value in design mode, or in a specific animation by setting keys. **Note that an animation that has a non-zero mix value will always be mixing with other animations, regardless if it has a play key set or not.**

To ensure the correct animation is playing, ensure that you key the mix value for the desired animation to 100% and all other animations have a mix of 0%.

## **Exposing Inputs and Events**

If you intend to use a Nested Artboard as a component, you should expose inputs that the main artboard can see. This allows you to control Nested Artboards with Other Nested Artboards using a main State Machine.

<figure><img src="../../.gitbook/assets/2023-10-26 12.25.22.gif" alt=""><figcaption></figcaption></figure>

### **How to Expose an Input**

Exposing an Input allows the parent artboard to access and manipulate it. To do this, select the desired input, then check the expose to main artboard option in the inspector.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-26 at 11.03.53.gif" alt=""><figcaption><p>Exposing an Input</p></figcaption></figure>

After creating a nested artboard, you’ll see any exposed inputs in the Inspector via the options panel and in the Inputs panel.



### **Using input on a parent artboard**

Exposed inputs can found in the Inputs panel, or in the inspector. You can use them through a listeners, an event, or by keying them on a timeline.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-26 at 11.12.06.gif" alt=""><figcaption></figcaption></figure>

#### **Via a listener**

When you create a listener, you’ll find all exposed Inputs as a set input property of a Listener. This option lets you, for example, change the boolean input of multiple artboards simultaneously.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-26 at 11.20.34.gif" alt=""><figcaption></figcaption></figure>

**Using Events**

Additionally, we can use listeners to listen for Events firing in our Nested Artboards and change inputs based on those.

<figure><img src="../../.gitbook/assets/CleanShot 2023-10-26 at 11.24.45.gif" alt=""><figcaption><p>Using Events</p></figcaption></figure>

To see an Event associated with an artboard, you’ll need to set the Artboard as a target of the listener. The Event will now be listed as a listener action.

#### **Keying on the State Machine**

You can key exposed inputs on the parent artboard via the options panel in the inspector.

This is a handy trick when you, for example, want to set the text value of a nested artboard.



