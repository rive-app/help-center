---
description: Noesis support for Rive
---

# Noesis

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/noesis/docmj3dhOsBF).
{% endhint %}

## Overview

This guide documents how to get started using Rive in [Noesis](https://www.noesisengine.com/index.php). Noesis is a UI middleware for game engines and real-time applications. As of [NoesisGUI v3.2](https://www.noesisengine.com/docs/Gui.Core.Changelog.html#version-3-2-0rc2), you can use Rive assets with NoesisGUI and therefore can render Rives in game engines that support Noesis such as Unity and Unreal, as well as C# and C++ standalone applications.

See our [use cases page](https://rive.app/use-cases/rive-noesis) for an overview of tutorials and assets.

Noesis Rive usage docs: [https://www.noesisengine.com/docs/3.2/App.RiveControl.\_RiveControl.html](https://www.noesisengine.com/docs/3.2/App.RiveControl.\_RiveControl.html)

## Getting Started

### First time using Noesis

With NoesisGUI, you can build UI in [Blend for Visual Studio](https://learn.microsoft.com/en-us/visualstudio/xaml-tools/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2022) (Windows only) that generates XAML code. When you install the NoesisGUI plugin/package in respective game engines, Noesis will parse through the XAML code to appropriately render UI onto cameras, textures, 3D objects, and more.\
\
We **strongly** encourage you to check out [Noesis docs](https://www.noesisengine.com/docs/Gui.Core.Index.html) before diving straight into using Noesis for your native applications or games.

### Using Rive in Noesis

If you are familiar with getting NoesisGUI running in your applications/games, you can start to use the `<RiveControl>` element in your XAML to add Rives and control state machines!\


Ensure you have installed the latest `Noesis.GUI` and `Noesis.GUI.Extensions` packages from the NuGet package manager to ensure you can add `RiveControl` elements to your stage/XAML. There are currently three XAML elements you can add:

* `<RiveControl>` - Responsible for specifying the Rive file for Noesis to render. Noesis will find the default Artboard on the Rive file and the default State Machine associated with that artboard to run automatically
* `<RiveInput>` - A reference to a State Machine boolean or number input that you can nest within RiveControl
* `<RiveTriggerAction>` - A reference to a State Machine trigger input
* `<RiveRun>` - A reference to a text run by which you can bind to other values to retrieve or set text values&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-17 142704.png" alt=""><figcaption><p>Rive elements available in Blend</p></figcaption></figure>

{% hint style="info" %}
Rive files will not render directly in Blend, so you may see a blue background/overlay for these elements in the stage area. To see Rives render, run in the XamlPlayer from Noesis or within the respective game engines you're running Noesis in.
{% endhint %}

To render Rive in your UI, Add a `<RiveControl>` with a `Source` property to specify the `.riv` asset bundled in with your application. In the example below, we run a Rive file called `game_health.riv` which displays the default Artboard and State Machine while also assigning a reference name `x:name` for this element.&#x20;

```xml
<noesis:RiveControl x:Name="Health" Source="game_health.riv" />
```

To control a State Machine via inputs, we can nest a `<RiveInput>` where we specify the input's State Machine name and value via `InputName` and `InputValue` properties.

```xml
<noesis:RiveControl x:Name="Health" Source="game_health.riv">
    <noesis:RiveInput InputName="health" InputValue="{Binding Health}" />
</noesis:RiveControl>
```

In the above `<RiveInput>` we bind the input value to a property called `Health`, which you can set via code elsewhere or set the value directly. This is where you use the power of Blend and XAML to define how to data-bind to interactions or other properties. In the example above, you can follow the tutorial video at the end of the page to see how we data-bind this specific property.

{% hint style="warning" %}
**Note:** As of NoesisGUI 3.2, if you intend to use multiple Artboards from the same file, you should break up the Artboards into their own `.riv` file, as NoesisGUI will only play the default artboard. Future functionality may introduce a property to specify the Artboard you want to display from a given Rive file.
{% endhint %}

Save your XAML file and Blend project and load it in the respective game engine/native application you're working in as recommended by Noesis.

## Unity Tutorial

See the following video to follow a tutorial on using Noesis and Rive with an FPS shooter game in Unity. Below you will also find the source code project so you can run it yourself.

{% embed url="https://youtu.be/F6Q4pfnBhbI" %}
Using Rive assets in Unity through NoesisGUI
{% endembed %}

Source code for this project can be found [here](https://github.com/zplata/rive-noesis-unity-fps-demo).

## Unreal Tutorial

See the following video to follow a tutorial on using Noesis and Rive with a scene in Unreal, built by the Noesis team.

{% embed url="https://youtu.be/WuMHGPOINSo" %}
Using Rive assets in Unreal through NoesisGUI
{% endembed %}
