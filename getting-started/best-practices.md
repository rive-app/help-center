---
description: Editor & runtime performance and usage considerations
---

# Best Practices

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/best-practices/docQqXohqHJa).
{% endhint %}

Rive is built to efficiently play interactive graphics in the editor and at runtime in applications and games. However, poorly optimized animations can consume significant resources and cause poor performance, particularly on low-end devices. In the following sections, we will outline important considerations and tips for maintaining optimal performance and minimal resource utilisation both during design/animate time in the Rive editor, as well as during runtime in applications.

{% hint style="info" %}
We recommend continuous testing of your animations on your target devices/platforms.
{% endhint %}

## Design-time Considerations

See below for some techniques to employ in the Rive editor to keep Rive performant:

### Raster Assets

Raster assets could have a big impact on runtime memory and file size.

When making use of images, you should aim to strike a balance between image quality, image dimension, and file size. Considerations include resizing images to fit the desired display area and choosing an appropriate compression level to reduce file size while maintaining acceptable visual quality.

#### Image Size/Dimension

It is important to ensure that the size of an image asset is appropriate for its usage. For instance, avoid using an image with large dimensions (e.g., 8192 x 7022) when it will be displayed in a small section of your artboard (e.g., sized 100 x 100).

Using large images can quickly consume device memory. This is particularly true on mobile devices where applications are more memory-constrained. Even if these images are compressed, their dimensions will still impact the amount of application memory used.

#### Compression

Compression involves reducing the file size by employing various algorithms to discard some image data. You can compress images directly from the Rive editor, which means you maintain the original image but compress the images for runtime use. If the asset is embedded in the animation, this will reduce the file size of the exported riv.

#### Other Considerations

* If you’re importing assets for reference only, ensure they are deleted before exporting the .riv file to ensure the asset doesn’t get bundled in.
* Depending on your target device/platform, it may make sense to have multiple different raster assets. For example, using smaller images when targeting for mobile vs. desktop. An upcoming feature, out-of-band assets, will make this process easier.

### Importing Lottie Files

Although Rive does offer a Lottie converter for easy export of `.riv` files, you will discover that re-implementing the graphics and animations directly in Rive can result in much smaller file sizes.

Working directly in Rive is generally preferable as it allows for file optimization based on your animation requirements. For instance, using bones and constraints to create rigs will result in fewer keys in the animation compared to converting directly from Lottie.

### Idle Animations

If you have idle animation states where the graphic remains static at a given state in a state machine, consider using a "one-shot" animation and ensuring that the timeline animation is not unnecessarily long. At runtime in a state machine, if there are no looping animations or active blend states playing, the runtime will preemptively “pause” itself until a state machine input or Rive Listener triggers the next transition in the state machine. This is useful because resource consumption (i.e. CPU usage) may decrease to the point of making Rive impact on resources negligent on applications.

Scenarios: Icons, buttons, graphics that only animate based on user interaction, etc.

### Using Solos

A [Solo](../editor/manipulating-shapes/solos.md) is similar to a group but with the added ability to toggle the rendering of nested objects. It functions like a radio button, deactivating other objects on the same level.

One common use case for Solos is creating different skins for a character that can be easily toggled on and off. Using Solos is much faster than individually animating the opacity of each object. Additionally, it allows the editor and runtimes to optimize your animation by not computing/rendering the deactivated Solos.

### Blend States

Similar to the guidance in [“Idle Animations”](best-practices.md#idle-animations), ensure blend states transition out to other states, or move to an exit state when finished if possible. When blend states are activated at runtime, Rive will continually play the state machine, even if it is not necessary anymore. Providing some transition away from the blend state when complete ensures Rive has one less “active” animation to track when considering whether to self-pause at any point while playing the state machine.

## Runtime Considerations

See below for some techniques to employ when using Rive runtimes in application to keep Rive performant:

### Pausing Programatically

There are several cases where you may want to pause a running animation/state machine configured with Rive programmatically. By pausing the Rive graphic at runtime, you may notice that Rive’s impact on the application has negligible resource consumption (i.e. CPU).

1. Rive graphic is offscreen
   1. If a Rive graphic is scrolled offscreen and does not need to continue playing, call the `pause` API on the respective runtime you’re using to prevent Rive from continuing to animate and consume resources when not needed
   2. Call the `play` API to continue playing Rive when the graphic needs to continue animating if back onscreen
2. Accessibility
   1. If a user has set in device settings that they prefer reduced motion, you may want to read this property at runtime and programmatically either call `pause` or set `autoplay: false` with the Rive runtime to ensure these users have reduced motion when navigating the application
3. State machine is idle with static graphics
   1. `Pause` when the Rive graphic is idling while it waits for a transition in the state machine from user interaction, data resolving, etc.

### Low-level runtime

Most runtimes have both a high-level API for ease of use and a more “advanced” low-level runtime counterpart, which allows you to have more granular control of how animations/state machines play (”advance”) over time.

Using low-level runtimes, you have the ability to load a Rive file (.riv) and manage its resources. With this pattern, you can dynamically create multiple instances of a given Artboard (and its associated state machine).

This is useful if you’re creating a whole “scene” with Rive content that draws to the same canvas/view and needs to efficiently manage how each entity advances over time. For example, if you render 100 instances of an artboard, such as a Zombie, you may want to control when and how each of these entities animates over time, as well as when it stops and removes itself from the scene, thus being more strategic about how much Rive utilizes in resources to draw each frame.

See runtime-specific documentation here:

* [JS/Web](../runtimes/overview/web-js/low-level-api-usage.md)
* [Flutter](https://help.rive.app/runtimes/overview/flutter/custom-painter)

### Low-end devices

Rive will try to run performantly across all browsers/devices, but if you’re able to, test how your application performs on resource-constrained devices with your specific Rive graphics running. You may find that for a given screen, Rive files that include heavily-animated graphics might be overkill for what is truly needed and decide to display static Rive graphics (i.e. autoplay: false) or reduce the amount of Rive entities animating at any given point.
