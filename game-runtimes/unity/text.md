---
description: Update Rive text at runtime in Unity
---

# Text

For more information on Rive Text see the respective [runtime](../../runtimes/text.md) and [editor](../../editor/text/) documentation.

## Update Rive Text in Unity

{% hint style="info" %}
A unique run name must be set in the editor to be easily discoverable at runtime. See the text [runtime](../../runtimes/text.md) docs for more information.
{% endhint %}

A text run can be updated from an artboard instance by providing the **name** and **new value**:

```csharp
Artboard artboard;

....

artboard.SetTextRun("textRunName", "newValue");
```

{% hint style="warning" %}
**Note:** This API only updates text runs on the given artboard, and will not update text runs on nested artboards.
{% endhint %}

{% hint style="warning" %}
An expanded API is planned to access a text run instance and to increase the discoverability of text runs.
{% endhint %}
