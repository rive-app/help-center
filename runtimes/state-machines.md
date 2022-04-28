---
description: Playing and changing inputs in state machines
---

# State Machines

For more information on designing and building state machines in Rive, please refer to the [editor's state machine section](https://app.gitbook.com/@rive/s/rive-help-center/editor/animate-mode/state-machine).

Rive's state machines provide a way to combine a set of animations and manage the transition between them through a series of inputs that can be programmatically controlled. Once a state machine is instantiated and playing, transitioning states can be accomplished by changing boolean or double value inputs, or firing triggers. The effects of these will be dependent on how the state machine has been configured in the editor.

## Playing state machines

State machines are instantiated in much the same manner as animations: provide the state machine name to the Rive object:

{% tabs %}
{% tab title="Web" %}
```javascript
const r = new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    autoplay: true,
    stateMachines: 'bumpy',
    fit: rive.Fit.cover,
});
```
{% endtab %}

{% tab title="React" %}
```javascript
// State Machine require the useRive hook.
export default function Simple() {
  const { RiveComponent } = useRive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    stateMachines: "weather",
    autoplay: true,
  });

  return <RiveComponent />;
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    fit: BoxFit.cover,
    stateMachines: ['bumpy'],
);
```
{% endtab %}

{% tab title="Angular" %}
```markup
<canvas riv="vehicles" width="500" height="500" fit="cover">
  <riv-state-machine name="bumpy" play></riv-state-machine>
</canvas>
```
{% endtab %}
{% endtabs %}

Once the Rive file is loaded and instantiated, the state machine can be queried for inputs, and these inputs can then be read from and written to, and in the case of triggers, fired.

{% tabs %}
{% tab title="Web" %}
The web runtime provides an `onLoad` callback that's run when the Rive file is loaded and ready for use. We use this here to ensure that the state machine is instantiated when we query for inputs.

```markup
<div id="button">
    <canvas id="canvas" width="1000" height="500"></canvas>
</div>
<script src="https://unpkg.com/@rive-app/canvas@1.0.47"></script>
<script>
    const button = document.getElementById('button');

    const r = new rive.Rive({
        src: 'https://cdn.rive.app/animations/vehicles.riv',
        canvas: document.getElementById('canvas'),
        autoplay: true,
        stateMachines: 'bumpy',
        fit: rive.Fit.cover,
        onLoad: (_) => {
            const inputs = r.stateMachineInputs('bumpy');
            const bumpTrigger = inputs.find(i => i.name === 'bump');
            button.onclick = () => bumpTrigger.fire();
        },
    });
</script>
```

We use the `stateMachineInputs` function on the Rive object to retrieve the inputs. Each input will have a name and type. There are three types:

* `StateMachineInputType.Trigger` which has a `fire()` function
* `StateMachineInputType.Number` which has a `value` number property
* `StateMachineInputType.Boolean` which has a `value` boolean property

```javascript
const inputs = r.stateMachineInputs('bumpy');
inputs.forEach(i => {
    const inputName = i.name;
    const inputType = i.type;
    switch(inputType) {
        case rive.StateMachineInputType.Trigger:
            i.fire();
            break;
        case rive.StateMachineInputType.Number:
            i.value = 42;
            break;
        case rive.StateMachineInputType.Boolean:
            i.value = true;
            break;
    }
});
```

We can set a callback to determine when the state machine changes state. `onStateChange` provides an `event` parameter that gives us the name of the current state:

```javascript
const r = new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    autoplay: true,
    stateMachines: 'bumpy',
    onStateChange: (event) => {
        stateName.innerHTML = event.data[0];
    },
});
```
{% endtab %}

{% tab title="React" %}
The react runtime provides the `useStateMachineInput` hook to make the process of retrieving a state machine input much simplier.

```javascript
import { useRive, useStateMachineInput } from "@rive-app/react-canvas";

export default function Simple() {
  const { rive, RiveComponent } = useRive({
    src: "https://cdn.rive.app/animations/vehicles.riv",
    stateMachines: "bumpy",
    autoplay: true,
  });

  const bumpInput = useStateMachineInput(rive, "bumpy", "bump");

  return (
    <RiveComponent
      style={{ height: "1000px" }}
      onClick={() => bumpInput && bumpInput.fire()}
    />
  );
}
```

The above example show a `Trigger` input. The three types of inputs are:

* `StateMachineInputType.Trigger` which has a `fire()` function
* `StateMachineInputType.Number` which has a `value` number property
* `StateMachineInputType.Boolean` which has a `value` boolean property

We can set a callback to determine when the state machine changes.

```javascript
import { useEffect } from 'react';
import { useRive, useStateMachineInput } from "@rive-app/react-canvas";

export default function Simple() {
  const { rive, RiveComponent } = useRive({
    src: "https://cdn.rive.app/animations/vehicles.riv",
    stateMachines: "bumpy",
    autoplay: true,
    // We can pass the call back to the `useRive` hook
    onStateChange: (event) => {
      console.log(event.data[0]);
    }
  });

  const bumpInput = useStateMachineInput(rive, "bumpy", "bump");

  // We can also pass the callback to the rive object once it has loaded.
  // NOTE: If you pass the callback to the rive object, you do not need to
  // pass it to the useRive hook as well, and vice versa.
  useEffect(() => {
    if (rive) {
      rive.on('statechange', (event) => {
        console.log(event.data[0]);
      });
    }
  }, [rive]);

  return (
    <RiveComponent
      style={{ height: "1000px" }}
      onClick={() => bumpInput && bumpInput.fire()}
    />
  );
}
```
{% endtab %}

{% tab title="Flutter" %}
State machine controllers are used to retrieve a state machine's inputs which can then be used to interact with, and drive the state of, the state machine.

State machine controllers require a reference to an artboard when being instantiated. The `RiveAnimation` widget provides a callback `onInit(Artboard artboard)` that is called when the Rive file has loaded and is initialized for playback:

```dart
void _onRiveInit(Artboard artboard) {}

RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    fit: BoxFit.cover,
    onInit: _onRiveInit,
);
```

Now that we have an artboard, we can create an instance of a `StateMachineController` and from that, retrieve the inputs we're interested in. Specific inputs can be retrieved using `findInput()` or all inputs with the `inputs` property.

```dart
SMITrigger? _bump;

void _onRiveInit(Artboard artboard) {
    final controller = StateMachineController.fromArtboard(artboard, 'bumpy');
    artboard.addController(controller!);
    _bump = controller.findInput<bool>('bump') as SMITrigger;
}
```

Here we retrieve the `bump` input, which is an `SMITrigger`. This type of input has a `fire()` method to activate the trigger. There are two other input types: `SMIBool` and `SMINumber`. These both have a `value` property that can get and set the value.

```dart
class SimpleStateMachine extends StatefulWidget {
  const SimpleStateMachine({Key? key}) : super(key: key);

  @override
  _SimpleStateMachineState createState() => _SimpleStateMachineState();
}

class _SimpleStateMachineState extends State<SimpleStateMachine> {
  SMITrigger? _bump;

  void _onRiveInit(Artboard artboard) {
    final controller = StateMachineController.fromArtboard(artboard, 'bumpy');
    artboard.addController(controller!);
    _bump = controller.findInput<bool>('bump') as SMITrigger;
  }

  void _hitBump() => _bump?.fire();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Simple Animation'),
      ),
      body: Center(
        child: GestureDetector(
          child: RiveAnimation.network(
            'https://cdn.rive.app/animations/vehicles.riv',
            fit: BoxFit.cover,
            onInit: _onRiveInit,
          ),
          onTap: _hitBump,
        ),
      ),
    );
  }
}
```

In the complete example above, every time the `RiveAnimation` is tapped, it fires the `bump` input trigger, and the state machine reacts appropriately, in this case mixing in a bump animation.

If you'd like to know which state a state machine is in, or when a state machine transitions to another state, you can provide a callback to `StateMachineController`. The callback has the name of the state machine and the name of the animation associated with the current state:

```dart
 void _onRiveInit(Artboard artboard) {
  final controller = StateMachineController.fromArtboard(
    artboard,
    'bumpy',
    onStateChange: _onStateChange,
  );
  artboard.addController(controller!);
  _bump = controller.findInput<bool>('bump') as SMITrigger;
}

void _onStateChange(
  String stateMachineName,
  String stateName,
) =>
    setState(
      () => message = 'State Changed in $stateMachineName to $stateName',
    );
```
{% endtab %}

{% tab title="Angular" %}
### State Machine

You can listen on event & manipulate the state machine animation as any other animation:

```markup
<canvas riv="vehicles" width="500" height="500" fit="cover">
  <riv-state-machine name="bumpy" play speed="2" (load)="onload($event)"></riv-state-machine>
</canvas>
```

### Number & Boolean Input

If the input is a number or a boolean you can use the `value`

```markup
<canvas riv="vehicles" width="500" height="500" fit="cover">
  <riv-state-machine name="bumpy" play (stateChange)="showStates($event)">
    <riv-input name="level" [value]="value"><riv-input>
  </riv-state-machine>
</canvas>

<input type="radio" formControl="level" value="0"> Car
<input type="radio" formControl="level" value="1"> Train
<input type="radio" formControl="level" value="2"> Airplane
```

_The `stateChange` output will display the list of state changed during the same frame._

### Trigger Input

If the input is a trigger you can access it with the export as `rivInput`:

```markup
<canvas riv="vehicles">
  <riv-state-machine name="bumpy" play>
    <riv-input #trigger="rivInput" name="bump" (change)="showInput($event)"><riv-input>
  </riv-state-machine>
</canvas>

<button (click)="trigger.fire()">Bump</button>
```

_You can listen to the change in the input with the `change` Ouput._
{% endtab %}
{% endtabs %}
