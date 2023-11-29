---
description: Unity runtime for Rive
---

# Unity

{% hint style="warning" %}
The Rive Unity runtime is currently in **Technical Review** for Mac and Windows installs of Unity. We're hoping to gather feedback about the API and feature-set as we expand platform support. Please reach out to us on [Discord](https://discord.com/invite/FGjmaTr) or through our [Support Channel](https://rive.atlassian.net/servicedesk/customer/portals).
{% endhint %}

{% hint style="warning" %}
Not all Rive features are yet supported in the rive-unity runtime, see [#feature-support](./#feature-support "mention")
{% endhint %}

## Rendering Support

The rive-unity runtime uses the [Rive Renderer](https://rive.app/renderer) and is up to date with the latest C++ runtime version of Rive.

* Metal on Mac and iOS
* D3D11 on Windows
* OpenGL on Windows

Planned support for:

* D3D12
* WebGL
* Vulkan

### Bug Reports

If you encounter any errors or unexpected crashes while integrating the Rive Unity runtime, we recommend logging a detailed issue directly to the [rive-unity](https://github.com/rive-app/rive-unity/issues) repo with an **Editor.log** attached to the issue to help provide more details and context about what might have occurred.

You can find more details on where to find your Editor.log file in the [Unity docs](https://docs.unity3d.com/Manual/LogFiles.html).

{% hint style="info" %}
Note that it is best to grab the Editor.log file immediately after a crash has occurred
{% endhint %}

## Feature Support

The rive-unity runtime uses the latest Rive C++ runtime. For more details on runtime support, see the [Runtime Feature Support](https://www.notion.so/o/-LLf9WNWru58qo4lWjp9/s/-M3EXlibk6bj2FzPQW-9/\~/changes/387/runtimes/feature-support) page. Refer to the following table for what is currently supported at runtime.

<table><thead><tr><th width="541">Feature</th><th>Supported</th></tr></thead><tbody><tr><td><a href="../../runtimes/playback.md">Animation Playback</a></td><td>✅</td></tr><tr><td><a href="../../runtimes/layout.md">Fit and Alignment</a></td><td>✅</td></tr><tr><td><a href="../../editor/state-machine/listeners.md">Listeners</a></td><td>✅</td></tr><tr><td><a href="../../runtimes/state-machines.md">Setting State Machine Inputs</a></td><td>✅</td></tr><tr><td><a href="../../runtimes/rive-events.md">Listening to Events</a></td><td>✅</td></tr><tr><td><a href="../../runtimes/text.md">Updating text at runtime</a></td><td>Coming Soon</td></tr><tr><td><a href="../../runtimes/loading-assets.md">Out-of-band assets</a></td><td>Coming soon</td></tr><tr><td>Procedural rendering</td><td>Coming Soon</td></tr></tbody></table>
