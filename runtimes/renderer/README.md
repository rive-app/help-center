---
description: Specify a renderer to use at runtime
---

# Choose a Renderer

Rive makes use of various different renderers depending on platform and runtime. We're working towards unifying the default renderer used across all platforms/runtimes with the [Rive Renderer](https://rive.app/renderer), which is now available on iOS, Android, and Rive GameKit.

## Renderer Options and Default

You can opt-in to use a specific renderer, see [#specifying-a-renderer](./#specifying-a-renderer "mention").

The table below outlines the available, and default, renderers for Rive's runtimes:

<table><thead><tr><th width="239">Runtime</th><th width="248.33333333333331">Renderer</th><th>Options</th></tr></thead><tbody><tr><td>Android</td><td>Skia</td><td>Rive / Skia</td></tr><tr><td>iOS</td><td>Skia</td><td>Rive / Skia</td></tr><tr><td>React Native</td><td>Skia</td><td>Skia</td></tr><tr><td>Web (Canvas) </td><td>Canvas2D</td><td>Canvas2D</td></tr><tr><td>Web (WebGL)</td><td>Skia</td><td>Skia</td></tr><tr><td>Flutter</td><td>Skia (other), Impeller (iOS)</td><td>Skia / Impeller</td></tr></tbody></table>

### Note on Rendering in Flutter

Starting in Flutter `v3.10`, [Impeller](https://docs.flutter.dev/perf/impeller) has replaced [Skia](https://skia.org/) to become the default renderer for apps on the iOS platform and may continue to be the default on future platforms over time. As such, there is a possibility of rendering discrepancies when using the `rive` Flutter runtime with platforms that use the Impeller renderer that may not have surfaced before. If you encounter visual errors at runtime compared to expected behavior in the Rive editor, we recommend trying the following steps to triage:

1. Try running the Flutter app with the `--no-enable-impeller` flag to use the Skia renderer. If the visual discrepancy does not show when using Skia, it may be a rendering bug on Impeller. However, before raising a bug with the Flutter team, try the second point below :point\_down:

```bash
flutter run --no-enable-impeller
```

2. Try running the Flutter app on the latest `master` channel. It is possible that visual bugs may be resolved on the latest Flutter commits, but not yet released in the `beta` or `stable` channel.
3. If you are still seeing visual discrepancies with just the Impeller renderer on the latest master branch, we recommend raising a detailed issue to the [Flutter](https://github.com/flutter/flutter) Github repo with a reproducible example, and other relevant details that can help the team debug any possible issues that may be present.

## Rive Renderer

The [Rive Renderer](https://rive.app/renderer) is now available on Android and iOS. See [#specifying-a-renderer](./#specifying-a-renderer "mention") to set it as your preferred renderer.

While it's ready for testing and your feedback is highly valued during this phase, we advise exercising caution before considering it for production builds. You may encounter compatibility issues with certain devices. Please reach out to us on Discord or through our Support Channel.

Your collaboration helps us refine and enhance the Rive Renderer to make it more robust and reliable for broader applications. Thank you for being a part of this exciting journey!

{% hint style="info" %}
By default, both Android and iOS will use the current Skia renderer.
{% endhint %}

{% tabs %}
{% tab title="iOS / macOS" %}
## Starting Version

The Rive Renderer was introduced in the iOS runtime starting at **v5.3.2**, however, we recommend installing the latest version of the dependency to get the latest updates. See the [CHANGELOG](https://github.com/rive-app/rive-ios/blob/main/CHANGELOG.md) for details on the latest versions.

{% hint style="warning" %}
At this time, the Rive Renderer is not supported in simulator environments, and can only be tested on physical devices. Configuring the `.riveRenderer` on your project while running on the iOS simulator may use a Core Graphics (CG) renderer as a fallback, which should support all Rive features.
{% endhint %}

Also note that at this time, only Apple Silicon-based Mac machines will support the Rive Renderer in macOS applications.

### Performance

The Rive Renderer will shine best on iOS in memory usage as an animation plays out, in comparison to previous default renderers.

With UIKit, you'll be able to see the best performance differences by drawing multiple times on a single `RiveView`, rather than creating multiple instances of `RiveView`'s, or multiple `RiveViewModel`'s.&#x20;

**Example:** See this [stress test example](https://github.com/rive-app/rive-ios/blob/main/Example-iOS/Source/Examples/Storyboard/StressTest.swift) to see how you can override the drawing function on `RiveView` to draw multiple times on the same view, with each graphic at an offset. You can switch out the renderer with the above config and test out the performance for yourself!
{% endtab %}

{% tab title="Android" %}
## Starting Version

The Rive Renderer was introduced in the Android runtime starting at **v8.5.0**, however, we recommend installing the latest version of the dependency to get the latest updates. See the [CHANGELOG](https://github.com/rive-app/rive-android/blob/master/CHANGELOG.md) for details on the latest versions.
{% endtab %}
{% endtabs %}

## Specifying a Renderer

See below for runtime instructions to enable a specific renderer.

{% tabs %}
{% tab title="iOS/macOS" %}
## Getting Started

Options: `Rive / Skia (default)`

Below are some notes on configuring the renderer in UIKit and SwiftUI.

### UIKit

Set the global renderer type during your application launch:

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        // For Skia: set RendererType.skiaRenderer (current default)
        RenderContextManager.shared().defaultRenderer = RendererType.riveRenderer
        return true
    }

    ...
}
```

### SwiftUI

New SwiftUI applications launch with the `App` protocol, but you can still add `UIApplicationDelegate` functionality.

#### iOS

Create a new file and class called `AppDelegate` as such, including a line to set the `defaultRenderer` to `RendererType.riveRenderer`:

{% code title="AppDelegate.swift" %}
```swift
import UIKit
import Foundation
import RiveRuntime

class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        RenderContextManager.shared().defaultRenderer = RendererType.riveRenderer
        return true
    }
}
```
{% endcode %}

Next, at the entry point of your application, use `UIApplicationDelegateAdaptor` to set the `AppDelegate` created above for the application delegate.

{% code title="MyRiveRendererApp.swift" %}
```swift
@main
struct MyRiveRendererApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
{% endcode %}

#### macOS

Create a new file and class called `AppDelegate` as such, including a line to set the `defaultRenderer` to `RendererType.riveRenderer`:

{% code title="AppDelegate.swift" %}
```swift
import Foundation
import RiveRuntime

class AppDelegate: NSObject, NSApplicationDelegate {
    func application(_ application: NSApplication, applicationDidFinishLaunching notification: Notification) -> Bool {
        RenderContextManager.shared().defaultRenderer = RendererType.riveRenderer
        return true
   
```
{% endcode %}

Next, at the entry point of your application, use `UIApplicationDelegateAdaptor` to set the `AppDelegate` created above for the application delegate.

{% code title="MyRiveRendererApp.swift" %}
```swift
@main
struct MyRiveRendererApp: App {
    @NSApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Android" %}
## Getting Started

Options: `Rive / Skia (default)`

Specify the renderer target in XML:

```xml
<app.rive.runtime.kotlin.RiveAnimationView
  app:riveRenderer="Rive"
  … />
```

Alternatively, when initializing Rive:

```kotlin
Rive.init(applicationContext, defaultRenderer = RendererType.Rive)
```
{% endtab %}
{% endtabs %}

