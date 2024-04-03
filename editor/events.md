# Events

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/events/docREl0GMIeb).
{% endhint %}

Events are a way to send signals to your runtime code to execute a block of code at the right moment. They enhance the communication between designers and developers by passing along useful information. With them, we can do things like go to a URL, play a sound, have some HTML  appear, or do anything else we may want to accomplish via code.

Coordinating Rive events at design time and runtime will be important to ensure a successful integration in apps, games, and more.

## Creating an Event

To create an Event, use the Events tool located in the Toolbar. Once the tool is active, click anywhere on the artboard to add a new event.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 12.49.11.gif" alt=""><figcaption><p>Adding a new event</p></figcaption></figure>

You'll notice that the Event is displayed on the artboard and in the Hierarchy.

## Configuring an Event

Once we've added an event, we need to configure the Event using the Inspector.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 12.52.15@2x.png" alt=""><figcaption><p>Inspector view for the Event</p></figcaption></figure>

### **Name**&#x20;

The Name field is where we can give our Event a specific name. It's important to do this so that at runtime, we can hook up the correct bits of code to their corresponding events.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 12.53.18.gif" alt=""><figcaption><p>Renaming an Event</p></figcaption></figure>

You can also rename the Event directly on the artboard.

### **Type**

The Type dropdown allows you to change the Event type between General and URL.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 12.56.27.gif" alt=""><figcaption></figcaption></figure>

### **Properties**

Properties allow us to define extra information being passed to the Runtimes. [For example, you may want to pass in the name of an audio file to play when the event is reported at runtime, or perhaps some other metadata for data analytics purposes.](#user-content-fn-1)[^1]

To add a new property, hit the plus button next to the Properties.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 12.58.19.gif" alt=""><figcaption><p>Add New Property</p></figcaption></figure>



First, we want to change the name of our property to something identifiable. Next, we need to select what type of value our property will track such as a Number, Boolean, or String.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 13.00.38.gif" alt=""><figcaption><p>Rename and select input</p></figcaption></figure>



### **URL**

When an `Open URL` Event is selected, we have additional configuration options.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 13.06.14@2x.png" alt=""><figcaption><p>Properties for Open URL Event</p></figcaption></figure>

In the text box, we will add the URL that we want our component to take us.&#x20;

{% hint style="info" %}
Make sure to include the URL Protocol (i.e. "http://" or "https://") if you want to link to a new domain.
{% endhint %}



The Target tells the user's browser where this URL should open. We have a few options: Blank, Parent, Self, and Top.

* `Blank` - Usually opens the link in a new tab, but users can configure browsers to open a new window instead. **Note:** if the Event is not signaled from a Rive Listener, the browser may block and notify the user a popup was blocked
* `Parent` - Opens in the parent browsing context of the current one. If no parent, behaves as `Self`.
* Self - Opens in the current browsing context
* Top - Opens in the topmost browsing context (the "highest" context that's an ancestor of the current one). If no ancestors, behaves as `Self`.

{% hint style="warning" %}
At this time, by default, Rive will not open URLs when this type of event is reported in share links or community posts due to security considerations. However, this may change in the future
{% endhint %}

## Signaling an Event

We can signal an event in three ways: via the timeline, on a state, or on a transition.



### Timeline

Signaling an event through the timeline allows us to control the precise moment a piece of code will fire, like a sound effect.



First, select the timeline you want to add the event to. Next, use the Report Event button in the inspector. Note that this key will be placed at the location of the playhead.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 13.08.09.gif" alt=""><figcaption><p>Keying an Event on the timeline</p></figcaption></figure>

Additionally, you can key a property to let the runtimes know that a particular boolean, number, or string property has a new value.



### **Transition & State**

You can report an event on a Transition or a State. We typically do this to signal at runtime contextual information about what is happening in the State Machine. For example, if we want to have some element appear at the end of our Transition, we would want to use an event tied to a Transition to signal this.

To report an event, select either the desired State or Transition and use the plus button next to the Events section in the Inspector.

<figure><img src="../.gitbook/assets/CleanShot 2023-09-18 at 13.13.02.gif" alt=""><figcaption><p>Signaling an Event via State or Transition</p></figcaption></figure>

The dropdown allows us to select any Event we've defined. Now that we've selected the Event, we can decide whether it is signaled at the start or end of the Transition or State.

## Events at Runtime

For more information on how to listen for Events at runtime, check out the [Rive Events](../runtimes/rive-events.md) section.

[^1]: 
