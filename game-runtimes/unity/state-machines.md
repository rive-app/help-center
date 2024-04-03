---
description: Interact with the Rive State Machine in Unity
---

# State Machines

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/state-machines/docHnjaSeIIr).
{% endhint %}

For more information on Rive State Machines see the respective [runtime](../../runtimes/state-machines.md) and [editor](../../editor/state-machine/) documentation.

## Overview

A StateMachine contains [Inputs](state-machines.md#accessing-inputs) and [Events](rive-events.md) and advances (plays) an animation.

State Machines are instantiated from an Arboard instance:

```csharp
private StateMachine m_stateMachine;

...

m_stateMachine = m_artboard?.StateMachine(); // default state machine
m_stateMachine = m_artboard?.StateMachine(0); // state machine at index
m_stateMachine = m_artboard?.StateMachine("Name"); // state machine with name
```

The state machine is played by calling `advance` and passing in the delta time:

<pre class="language-csharp"><code class="lang-csharp">private void Update()
{
<strong>    m_stateMachine?.Advance(Time.deltaTime);
</strong><strong>}
</strong></code></pre>

## Accessing Inputs

There are three input types, each extends `SMIInput` (State Machine Input):

* `SMIBool` contains a `.Value` property, a boolean that can be set to true or false.
* `SMITrigger` is a boolean that is set to true for one frame by calling the `.Fire()` method.
* `SMINumber` contains a `.Value` property, a float that can be set to any value.

State machine inputs can be accessed in a number of different ways.

#### Access by name

Retrieve a state machine input by name and type.

**Trigger:**

```csharp
SMITrigger someTrigger = m_stateMachine.GetTrigger("icon_02_press_trig");
if (someTrigger != null)
{
    someTrigger.Fire();
}
```

**Bool:**

```csharp
SMIBool someBool = m_stateMachine.GetBool("centerHover");
if (someBool == null) return;
Debug.Log(someBool.Value);
someBool.Value = !someBool.Value;
Debug.Log(someBool.Value);
```

**Number:**

```csharp
SMINumber someNumber = m_stateMachine.GetNumber("rating");
if (someNumber == null) return;
Debug.Log(someNumber.Value);
someNumber.Value = 4;
Debug.Log(someNumber.Value);
```

#### Access by index

Get the input count (length) and retrieve by index:

<pre class="language-csharp"><code class="lang-csharp">Debug.Log(m_stateMachine.InputCount());
<strong>SMIInput input = m_stateMachine.Input(1);
</strong></code></pre>

#### Access all inputs

Retrieve a list of all `SMIInput`s:

```csharp
var inputs = m_riveStateMachine.Inputs();
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
