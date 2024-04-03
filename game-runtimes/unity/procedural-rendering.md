---
description: Procedurally draw shapes and paths in Unity using Rive
---

# Procedural Rendering

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/procedural-rendering/docF2fNqCP1W).
{% endhint %}

In this section, you’ll learn how to create custom paint and path objects. This enables you to perform custom computed draw commands using the Rive Renderer.

## Overview

The following classes are available:

* **BlendMode**: determines how the source pixels are blended with the destination pixels.
* **Color**: determines the color of the shape.
* **Gradient**: determines the color gradient of the shape (`LinearGradient` or `RadialGradient`)
* **Paint**: describes how to draw a shape - see [#paint](procedural-rendering.md#paint "mention").
* **PaintingStyle**: determines if the shape is filled or stroked.
* **Path**: defines a shape's outline or a clipping mask - see [#path](procedural-rendering.md#path "mention").
* **StrokeCap**: determines how the path's endpoints are drawn.
* **StrokeJoin**: the kind of finish to place on the joins between segments.

### RenderQueue

Create a `Rive.RenderQueue` and a `Rive.Renderer`:

```csharp
public RenderTexture renderTexture;
private Rive.RenderQueue m_renderQueue;
private Rive.Renderer m_riveRenderer;

...

m_renderQueue = new RenderQueue(renderTexture);
m_riveRenderer = m_renderQueue.Renderer();
```

Call `draw` on the renderer and pass in a `Path` and `Paint` object.

```csharp
m_path = new Path();
m_paint = new Paint();
m_riveRenderer.Draw(m_path, m_paint);
```

### Path

A path is a series of drawing commands. The path is used to define a shape's outline or a clipping mask.

Create a new path:

```csharp
m_path = new Path();
```

The `Path` class provides various methods to construct a path:&#x20;

* `moveTo`: Moves the current point to the given point.
* `lineTo`: Adds a straight line from the current point to the given point.
* `circle`: Adds a circle to the path.
* `cubicTo`: Adds a cubic bezier curve to the path.
* `quadTo`: Adds a quadratic bezier segment that curves from the current point.
* `addPath`: Adds the sub-paths of path to this path, transformed by the provided matrix.
* `close`: Closes the path. This will draw a line from the current point to the first point in the path.
* `reset`: Resets the path to an empty state.
* `flush`: to flush the path to native memory.

### Paint

Paint is used to describe how to draw a shape.&#x20;

Create a new paint:

```csharp
m_paint = new Paint();
```

The paint describes the color, gradient, style, thickness, blend mode, stroke cap, and stroke join for a shape.

Call `.Flush()` to flush the paint to native memory. See the [#example](procedural-rendering.md#example "mention")below.

## Example

This example demonstrates drawing an animated triangle to a [RenderTexture](https://docs.unity3d.com/ScriptReference/RenderTexture.html).

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-08 at 11.14.14.gif" alt=""><figcaption><p>Rive Unity: Procedural Rendering</p></figcaption></figure>

The `MonoBehaviour` to create the above:

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.Rendering;
using UnityEditor;
using Rive;

public class RiveProcedural : MonoBehaviour
{
    public RenderTexture renderTexture;
    private Rive.RenderQueue m_renderQueue;
    private Rive.Renderer m_riveRenderer;
    private CommandBuffer m_commandBuffer;

    private Camera m_camera;

    Path m_path;
    Paint m_paint;
    private void Start()
    {
        m_renderQueue = new RenderQueue(renderTexture);
        m_riveRenderer = m_renderQueue.Renderer();
        m_path = new Path();
        m_paint = new Paint();
        m_paint.Color = new Rive.Color(0xFFFF0000);
        m_paint.Style = PaintingStyle.stroke;
        m_paint.Join = StrokeJoin.round;
        m_paint.Thickness = 20.0f;
        m_riveRenderer.Draw(m_path, m_paint);

        m_commandBuffer = new CommandBuffer();
        m_commandBuffer.SetRenderTarget(renderTexture);
        m_riveRenderer.AddToCommandBuffer(m_commandBuffer);
        m_camera = Camera.main;
        if (m_camera != null)
        {
            Camera.main.AddCommandBuffer(CameraEvent.AfterEverything, m_commandBuffer);
        }
    }


    private void Update()
    {
        if (m_path == null)
        {
            return;
        }
        m_path.Reset();

        float expand = Time.fixedTime * 10;
        m_path.MoveTo(256, 256 - 100 - expand);
        m_path.LineTo(256 + 50 + expand, 256 + 50 + expand);
        m_path.LineTo(256 - 50 - expand, 256 + 50 + expand);
        m_path.Close();
        m_path.Flush();


        m_paint.Thickness = (Mathf.Sin(Time.fixedTime * Mathf.PI * 2) + 1.0f) * 20.0f + 1.0f;
        m_paint.Flush();
    }

    private void OnDisable()
    {
        if (m_camera != null && m_commandBuffer != null)
        {
            m_camera.RemoveCommandBuffer(CameraEvent.AfterEverything, m_commandBuffer);
        }
    }
}
```

### Additional Resources

See the **getting-started** project in the examples repository for a complete example and open the **ProceduralRenderingScene** scene.
