---
description: Run Rive on your platform of choice.
---

# Getting Started

The Rive runtimes are open-source libraries that allow you to load and control your animations in apps, games, and websites. Dive into each of the subpages to get started!

## How to use this guide

In this section you'll find runtime subpages that provide all the needed information and resources to get started on your platform of choice. See [Installation and Getting Started](overview.md#installation-and-getting-started) below.

You'll also find pages dedicated to controlling your animation at runtime. For example, updating a state machine input or text run. See [Animation Control and Interaction](overview.md#animation-control-and-interaction) below.

### Installation and Getting Started

{% hint style="info" %}
Make sure to check out the additional documentation provided under each runtime section. These documents provide platform-specific considerations, migration guides, and advanced usage information.
{% endhint %}

* [Web (JS)](overview/web-js/)
* [React](overview/react/)
* [iOS/macOS](overview/ios/)
* [Android](overview/android.md)
* [Flutter](overview/flutter.md)
* [React Native](overview/react-native/)
* [Vue](overview/vue.md)
* [Angular](overview/angular.md)
* [C#](https://github.com/CommunityToolkit/Labs-Windows/blob/main/components/RivePlayer/samples/RivePlayer.md)
* [Noesis](overview/noesis.md)

### Animation Control and Interaction

These sections provide details on how to interact with your Rive animations at runtime. Here, you'll find documentation and code samples for all of Rive's official runtimes.

* [Animation Playback](playback.md)
* [Layout](layout.md)
* [State Machines](../editor/state-machine/)
* [Text](text.md)
* [Advanced Topics](advanced\_topics/)
* [Feature Support](feature-support.md)

***

## Versioning

As we publish updates to our Rive editor, we will occasionally push updated runtimes to support the new features. See [feature support](feature-support.md) for the required minimum runtime version needed for specific features.

In most cases, the newest runtimes will also support previous versions of your Rive assets, so you will not need to re-export assets to update to the latest runtimes.

There are a number of ways to export your Rive files in cases where re-exporting is necessary to take advantage of the latest features. Check out our documentation on [exporting](../editor/exporting.md) for more information.

***

## Official runtimes

Check out the runtime subpages for steps on how to get started!

| [Web](overview/web-js/)                | <p>All web runtimes are distributed by npm:</p><ul><li><strong>(Recommended)</strong> <a href="https://www.npmjs.com/package/@rive-app/canvas">canvas</a></li><li><a href="https://www.npmjs.com/package/@rive-app/canvas-advanced">canvas (advanced)</a></li><li><a href="https://www.npmjs.com/package/@rive-app/webgl">webgl</a></li><li><a href="https://www.npmjs.com/package/@rive-app/webgl-advanced">webgl (advanced)</a></li></ul><p>See <a href="overview/web-js/canvas-vs-webgl.md">Canvas vs WebGL</a> for more on the differences between these dependencies</p>                                                                                           |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [React](overview/react/)               | <p>All React runtimes are distributed by npm:</p><ul><li><strong>(Recommended)</strong> <a href="https://www.npmjs.com/package/@rive-app/react-canvas">canvas</a></li><li><a href="https://www.npmjs.com/package/@rive-app/react-webgl">webgl</a></li></ul><p><a href="https://github.com/rive-app/rive-react">GitHub</a></p>                                                                                                                                                                                                                                                                                                                                           |
| [iOS/macOS](overview/ios/)             | <p>The iOS/macOS runtime is distributed by:</p><ul><li><a href="https://swiftpackageregistry.com/rive-app/rive-ios">Swift Package Manager</a></li><li>Cocoapods</li></ul><p><a href="https://github.com/rive-app/rive-ios">GitHub</a></p>                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [Android](overview/android.md)         | [maven](https://search.maven.org/artifact/app.rive/rive-android), [GitHub](https://github.com/rive-app/rive-android)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| [Flutter](overview/flutter.md)         | ​[pub.dev](https://pub.dev/packages/rive), [GitHub](https://github.com/rive-app/rive-flutter)​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| C++ (Mac/Linux/Windows)                | ​[GitHub](https://github.com/rive-app/rive-cpp)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| C#                                     | <ul><li><strong>(Recommended)</strong> <a href="https://dev.azure.com/dotnet/CommunityToolkit/_artifacts/feed/CommunityToolkit-Labs/NuGet/CommunityToolkit.Labs.Uwp.RivePlayer/overview/0.0.1">UWP</a></li><li><a href="https://dev.azure.com/dotnet/CommunityToolkit/_artifacts/feed/CommunityToolkit-Labs/NuGet/CommunityToolkit.Labs.WinUI.RivePlayer/overview/0.0.1">WinUI</a></li><li>(High-level API) <a href="https://github.com/CommunityToolkit/Labs-Windows/blob/main/labs/RivePlayer/samples/RivePlayer.Samples/RivePlayer.md">RivePlayer Github</a></li><li>(Low-level API) <a href="https://github.com/rive-app/rive-sharp">RiveSharp Github</a></li></ul> |
| Tizen                                  | [GitHub](https://github.com/rive-app/rive-tizen)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [React Native](overview/react-native/) | [npm](https://www.npmjs.com/package/rive-react-native), [GitHub](https://github.com/rive-app/rive-react-native)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

## Community runtimes

| Runtime | Author                                       | Link                                                            |
| ------- | -------------------------------------------- | --------------------------------------------------------------- |
| Vue.js  | Dan Nelson                                   | [GitHub](https://github.com/Coded-Clouds/Rive\_Vue\_ExampleApp) |
| Angular | François Guezengar                           | [GitHub](https://github.com/dappsnation/ng-rive)                |
| AWTK    | [Li XianJing](https://twitter.com/xianjimli) | [GitHub](https://github.com/zlgopen/awtk-widget-rive)           |

## Handling .riv Files

When checking in `.riv` files with Git, consider adding a `.gitattributes` file and marking `.riv` files as `binary` files to prevent Git from changing line endings when these files are checked in. Otherwise, some platforms may accidentally corrupt the `.riv` file where there are line returns (i.e. Windows CRLF line endings vs LF line endings) and cause issues at runtime when the file is read.

{% code title=".gitattributes" %}
```
*.riv binary
```
{% endcode %}

## Licensing

Our official runtimes are all open-source and licensed under the [MIT License](https://choosealicense.com/licenses/mit/). You're free to use them for personal and commercial applications.

## Contributing

Since all the runtimes are open-source, we encourage you to dive in and take a look around! If you see something missing or feel you can improve upon it, then fork it!
