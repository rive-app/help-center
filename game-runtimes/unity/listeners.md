---
description: Enable listeners on your Rive animation in Unity
---

# Listeners

For more information on Rive Listeners see the [editor documentation](../../editor/state-machine/listeners.md).

## Pointer Positions

In rive-unity pointer events can be passed to an artboard to enable Rive Listeners. The following code snippet demonstrates how to translate the mouse position to an artboard's local coordinate.

For a complete example see the **getting-started** project in the [examples repository](https://github.com/rive-app/rive-unity-examples) and open the scene: **DrawToCameraScene**.

```csharp
private Artboard m_artboard;
private StateMachine m_stateMachine;

...

Camera camera = gameObject.GetComponent<Camera>();
if (camera != null)
{
    Vector3 mousePos = camera.ScreenToViewportPoint(Input.mousePosition);
    Vector2 mouseRiveScreenPos = new Vector2(
        mousePos.x * camera.pixelWidth,
        (1 - mousePos.y) * camera.pixelHeight
    );
    if (m_artboard != null && m_lastMousePosition != mouseRiveScreenPos)
    {
        Vector2 local = m_artboard.localCoordinate(
            mouseRiveScreenPos,
            new Rect(0, 0, camera.pixelWidth, camera.pixelHeight),
            fit,
            alignment
        );
        m_stateMachine?.pointerMove(local);
        m_lastMousePosition = mouseRiveScreenPos;
    }
    if (Input.GetMouseButtonDown(0))
    {
        Vector2 local = m_artboard.localCoordinate(
            mouseRiveScreenPos,
            new Rect(0, 0, camera.pixelWidth, camera.pixelHeight),
            fit,
            alignment
        );
        m_stateMachine?.pointerDown(local);
        m_wasMouseDown = true;
    }
    else if (m_wasMouseDown)
    {
        m_wasMouseDown = false;
        Vector2 local = m_artboard.localCoordinate(
            mouseRiveScreenPos,
            new Rect(0, 0, camera.pixelWidth, camera.pixelHeight),
            fit,
            alignment
        );
        m_stateMachine?.pointerUp(local);
    }
}
```
