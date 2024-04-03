---
description: Read and update Rive Text
---

# Text

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/text/docOxY8YTQ5T).
{% endhint %}

For more information on Rive Text see the respective [runtime](../../runtimes/text.md) and [editor](../../editor/text/) documentation.

## Read/Update Text Runs at Runtime

{% hint style="info" %}
A unique run name must be set in the editor for it to be easily discoverable at runtime. See the [runtime](../../runtimes/text.md) docs for more information.
{% endhint %}

Imports needed:

```rust
use rive_bevy::{
    events::{self, InputValue},
    RivePlugin, RiveStateMachine, SceneTarget, SpriteEntity, StateMachine,
};
use rive_rs::components::TextValueRun;
```

The following code retrieves the active `Artboard` from a state machine and finds the `TextValueRun` component by name.

```rust
fn update_rive_text_system(kbd: Res<Input<KeyCode>>, query: Query<&mut RiveStateMachine>) {
    if kbd.just_pressed(KeyCode::Return) {
        match query.get_single() {
            Ok(state_machine) => {
                // Access the Rive artboard from the State Machine
                let mut artboard = state_machine.artboard();

                // Find the text run component by name by iterating over all artboard `Components`
                let mut text: TextValueRun = artboard
                    .components()
                    .find(|comp| comp.name() == "Sector") // name of the text run
                    .unwrap()
                    .try_into()
                    .unwrap();

                info!("current text value: {:?}", text.get_text());

                text.set_text("Hello World");

                info!("updated text value: {:?}", text.get_text());
            }
            Err(_) => {}
        }
    }
}
```

**Note** that finding the component by name can be an expensive operation if the artboard has many components.

If the text run needs to be accessed frequently you can store the index value and access it directly.&#x20;

For example:

```rust
let text_run_index: usize = artboard
    .components()
    .into_iter()
    .position(|comp| comp.name() == "Sector")
    .unwrap();
```

Then store `text_run_index` and later access the component by index directly:

```rust
let mut text: TextValueRun = artboard
    .components()
    .nth(text_run_index)
    .unwrap()
    .try_into()
    .unwrap();
```

### Additional Resources

* [Rive Bevy inputs example](https://github.com/rive-app/rive-bevy/blob/main/examples/rive-input.rs)
