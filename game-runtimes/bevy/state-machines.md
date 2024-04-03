---
description: Interact with the Rive State Machine in Bevy
---

# State Machines

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/state-machines/docKIZctc54e).
{% endhint %}

For more information on Rive State Machines see the respective [runtime](../../runtimes/state-machines.md) and [editor](../../editor/state-machine/) documentation.

## Reading/Writing Inputs

Imports needed:

```rust
use rive_bevy::{
    events::{self, InputValue},
    RivePlugin, RiveStateMachine, SceneTarget, SpriteEntity, StateMachine,
};
```

The following code demonstrates reading and updating a **Boolean** state machine input.

**Number** and **Trigger** inputs can be accessed using `get_number` and `get_trigger` respectively.

```rust
fn update_state_machine_system(
    kbd: Res<Input<KeyCode>>,
    query: Query<(Entity, &mut RiveStateMachine)>,
) {
    if kbd.just_pressed(KeyCode::Return) {
        // Get the State Machine and its Entity
        let (_, state_machine) = query.get_single().expect("State machine not found");

        // Read the current value of an input
        let center_hover_current = state_machine.get_bool("centerHover").unwrap().get();

        // Update the input
        state_machine
            .get_bool("centerHover")
            .unwrap()
            .set(!center_hover_current);
    }
}
```

Alternatively, you can update an input using Bevy's [EventWriter](https://docs.rs/bevy/latest/bevy/prelude/struct.EventWriter.html):

```rust
fn update_state_machine_system(
    kbd: Res<Input<KeyCode>>,
    query: Query<(Entity, &RiveStateMachine)>,
    mut input_events: EventWriter<events::Input>,
) {
    if kbd.just_pressed(KeyCode::Return) {
        // Get the State Machine and its Entity
        let (entity, _) = query.get_single().expect("State machine not found");

        // Send a new value to the input using Bevy events.
        input_events.send(events::Input {
            state_machine: entity,
            name: "shoot".into(), // name of the input
            value: events::InputValue::Trigger,
        });
    }
}
```

### Additional Resources

* [Rive Bevy inputs example](https://github.com/rive-app/rive-bevy/blob/main/examples/rive-input.rs)
