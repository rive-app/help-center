---
description: Control the layout of your Rive animation in Unity
---

# Layout

For more information on Rive Layout see the [runtime documentation](../../runtimes/layout.md).

## Fit and Alignment

The **fit** and **alignment** can be controlled on the **RenderQueue** `align` method:

<pre class="language-csharp"><code class="lang-csharp">public Fit fit = Fit.contain;
public Alignment alignment = Alignment.center;
public RenderTexture renderTexture;

...
<strong>
</strong><strong>m_renderQueue = new RenderQueue(renderTexture);
</strong>
...

if (m_artboard != null &#x26;&#x26; renderTexture != null)
{
    m_renderQueue.align(fit, alignment, m_artboard);
    m_renderQueue.draw(m_artboard);
}
</code></pre>
