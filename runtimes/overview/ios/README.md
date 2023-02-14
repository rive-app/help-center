---
description: iOS runtime for Rive
---

# iOS

## Overview

This guide documents how to get started using the iOS runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-ios).\
\
This library contains an API for iOS apps to easily integrate their Rive assets for both UIKit and SwiftUI. The runtime can also be installed via Cocoapods or Swift Package Manager.

The minimum iOS target is **14.0**

## Getting Started

Follow the steps below for a quick start on integrating Rive into your iOS app.

### 1. Install the dependency

#### Via Cocoapods

Add the following to your Podspec file:

{% code title="Podfile" %}
```ruby
  pod 'RiveRuntime'
```
{% endcode %}

#### Via Swift Package Manager

To install via Swift Package Manager, in the package finder in xcode, search with the Github repository name: `https://github.com/rive-app/rive-ios`

### 2. Importing Rive

Add the following to the top of your file where you utilize the Rive runtime:

```swift
import RiveRuntime
```

### 3. v2 Runtime Usage

Rive iOS runtimes of versions 2.x.x or later should use the newer patterns for integrating Rive into your iOS applications. This involves some API changes from the pattern in versions 1.x.x (see migration guide for guidance on moving to 2.x.x). The primary object you'll use is a `RiveViewModel`. It is responsible for creating and interacting with Rive assets.&#x20;

#### 3a. SwiftUI

**Set up RiveViewModel w/ View**

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(fileName: "cool_rive_animation").view()
    }
}
```

In the above example, you reference the name of a `.riv` asset bundled into your application, but you can also load in a `.riv` file hosted on a remote URL like so:

```swift
struct AnimationView: View {
    var body: some View {
        RiveViewModel(
            webURL: "https://cdn.rive.app/animations/off_road_car_v7.riv"
        ).view()
    }
}
```

#### 3b. UIKit - Storyboard

#### Set up RiveViewModel w/ Controller formatted on a Storyboard

The simplest way of adding Rive to a controller using Storyboards is to make a `RiveViewModel`, and set its view to be the `RiveView` you made in the Storyboard.

```swift
class AnimationViewController: UIViewController {
    @IBOutlet weak var riveView: RiveView!
    var simpleVM = RiveViewModel(fileName: "cool_rive_animation")

    override public func viewDidLoad() {
        simpleVM.setView(riveView)
    }
}
```

#### 3c. UIKit - Programmatic

#### Set up RiveViewModel w/ Controller from scratch in code

You can also add Rive to a controller purely with code by making the `RiveViewModel`, telling it to create a fresh `RiveView` and then adding it to the view hierarchy.

```swift
class AnimationViewController: UIViewController {
    var simpleVM = RiveViewModel(fileName: "cool_rive_animation")
    
    override func viewWillAppear(_ animated: Bool) {
        let riveView = simpleVM.createRiveView()
        view.addSubview(riveView)
        riveView.frame = view.frame
    }
}
```

See subsequent runtime pages to learn how to control animation playback, state machines, and more.

## Resources

Github: [https://github.com/rive-app/rive-ios](https://github.com/rive-app/rive-ios)\
Examples:&#x20;

* [https://github.com/rive-app/rive-ios/tree/main/Example-iOS](https://github.com/rive-app/rive-ios/tree/main/Example-iOS)
* [https://github.com/rive-app/rive-ios/tree/main/Demo-App](https://github.com/rive-app/rive-ios/tree/main/Demo-App)
* Free course from Meng To: [https://designcode.io/swiftui-rive](https://designcode.io/swiftui-rive)&#x20;
