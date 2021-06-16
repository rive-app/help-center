---
description: Playing and changing inputs in state machines
---

# State Machines

Rive's state machines provide a way to combine a set of animations and manage the transition between them through a series of inputs that can be programmatically controlled. Once a state machine is instantiated and playing, transitioning states can be accomplished by changing boolean or double value inputs, or firing triggers. The effects of these will be dependent on how the state machine has been configured in the editor.

## Playing state machines

State machines are instantiated in much the same manner as animations: provide the state machine name to the Rive object:

{% tabs %}
{% tab title="web" %}
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
{% endtabs %}

Once the Rive file is loaded and instantiated, the state machine can be queried for inputs, and these inputs can then be read from and written to, and in the case of triggers, fired.

{% tabs %}
{% tab title="web" %}
The web runtime provides an `onload` callback that's run when the Rive file is loaded and ready for use. We use this here to ensure that the state machine is instantiated when we query for inputs.

```markup
<div id="button">
    <canvas id="canvas" width="1000" height="500"></canvas>
</div>
<script src="/dist/rive.min.js"></script>
<script>
    const button = document.getElementById('button');

    const r = new rive.Rive({
        src: 'https://cdn.rive.app/animations/vehicles.riv',
        canvas: document.getElementById('canvas'),
        autoplay: true,
        stateMachines: 'bumpy',
        fit: rive.Fit.cover,
        onload: (_) => {
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

We can set a callback to determine when the state machine changes state. `onstatechange` provides an `event` parameter that gives us the name of the current state:

```javascript
const r = new rive.Rive({
    src: 'https://cdn.rive.app/animations/vehicles.riv',
    canvas: document.getElementById('canvas'),
    autoplay: true,
    stateMachines: 'bumpy',
    onstatechange: (event) => {
        stateName.innerHTML = event.data[0];
    },
});
```
{% endtab %}
{% endtabs %}

