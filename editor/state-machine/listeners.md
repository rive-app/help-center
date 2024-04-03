# Listeners

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/listeners/doceaiA9rRW1).
{% endhint %}

Listeners give designers the tools to take their State Machine one step further and define click, hover, and mouse move actions that can change inputs in the editor and at runtime without the need for a developer.

For example, this button does not have listeners attached to it. If we click on the button, nothing happens. We would need to change the inputs directly in the input panel to make our button react.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.53.07.gif" alt=""><figcaption><p>Button without Listener</p></figcaption></figure>

Once we add a listener, you’ll notice that now when we click on the button, it reacts exactly like we would expect it to in our website, app, or game.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.55.55.gif" alt=""><figcaption><p>Button with a Listener</p></figcaption></figure>

Note that Listeners are not required for any State Machine. Once you’ve gone through the basic set-up and ensured that your interaction works as expected, it can be exported and used at runtime. While listeners do allow designers to enable some interactions, more complex interactions will need to be defined at runtime.

#### **Creating a new Listener**

To create a new Listener, use the plus button in the Listeners tab. Once you’ve created a Listener, select the Listener and configure it using the inspector. Read more about configuring a Listener below.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 15.59.17.gif" alt=""><figcaption><p>Creating a new Listener</p></figcaption></figure>

## **Anatomy of a Listener**

A listener consists of three parts; a target, a user action, and a listener action. The target tells the listener where on the artboard it should be listening for a users action. Once the defined action happens within the target area, the listener reacts by either changing an input value, or aligning a target with the mouse cursor. Continue reading below for more information.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.00.32@2x.png" alt=""><figcaption><p>Listener properties</p></figcaption></figure>

### Target

Listeners require us to define an area where it will listen for an action. If you are familiar with game development, then our target is a hitbox that we define on the artbord.

There are two ways to define a target.

If you have an object selected when creating a listener, then the object you have selected will automatically be designated as the target.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.07.28.gif" alt=""><figcaption><p>Automatically add target</p></figcaption></figure>

Alternatively, you can select hit the target button with the listener selected and either select an object in the Hierarchy or directly on the Stage.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.09.11.gif" alt=""><figcaption><p>Manually add target</p></figcaption></figure>

**Considerations**

You have the ability to set most objects as the target of a Listener but the target will work differently depending on the type of object you target.

As a best practice, we typically recommend using shapes that are specifically meant to be targets, I.E. an ellipse or rectangle with it’s opacity set to 0%. Using shapes as targets give you more control over over when that target is available and specifically, how much of the artboard it covers.

If you do decide to select a Group as a target, shapes within the group will act as the target.

### User Action

Actions are specifically which actions the listener is listening for. By default, your listener is set to the Pointer Enter action, but you can change it by using the dropdown provided in the Inspector.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.13.42@2x.png" alt=""><figcaption></figcaption></figure>

There are five different listener actions.

**Pointer Down** - Either a mouse click down, or finger press (on touch screen device).

**Pointer Up** - Releasing a mouse click or finger press.

**Pointer Enter** - A mouse or finger entering the target area.

**Pointer Exit** - A mouse of finger exiting the target area.

**Mouse move** - Any mouse of finger movement within the target area.

### **Listener Action**

A listener actions is what the listener does when the defined user interaction happens within the target area. There are two types of Listener Actions; Input change and Align target. To add a Listener action, click the plus button next to the User Action dropdown. Keep in mind that you can add as many Listener Actions as you’d like.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.16.38.gif" alt=""><figcaption></figcaption></figure>

**Input Change**

An input change allows the listener to change a defined input. For example, we can change a boolean value, fire a trigger, or set a number value to a specific integer. This is how we can, for example, create hovers and clicks that work directly on the Artboard.

Keep in mind that when you select an Input Change as a Listener action, you’ll be prompted to define how you want to change the selected input.

**Align Target**

The Align Target action will align a target with the mouse cursor when the defined user action happens within the target area. With this action, we can create powerful parallax effects and avatars that follow the mouse cursor.

When adding an Align target, you’ll need to specify which object you want aligned.
