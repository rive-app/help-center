---
description: Playing and changing inputs in state machines
---

# State Machines

For more information on designing and building state machines in Rive, please refer to the [editor's state machine section](https://app.gitbook.com/@rive/s/rive-help-center/editor/animate-mode/state-machine).

Rive's state machines provide a way to combine a set of animations and manage the transition between them through a series of inputs that can be programmatically controlled. Once a state machine is instantiated and playing, transitioning states can be accomplished by changing `boolean` or `double`-value inputs, or firing trigger inputs. The effects of these will be dependent on how the state machine has been configured in the editor.

## Playing state machines

State machines are instantiated in much the same manner as animations: provide the state machine name to the Rive object when instantiated. Ensure that the Rive instance is set to auto-play on initialization to allow the state machine to start immediately.

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

{% tab title="Angular" %}
```markup
<canvas riv="vehicles" width="500" height="500" fit="cover">
  <riv-state-machine name="bumpy" play></riv-state-machine>
</canvas>
```
{% endtab %}

{% tab title="React Native" %}
Set `stateMachineName` on the Rive component to play a single state machine.

```jsx
  <Rive
    resourceName={'skills'}
    autoplay={true}
    stateMachineName="Designer's Test"
  />
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

{% tab title="iOS" %}
Specify a starting state machine by setting the name of the state machine via `stateMachineName` when instantiating the `RiveViewModel`.

### SwiftUI

```swift
var stateChanger = RiveViewModel(
    fileName: "skills",
    stateMachineName: "Designer's Test"
)
```

### UIKit

```swift
class StateMachineViewController: UIViewController {
    var viewModel = RiveViewModel(
        fileName: "skills",
        stateMachineName: "Designer's Test"
    )
    
    override public func loadView() {
        super.loadView()
        
        guard let stateMachineView = view as? StateMachineView else {
            fatalError("Could not find StateMachineView")
        }
        
        viewModel.setView(stateMachineView.riveView)
    }
}
```
{% endtab %}

{% tab title="Android" %}
### Via XML

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:id="@+id/simple_state_machine"
    android:layout_width="match_parent"
    android:layout_height="400dp"
    app:riveResource="@raw/skills"
    app:riveStateMachine="Designer's Test" />
```

### &#x20;Via Kotlin

```kotlin
animationView.setRiveResource(
    R.raw.simple_state_machine,
    autoplay = true,
    stateMachineName = "Designer's Test"
)
```

\
Additionally, you can use the same APIs from animation playback (i.e `play`, `pause`, and `stop`) to control state machine playback, as long as you set the `isStateMachine` attribute to `true`.

```kotlin
animationView.play(
    "Designer's Test",
    Loop.AUTO,
    Direction.AUTO,
    isStateMachine = true
)

animationView.pause(
    "Designer's Test",
    isStateMachine = true
)

animationView.stop(
    "Designer's Test",
    isStateMachine = true
)
```
{% endtab %}
{% endtabs %}

## Controlling state machine inputs

Once the Rive file is loaded and instantiated, the state machine(s) can be queried for inputs, and these input values can be set, and in the case of triggers, fired, all programmatically.

{% tabs %}
{% tab title="Web" %}
### Inputs

The web runtime provides an `onLoad` callback that's run when the Rive file is loaded and ready for use. We use this callback to ensure that the state machine is instantiated when we query for inputs.

```markup
<div id="button">
    <canvas id="canvas" width="1000" height="500"></canvas>
</div>
<script src="https://unpkg.com/@rive-app/canvas@1.0.60"></script>
<script>
    const button = document.getElementById('button');

    const r = new rive.Rive({
        src: 'https://cdn.rive.app/animations/vehicles.riv',
        canvas: document.getElementById('canvas'),
        autoplay: true,
        stateMachines: 'bumpy',
        fit: rive.Fit.cover,
        onLoad: (_) => {
            // Get the inputs via the name of the state machine
            const inputs = r.stateMachineInputs('bumpy');
            // Find the input you want to set a value for, or trigger
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



### State change event callback

We can set a callback to determine when the state machine changes state. `onStateChange` provides an `event` parameter that gives us the string name(s) of the current state(s):

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
### Inputs

The react runtime provides a `useStateMachineInput` hook to make the process of retrieving a state machine input much simpler than that of the basic web runtime.

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

The above example shows the retrieval of a `Trigger` input from a named state machine. The three types of inputs are:

* `StateMachineInputType.Trigger` which has a `fire()` function
* `StateMachineInputType.Number` which has a `value` number property
* `StateMachineInputType.Boolean` which has a `value` boolean property



### State change event callback

We can set a callback to determine when the state machine changes, just like in the web runtime.

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

{% tab title="React Native" %}
### Inputs

With the React Native runtime, most methods/triggers are available on the ref of the `Rive` component, including setting input values/triggering for state machines. In this case, there is no need to acquire an instance of an input. Simply set the input state from the Rive `ref` or fire an input state.

```jsx
export default function StateMachine() {
  const riveRef = React.useRef<RiveRef>(null);
  // Maintain the values of your state machine in React state
  const [selectedLevel, setSelectedLevel] = useState('2');

  const setLevel = (n: number) => {
    setSelectedLevel(n.toString());
    // No need to acquire an instance of a state machine input, just set the
    // input state on the `riveRef` itself
    riveRef.current?.setInputState("Designer's Test", 'Level', n);
  };

  return (
    <SafeAreaView style={styles.safeAreaViewContainer}>
      <ScrollView contentContainerStyle={styles.container}>
        <Rive
          resourceName={'skills'}
          ref={riveRef}
          autoplay={true}
          stateMachineName="Designer's Test"
        />
        <RadioButton.Group
          onValueChange={(newValue) => setLevel(parseInt(newValue, 10))}
          value={selectedLevel}
        >
          <View style={styles.radioButtonsWrapper}>
            <View style={styles.radioButtonWrapper}>
              <Text>{'Beginner'}</Text>
              <RadioButton value={'0'} />
            </View>
            <View style={styles.radioButtonWrapper}>
              <Text>{'Intermediate'}</Text>
              <RadioButton value={'1'} />
            </View>
            <View style={styles.radioButtonWrapper}>
              <Text>{'Expert'}</Text>
              <RadioButton value={'2'} />
            </View>
          </View>
        </RadioButton.Group>
      </ScrollView>
    </SafeAreaView>
  );
}
```

{% hint style="info" %}
See the [React Native API's](overview/react-native/rive-ref-methods.md#.setinputstate) to learn more about the parameters for `.setInputState()` and `.fireState()`
{% endhint %}



### State change event callback

We can set a callback to determine when the state machine changes.

```jsx
<Rive
  resourceName={'skills'}
  autoplay={true}
  stateMachineName="Designer's Test"
  onStateChanged={(stateMachineName, stateName) => {
    console.log(
      'onStateChanged: ',
      'stateMachineName: ',
      stateMachineName,
      'stateName: ',
      stateName
    );
  }}
/>
```
{% endtab %}

{% tab title="Flutter" %}
### Inputs

State machine controllers are used to retrieve a state machine's inputs which can then be used to interact with, and drive the state of a state machine.

State machine controllers require a reference to an artboard when being instantiated. The `RiveAnimation` widget provides a callback `onInit(Artboard artboard)` that is called when the Rive file has loaded and is initialized for playback:

```dart
void _onRiveInit(Artboard artboard) {}

RiveAnimation.network(
    'https://cdn.rive.app/animations/vehicles.riv',
    fit: BoxFit.cover,
    onInit: _onRiveInit,
);
```

In the `onInit` callback, you can create an instance of a `StateMachineController` and then retrieve the inputs you're interested in by their name. Specific inputs can be retrieved using `findInput()` or all inputs with the `inputs` property.

```dart
SMITrigger? _bump;

void _onRiveInit(Artboard artboard) {
    final controller = StateMachineController.fromArtboard(artboard, 'bumpy');
    artboard.addController(controller!);
    _bump = controller.findInput<bool>('bump') as SMITrigger;
}
```

In the above snippet, the `bump` input is retrieved, which is an `SMITrigger`. This type of input has a `fire()` method to activate the trigger. There are two other input types: `SMIBool` and `SMINumber`. These both have a `value` property that can get and set the value.

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

In the complete example above, every time the `RiveAnimation` is tapped, it fires the `bump` input trigger and the state machine reacts appropriately.



### State change event callback

If you'd like to know which state a state machine is in, or when a state machine transitions to another state, you can provide a callback to `StateMachineController`. The callback has the name of the state machine and the name of the animation associated with the state transitioned to:

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

{% tab title="iOS" %}
### Inputs

Just like with animation playback controls, setting input values for state machines goes through the `RiveViewModel` instantiated in the View class.

`.setInput()`

* `inputName` (String) - Name of the input on a state machine to set a value for
* `value` (Bool, Float, or Double) - value to set for the associated `inputName`

`triggerInput()`

* `inputName` (String) - Name of the input on a state machine to trigger\


```swift
// Example of a number input
starsVM.setInput("Rating Changed", value: 5)

// Example of a boolean input
toggleVM.setInput("Switch Flipped", value: true)

// Example of a trigger input
confettiVM.triggerInput("Celebrate")
```

####

### State change event callbacks

This runtime allows for delegates that can be set on the `RiveViewModel`. If provided, these delegate functions will be fired whenever a matching event is triggered to be able to hook into and listen for certain events in the Rive animation cycle.

\
Currently, there exist the following delegates:

* `RivePlayerDelegate` - Hook into animation and state machine lifecycle events
  * `player`: `(loopedWithModel riveModel: RiveModel?, type: Int) {}`
  * `player`: `(playedWithModel riveModel: RiveModel?) {}`
  * `player`: `(pausedWithModel riveModel: RiveModel?) {}`
  * `player`: `(stoppedWithModel riveModel: RiveModel?) {}`
* `RiveStateMachineDelegate` - Hook into state changes on a state machine lifecycle
  * `stateMachine`: `(_ stateMachine: RiveStateMachineInstance, didStateChange stateName: String) {}`

You can create your own delegate or mix in with the `RiveViewModel`, implementing as many protocols as are needed. Below is an example of how to customize a RiveViewModel's implementation of the `RivePlayerDelegate`:

```swift
class SimpleAnimation: RiveViewModel {
    init() {
        let model = RiveModel(fileName: "truck_v7", stateMachineName: "Drive")
        super.init(model)
    }
    
    override func setView(rview view: RiveView) {
        super.setView(view)
        rview?.playerDelegate = self
        rview?.stateMachineDelegate = self
    }

    override func player(playedWithModel riveModel: RiveModel?) {
        if let stateMachineName = riveModel?.stateMachine?.name() {...}
    }
    
    override func player(pausedWithModel riveModel: RiveModel?) {
        if let stateMachineName = riveModel?.stateMachine?.name() {...}
    }
    
    override func player(stoppedWithModel riveModel: RiveModel?) {
        if let stateMachineName = riveModel?.stateMachine?.name() {...}
    }
    
    func stateMachine(_ stateMachine: RiveStateMachineInstance, didChangeState stateName: String) {
        var stateMachineNames: [String] = []
        var stateMachineStates: [String] = []
        stateMachineNames.append(stateMachine.name())
        stateMachineStates.append(stateName)
        ...
    }
}
```
{% endtab %}

{% tab title="Android" %}
### Inputs

Just like other methods within the `rive-android` runtime, use the view to set values on a state machine input. In this case, there is no need to grab references to state machine input instances to set values.



There are 3 different methods to set input values or trigger inputs for number, boolean, and trigger inputs respectively:

* `.setNumberState(stateMachineName: String, inputName: String, value: Float)`
* `.setBooleanState(stateMachineName: String, inputName: String, value: Boolean)`
* `.fireState(stateMachineName: String, inputName: String)`

```kotlin
// i.e Set input state on a number input
animationView.setNumberState("Designer's Test", "Level", 0f)

// i.e Set boolean state on a boolean input
animationView.setBooleanState("Boolean test", "foo", true)

// i.e Fire a trigger input
animationView.fireState("Trigger test", "fireInput");
```

### &#x20;State change event callback

To listen for state changes, when creating a `Listener` to register on your animation view, you can add the following callback, where you'll receive the name of the state machine, and the state it transitions to:\


```kotlin
val listener = object : Listener {
    override fun notifyStateChanged(stateMachineName: String, stateName: String) {
        // Do something
    }
}
animationView.registerListener(listener)
```
{% endtab %}
{% endtabs %}





