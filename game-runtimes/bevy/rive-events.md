---
description: Listen to Rive Events in Bevy
---

# Rive Events

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/rive-events/docX2iZ1A8EQ).
{% endhint %}

For more information on Rive Events see the respective [runtime](../../runtimes/rive-events.md) and [editor](../../editor/events.md) documentation.

## Subscribing to Events

Imports needed:

```rust
use rive_bevy::{GenericEvent, RivePlugin, SceneTarget, SpriteEntity, StateMachine};
use rive_rs::state_machine::Property;
```

The following code demonstrates a system listening to all Rive events reported from active state machines.

For example, let's take a look at a code snippet for a star-rating Rive file. If a reported event's name is **star,** the **Number** property is retrieved from the event data.

```rust
fn receive_rive_events_system(mut rive_event: EventReader<GenericEvent>) {
    for event in rive_event.read() {
        info!("Rive event: {:?}", event);
        // We can match on the event name and extract the properties.
        if event.name == "Star" {
            // Find the "rating" property which is a Property::Number.
            if let Some(Property::Number(rating)) = event.properties.get("rating") {
                info!("Rating: {:?}", rating);
            }
        }
    }
}
```

Other properties that can be read are **Bool** and **String**, with `Property::Bool` and `Property::String`.

### Additional Resources

* [Rive Bevy events example](https://github.com/rive-app/rive-bevy/blob/main/examples/rive-events.rs)
