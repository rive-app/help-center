# Layers

A layer on the state machine allows you to play a single animation at a time. For this reason, you can create multiple layers if you wish to mix multiple animations or add additional interactions to a state machine.

This example uses layers to mix different background animations, and add multiple interactions onto a single artboard.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.19.55.gif" alt=""><figcaption><p>Using multiple Layers</p></figcaption></figure>

### Creating a new layer

To create a new layer, use the plus button on the Layers Tab.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.25.28.gif" alt=""><figcaption><p>Add a new Layer</p></figcaption></figure>

Notice that each new tab that you create comes with the Default States.

### Layer Order

It may not be obvious, but the order of your Layers matter with Layers to the right taking priority over the Layers to the left. In most cases, this won’t matter, but if your Layers have States that control the same object, the animations in the right most layer will take priority over the layers to the left as they mix.

**Changing layer order**

You can change the layer order by dragging and dropping your layers around on the Layers tab.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 16.26.29.gif" alt=""><figcaption><p>Move layers</p></figcaption></figure>
