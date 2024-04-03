---
description: Bevy runtime for Rive
---

# Bevy

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/bevy/docYfczHUQLr).
{% endhint %}

The Bevy runtime is available here: [https://github.com/rive-app/rive-bevy/](https://github.com/rive-app/rive-bevy/)

{% hint style="warning" %}
This runtime uses [Vello](https://github.com/linebender/vello) as a render back-end, which has certain limitations. Refer to [Known Issues](./#known-issues) for details. Efforts are underway to incorporate the [Rive Renderer](https://rive.app/renderer) as another back-end.
{% endhint %}

## Known Issues

The existing [Vello](https://github.com/linebender/vello) render back-end may lead to some inconsistencies in comparison to the original design:

* Image meshes: They can exhibit small inconsistencies at triangle borders, there can be gaps between triangles, transparent meshes will overdraw at triangle borders.
* Very high number of clips: Vello is currently rendering very high numbers of clips incorrectly.
* All strokes will have round joins and caps.

Efforts are being made to make the [Rive Renderer](https://rive.app/renderer) available. You'll then have the choice to select your preferred renderer.
