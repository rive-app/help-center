---
description: Interact with the Rive State Machine in Unity
---

# State Machines

For more information on Rive State Machines see the respective [runtime](../../runtimes/state-machines.md) and [editor](../../editor/state-machine/) documentation.

## Accessing Inputs

There are three input types, each extends `SMIInput` (State Machine Input):

* `SMIBool` is a boolean that can be set to true or false.
* `SMITrigger` is a boolean that is set to true for one frame.
* `SMINumber` is a float that can be set to any value.

State machine inputs can be accessed in a number of different ways.

#### Access by name

Retrieve a state machine input by name and type.

**Trigger:**

```csharp
SMITrigger someTrigger = m_stateMachine.getTrigger("icon_02_press_trig");
if (someTrigger != null)
{
    someTrigger.fire();
}
```

**Bool:**

```csharp
SMIBool someBool = m_stateMachine.getBool("centerHover");
if (someBool == null) return;
Debug.Log(someBool.value);
someBool.value = !someBool.value;
Debug.Log(someBool.value);
```

**Number:**

```csharp
SMINumber someNumber = m_stateMachine.getNumber("rating");
if (someNumber == null) return;
Debug.Log(someNumber.value);
someNumber.value = 4;
Debug.Log(someNumber.value);
```

#### Access by index

Get the input count (length) and retrieve by index:

<pre class="language-csharp"><code class="lang-csharp">Debug.Log(m_stateMachine.inputCount());
<strong>SMIInput input = m_stateMachine.input(1);
</strong></code></pre>

#### Access all inputs

Retrieve a list of all `SMIInput`s:

```csharp
var inputs = m_riveStateMachine.inputs();
foreach (var input in inputs)
{
    switch (input)
    {
        case SMITrigger smiTrigger:
        {
            // Do something
            break;
        }
        case SMIBool smiBool:
        {
            // Do something
            break;
        }
        case SMINumber smiNumber:
        {
            // Do something
            break;
        }
    }
}
```

### Additional Resources

For a complete example see the **getting-started** project in the [examples repository](https://github.com/rive-app/rive-unity-examples) and open the **StateMachineInputScene** scene. Enter **Play** mode and in the inspector on the **Main Camera** component, you can interact with all available state machine inputs for the provided animation.