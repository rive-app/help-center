---
description: Control the layout of your Rive animation in Unity
---

# Layout

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/layout/docFHk0CrU8G).
{% endhint %}

For more information on Rive Layout see the [runtime documentation](../../runtimes/layout.md).

## Fit and Alignment

The **fit** and **alignment** can be controlled on the **Rive.Renderer** `.Align()` method:

<pre class="language-csharp"><code class="lang-csharp">public Fit fit = Fit.contain;
public Alignment alignment = Alignment.Center;
public RenderTexture renderTexture;
private Rive.Renderer m_riveRenderer;

...
<strong>
</strong><strong>m_renderQueue = new Rive.RenderQueue(renderTexture);
</strong>m_riveRenderer = m_renderQueue.Renderer();
...

if (m_artboard != null &#x26;&#x26; renderTexture != null)
{
    m_riveRenderer.Align(fit, alignment, m_artboard);
    m_riveRenderer.Draw(m_artboard);
}
</code></pre>
