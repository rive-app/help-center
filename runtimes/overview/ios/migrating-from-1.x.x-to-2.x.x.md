---
description: Migrating guide from < 2.x
---

# Migrating from 1.x.x to 2.x.x

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/migrating-from-1xx-to-2xx/docP4U5Gjx4i).
{% endhint %}

The Rive runtime for iOS has a different API in 2.x.x from 1.x.x that allows for a unified internal model that supports both Storyboard/UIKit and SwiftUI usage.&#x20;

There are now 3 main pieces of the Rive API to be familiar with for iOS development:

* `RiveView` - Core logic for building and manipulating Rive views
* `RiveModel` - Describes the configuration model for Rive objects
* `RiveViewModel` - The main class to interface with when integrating rive, creating a Rive view in some instances. It provides a high-level API that makes it simple to do actions like instantiation, animation playback, layout changes, and more.&#x20;

We recommend migrating to the latest version of v2.x.x as soon as possible, and you can find steps on this below:

## UIKit

In v1.x.x, you may have loaded in a Rive file in the following snippet pattern:

```swift
class SimpleAnimationViewController: UIViewController {
    let url = "https://cdn.rive.app/animations/truck.riv"

    override public func loadView() {
        super.loadView()

        let view = RiveView()
        guard let riveFile = RiveFile(httpUrl: url, with: view) else {
            fatalError("Unable to load RiveFile")
        }
        try? view.configure(riveFile)

        self.view = view
    }
}
```

This pattern interfaced with 2 Rive APIs, `RiveFile` and `RiveView`. With v2.x.x, the pattern becomes simpler, interfacing with one `RiveViewModel`.

```swift
class SimpleAnimationViewController: UIViewController {
    var viewModel = RiveViewModel(fileName: "truck")
    
    override func viewWillAppear(_ animated: Bool) {
        let riveView = viewModel.createRiveView()
        view.addSubview(riveView)
        riveView.frame = view.frame
    }
}
```

Here's another example:

```swift
class MultipleAnimationsController: UIViewController, RivePLayerDelegate {
    @IBOutlet weak var riveView: RiveView!
    
    var viewModel = RiveViewModel(
        fileName: "multiple_animations", 
        animationName: "Animation 1", 
        artboardName: "Animation Playground"
    )
    
    override func viewDidLoad() {
        viewModel.setView(riveView)
    }
}
```

See subsequent runtime pages for new usage of animation playback and layouts with UIKit.

### State Machine Usage

In v1.x.x, you would set state machine input values with the following API:\
`riveView.setNumberState("Number Test", inputName: "Level", value: 2.0)`

`riveView.setBooleanState("Boolean Test", inputName: "isSuccess", value: true)`

`riveView.fireState("Trigger Test", inputName: "trigFail")`

In v2.x.x, some of the input state setters have been consolidated and renamed. Additionally, the setters are called on the `RiveViewModel` which has the context of the state machine that was instantiated, so there is no longer a need to pass it the name:\
viewModel`.setInput("Level", value: 2.0)`

viewModel`.setInput("isSuccess", value: true)`

`viewModel.triggerInput("trigFail")`

### Delegates

In the past, you may have implemented various functions that came with some of the following delegates: `LoopDelegate` , `PlayDelegate`, `PauseDelegate`, `StopDelegate`, and `StateChangeDelegate`. The various functions that get implemented on your end (i.e `loop`, `play`, `pause`, `stateChange`, etc.) have been consolidated under 2 main delegates, `RivePlayerDelegate` and `RiveStateMachineDelegate` with a slightly different function to override.

See the following list of delegates for methods to hook into:

* `RivePlayerDelegate` - Hook into animation and state machine lifecycle events
  * `player`: `(loopedWithModel riveModel: RiveModel?, type: Int) {}`
  * `player`: `(playedWithModel riveModel: RiveModel?) {}`
  * `player`: `(pausedWithModel riveModel: RiveModel?) {}`
  * `player`: `(stoppedWithModel riveModel: RiveModel?) {}`
* `RiveStateDelegate` - Hook into state changes on a state machine lifecycle
  * `stateChange`: `(_ stateMachineName: String, _ stateName: String) {}`

## SwiftUI

v1.x.x had a small wrapper around the existing `RiveView` class to help support Rive in the context of applications written in SwiftUI. v2.x.x now supports a more robust pattern for consuming Rive in your SwiftUI applications that fixes several bugs with the existing wrapper approach and provides a closer experience with the new pattern of SwiftUI.&#x20;

See subsequent runtime pages to learn how to control animation playback, state machines, and more with v2.x.x

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(fileName: "cool_rive_animation").view()
    }
}
```
