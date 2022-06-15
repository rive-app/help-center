# Rive Ref Methods

Once you have access to the Rive ref object, there are a number of methods available to invoke for controlling animations and state machines.

### `.play()`

A reference method that will play animation/animations. For an animation currently playing it is a no-op.

Type: `(animationNames?: string | string[], loop?: LoopMode, direction?: Direction, areStateMachines?: boolean) => void`

* `animationNames` - Specifies which animations should be played. A string can be passed in the case of a single animation. If this argument hasn't been passed the first animation will be played.
  * Default: `[]`
* `loop` - Specifies which [`LoopMode`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#LoopMode) should be used for playing the animations.
  * Default: `LoopMode.Auto`
* `direction` - Specifies which [`Direction`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#Direction) should be used for playing the animations.
  * Default: `Direction.Auto`
* `areStateMachines` - Specifies whether passed `animationNames` are state machines or just animations.
  * Default: `false`

Example:

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'truck_v7'

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handlePlay = () => { riveRef.current?.play() };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} autoplay={false} />
      <Button onPress={handlePlay} title="Play">
    </>
  );
}
```

### `.pause()`

A reference method that will pause animation/animations. For the animations currently stopped/paused it is no-op.

Type: `(animationNames?: string | string[], areStateMachines?: boolean) => void`

* `animationNames` - Specifies which animations should be paused. A string can be passed in the case of a single animation. If this argument hasn't been passed all currently playing animations will be played.
  * Default: `[]`
* `areStateMachines` - Specifies whether passed `animationNames` are state machines or just animations.
  * Default: `false`

Example:

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'truck_v7'

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handlePause = () => { riveRef.current?.pause() };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} />
      <Button onPress={handlePause} title="Pause">
    </>
  );
}
```

### `.stop()`

A reference method that will stop animation/animations. For the animations currently stopped/paused it is no-op.

Type: `(animationNames?: string | string[], areStateMachines?: boolean) => void`

* `animationNames` - Specifies which animations should be stopped. A string can be passed in the case of a single animation. If this argument hasn't been passed the whole artboard will be reset and paused.
  * Default: `[]`
* `areStateMachines` - Specifies whether passed `animationNames` are state machines or just animations.
  * Default: `false`

Example:

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'truck_v7'

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handleStop = () => { riveRef.current?.stop() };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} />
      <Button onPress={handleStop} title="Stop">
    </>
  );
}
```

### `.reset()`

A reference method that will reset the whole artboard. It will play `animationName` or the first animation _(if `animationName` hasn't been passed)_ immediately if `autoplay` hasn't been set to `false` explicitly.

Type: `() => void`

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'truck_v7'

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handleReset = () => { riveRef.current?.reset() };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} autoplay={true} />
      <Button onPress={handleReset} title="Reset">
    </>
  );
}
```

### `.fireState()`

A reference method that will fire `trigger` identified by the `inputName` on all active matching state machines.

Type: `(stateMachineName: string, inputName: string) => void`

* `stateMachineName` - Specifies state machine name which will be matched against all active state machines.
* `inputName` - Specifies the name of the `trigger` that should be fired.

Example

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'ui_swipe_left_to_delete'

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handleFireState = () => { riveRef.current?.fireState('Swipe to delete', 'Trigger Delete') };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} autoplay={true} />
      <Button onPress={handleFireState} title="FireState">
    </>
  );
}
```

### `.setInputState()`

A reference method that will set `input` state identified by the `inputName` on all active matching state machines to the given `value`.

Type: `(stateMachineName: string, inputName: string, value: boolean | number) => void`

* `stateMachineName` - Specifies state machine name which will be matched against all active state machines.
* `inputName` - Specifies name of the `input` which state should be updated.
* `value` - Specifies a value that the `input` state should be set to.

Example:

```jsx
import Rive, { RiveRef } from 'rive-react-native';

const resourceName = 'ui_swipe_left_to_delete'
const threshold = 50

function App() {
  const riveRef = React.useRef<RiveRef>(null);

  const handleFireState = () => {
    riveRef.current?.setInputState(
      'Swipe to delete',
      'Swipe Threshold',
      threshold
    );
  };

  return (
    <>
      <Rive ref={riveRef} resourceName={resourceName} autoplay={true} />
      <Button onPress={handleFireState} title="FireState">
    </>
  );
}
```

