---
description: Overview of Rive GameKit
---

# Overview

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/overview/dochMzQqsILp).
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=YwRWJX0OC34" %}
Rive GameKit announcement
{% endembed %}

## Why Rive?

Rive's familiar design and animation tools, coupled with our ground-breaking [State Machine](https://help.rive.app/editor/state-machine), enables game creators to build beautiful animations that are fully interactive at runtime.

Creators are already experimenting with Rive for game development, and popular game development solutions have implemented Rive within their engines, such as [NoesisGUI](https://rive.app/use-cases/rive-noesis) and [Defold](https://defold.com/extension-rive/).

Vector animations are excellent for GUIs, HUDs, and interfaces within games. Regardless of resolution or scale the assets will always look fantastic. With NoesisGUI, you can already add Rive to Unity, Unreal and other popular game engines.

<figure><img src="../.gitbook/assets/rive-noesis-cats.gif" alt=""><figcaption><p><em><strong>Create jaw dropping GUIs for your games (NoesisGUI &#x26; Rive)</strong></em></p></figcaption></figure>

<figure><img src="../.gitbook/assets/rive-noesis-unity.gif" alt=""><figcaption><p><strong>Create animated HUDs and other game interfaces, such as health bars (NoesisGUI &#x26; Rive)</strong></p></figcaption></figure>

## **Rive Renderer & GameKit**

The release of our new renderer and game kit enables you as a game creator to take Rive beyond simple game elements, such as GUIs or HUDs. You can now create full experiences where every single element on screen is a Rive animation.

<figure><img src="../.gitbook/assets/rive-gamekit-joel-demo.gif" alt=""><figcaption><p>Rive GameKit - Joel game demo</p></figcaption></figure>

The **Rive Renderer** can draw an unprecedented amount of vectors on screen. Allowing gameplay at 120fps, while every single scene element can animate without losing quality. Gameplay will remain crisp and clear regardless of scale, resolution or device.

The **Rive GameKit** is a package for Flutter to help you build highly performant games. The Renderer powers the drawing of all the vectors, while the GameKit allows you to:

* Efficiently control the animation loop of all Rive content in your scene.
* Interact with the animations through state machine inputs.
* Access and update individual components in the drawing hierarchy.
* Precisely layout your game scene.
* Tap into APIs that make game development easier (more to come on this!)

We believe this will enable game developers to create some truly unique experiences that are not possible anywhere else.

{% hint style="warning" %}
The Rive GameKit is currently in technical preview. The API is experimental and subject to change.
{% endhint %}

## Why Flutter?

Flutter is a popular framework for developing applications and provides a rich set of widgets and tools for building custom UI elements and animations, which allows you to create rich interfaces and experiences.

These capabilities also lends itself well to games that rely heavily on user interface design and customization. As a result, Flutter for game development is growing in popularity.

Flutter provides a lot of important development features out of the box, such as:

* Multi-platform support
* Hot reload / hot restart
* Asset management
* Great documentation and developer experience
* Active developer community
* Open source and free - no black boxes
* Easy to learn

It’ll also allow you to speed up your game's development with pre-built integrations for services like Ads, In-App Purchases, Firebase, Play Services, and Game Center. You can read more about [Flutter for games](https://flutter.dev/games) on their official website.

The popular mobile game, PUBG, [has integrated Flutter](https://flutter.dev/showcase/pubg-mobile) as a community module to allow players from all over the world to share gameplay clips.

The Rive GameKit enables you to use Flutter, with all its development benefits, bolted with a super fast vector renderer powered by Rive.

## Supported Platforms - Rive GameKit

At this time, only **iOS** and **macOS (Apple Silicon)** are supported.

In addition, you will need to be on macOS Ventura with **Xcode** **14.3** installed to run apps with GameKit locally at this time. This ensures you are on a clang version supported by the latest Rive GameKit binary release. Future versions may include support for earlier versions of Xcode and macOS.&#x20;

The Rive Renderer is written in C++ with various backends and is WebGL compatible, thanks to [@csmartdalton](https://twitter.com/csmartdalton)'s work on [Shader Pixel Local Storage](https://registry.khronos.org/webgl/extensions/WEBGL\_shader\_pixel\_local\_storage/).

Work is underway to enable it to work on Chrome, Android, and Windows.

## Contributing

The Rive GameKit is currently in technical preview and guided by community feedback and usage. The code is open source, and contributions are welcome!

If you have an issue or want to suggest a feature/API, let us know in our issues tab or reach out on our [Discord channel](https://discord.com/invite/FGjmaTr).
