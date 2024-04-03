---
description: Rive React Native Expo
---

# Adding Rive to Expo

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/adding-rive-to-expo/docFSwIlblYi).
{% endhint %}

To enable your Expo application to run Rive animations you'll need to include the native Android and iOS libraries.

To achive this you can make use of [development builds](https://docs.expo.dev/develop/development-builds/introduction/) or by [generating the native projects with prebuild](https://docs.expo.dev/workflow/customizing/#generate-native-projects-with-prebuild). A development build can be seen as your own version of the Expo Go client that includes custom native libraries. We suggest reading [Expo's documentation](https://docs.expo.dev/workflow/customizing/#generate-native-projects-with-prebuild) for more information.

### Setup

This guide will demonstrate how you can make use of prebuild to generate the required native code.

Start by creating a new expo app (or use an existing project):

```bash
npx create-expo-app MyRiveApp
```

Add the Rive React Native package:

```
npx expo install rive-react-native
```

Add a Rive animation to a view:

{% code title="App.js" %}
```javascript
import Rive from 'rive-react-native';
import { StyleSheet, Text, View } from 'react-native';

function RiveDemo() {
  return <Rive
      url="https://public.rive.app/community/runtime-files/2195-4346-avatar-pack-use-case.riv"
      artboardName="Avatar 1"
      stateMachineName="avatar"
      style={{width: 400, height: 400}}
  />;
}

export default function App() {
  return (
    <View style={styles.container}>
      <RiveDemo />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```
{% endcode %}



{% hint style="danger" %}
Running **`npx expo start`** will not work as Expo Go does not include the native libraries needed to run Rive. If you were to run this command you'll see the following error:\
_`Invariant Violation: requireNativeComponent: "RiveReactNativeView" was not found in the UIManager.`_
{% endhint %}

Before running your app, you first need to generate the iOS and Android native projects to take ownership of them. You can do this by running `npx expo prebuild`, or `npx expo run:[ios|android]` (which will run `prebuild` automatically).

* `npx expo run:android` requires Android Studio and the Android SDK to be installed. See the [setup environment guide](https://reactnative.dev/docs/environment-setup).
* `npx expo run:ios` requires Xcode (macOS only) installed on your computer. See the [setup environment guide](https://reactnative.dev/docs/environment-setup).

Generate the build folders for both Android and iOS:

```
npx expo prebuild
```

You'll be prompted to enter an **Android package** and **iOS Bundle Identifier**.

This command will generate `android` and `ios` build folders in your project directory. These projects allow you to configure your iOS and Android application as you would for a traditional native application.

{% hint style="warning" %}
You may get the following error:\
`Something went wrong running pod install in the ios directory.`

Which is likely due to the minimum target iOS version not being high enought.
{% endhint %}

Ensure that your apps minimum iOS version is at least `14.0` (this is the minimum version that Rive iOS supports). Open `ios/Podfile` and look for the `platform :ios` version. It should look something like this:

```ruby
platform :ios, podfile_properties['ios.deploymentTarget'] || '13.0'
```

Change `13.0` to `14.0`:

```ruby
platform :ios, podfile_properties['ios.deploymentTarget'] || '14.0'
```

You can now run on iOS:

<pre class="language-bash"><code class="lang-bash"><strong># Build your native Android project
</strong><strong>npx expo run:ios
</strong></code></pre>

and Android:

```bash
# Build your native iOS project
npx expo run:android
```

### Local Assets

The example above demonstrates how to load a `riv` file as a network asset. To load files from the asset bundle they need to be included in Android Studio and XCode as assets, see [Loading in Rive Files](loading-in-rive-files.md).

You can also see [this Github Issue](https://github.com/rive-app/rive-react-native/issues/185) for an alternative approach to load assets.
