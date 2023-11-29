---
description: Access Rive Events in Unity
---

# Rive Events

For more information on Rive Events see the respective [runtime](../../runtimes/rive-events.md) and [editor](../../editor/events.md) documentation.

## Accessing Events

The following code demonstrates accessing all Rive events reported from an active state machine.

```csharp
...

foreach (var report in m_stateMachine?.reportedEvents() ?? Enumerable.Empty<ReportedEvent>())
{
    Debug.Log($"Event received, name: \"{reportedEvent.name}\", secondsDelay: {reportedEvent.secondsDelay}");
}

// Important! Call `advance` after accessing events.
m_stateMachine?.advance(Time.deltaTime);
```

The method `reportedEvents()` returns a list of `ReportedEvent`s.

Let's look at a code snippet for a star-rating Rive file. If a reported event's name is **Star,** the **rating** property of type `float` and a **message** of type `string` are retrieved from the event data (custom properties).

```csharp
private void RiveScreen_OnRiveEvent(ReportedEvent reportedEvent)
{
    Debug.Log($"Event received, name: \"{reportedEvent.name}\", secondsDelay: {reportedEvent.secondsDelay}");
    
    // Access specific event properties
    if (reportedEvent.name == "Star")
    {
        var rating = reportedEvent["rating"] as float?;
        var message = reportedEvent["message"] as string;
        Debug.Log($"Rating: {rating}");
        Debug.Log($"Message: {message}");
    }
}
```

* Properties that can be read are **bool**, **string**, and **float**.
* Access a dictionary of all properties with: `reportedEvent.properties`.

### Additional Resources

For a complete example see the **getting-started** project in the [examples repository](https://github.com/rive-app/rive-unity-examples) and open the **EventsScene** scene.
