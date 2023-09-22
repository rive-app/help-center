# Props

## Rive Component Props

The following are props you can set on the Rive React component for this runtime:

* `children` _(optional)_ - Can be used to display something positioned `absolutely` on top of the rive animation view.
* `style` _(optional) -_ Style of the rive animation view wrapper.&#x20;
  * Default: `undefined`&#x20;
  * Type: `StyleProp<ViewStyle>`
* `resourceName` _(optional)_ - A file name that matches the rive file without `.riv` extension. You should provide either `resourceName` or `url` not both at the same time.
  * Default: `undefined`&#x20;
  * Type: `string`
* `url` _(optional)_ - A URL that provides a rive file. You should provide either `resourceName` or `url` not both at the same time.
  * Default: `undefined`&#x20;
  * Type: `string`
* `autoplay` _(optional)_ - Opening a rive animation view or specifying new `resourceName` or `url` will make it automatically play, when it is ready.
  * Default: `true`&#x20;
  * Type: `boolean`
* `fit` _(optional)_ - Specifies how animation should be displayed inside rive animation view
  * Default: `Fit.Contain`&#x20;
  * Type: [`Fit`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#Fit)
* `alignment` _(optional)_ - Specifies how animation should be aligned inside rive animation view.
  * Default: `Alignment.None`&#x20;
  * Type: [`Alignment`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#Alignment)
* `artboardName` _(optional)_ - Specifies which animation artboard should be displayed in rive animation view.
  * Default: `undefined`&#x20;
  * Type: `string`
* `animationName` _(optional)_ - Specifies which animation should be played when `autoplay` is set to `true`.
  * Default: `undefined`&#x20;
  * Type: `string`
* `stateMachineName` _(optional)_ - Specifies which stateMachine should be played when `autoplay` is set to `true`.
  * Default: `undefined`&#x20;
  * Type: `string`
* `testID` _(optional)_ - Specifies testID which could be handy in tests.
  * Default: `undefined`&#x20;
  * Type: `string`
* `onPlay` _(optional)_ - Callback function that is called when animation or stateMachine has been started.
  * Type: `(animationName: string, isStateMachine: boolean) => void`
* `onPause` _(optional)_ - Callback function that is called when animation or stateMachine has been paused.
  * Type: `(animationName: string, isStateMachine: boolean) => void`
* `onStop` _(optional)_ - Callback function that is called when animation or stateMachine has been stopped.
  * Type: `(animationName: string, isStateMachine: boolean) => void`
* `onLoopEnd` _(optional)_ - Callback function that is called when animation loop has been ended. **Note:** This callback is only invoked if playing individual animations via the `animationName` prop, and does not get invoked if playing a state machine via the `stateMachineName` prop.
  * Type: [`(animationName: string, loopMode: LoopMode) => void`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#LoopMode)
* `onStateChanged` _(optional)_ - Callback function that is called when the internal animation state has been changed. It's tightly coupled with state machines feature.
  * Type: `(stateMachineName: string, stateName: string) => void`
* `onError` _(optional)_ - Callback function that is called when error is thrown. Allows manual handling of thrown errors that are described by [`RNRiveError`](https://github.com/rive-app/rive-react-native/blob/main/docs/types.md#RNRiveError).
  * Type: `(riveError: RNRiveError) => void`
* `onRiveEventReceived` _(optional)_ - Callback function that is called when the render loop reports a Rive Event.
  * Type: `(event: RiveGeneralEvent | RiveOpenUrlEvent) => void`





