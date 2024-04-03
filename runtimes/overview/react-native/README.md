---
description: React Native runtime for Rive
---

# React Native

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/react-native/docvvuSp165E).
{% endhint %}

## Overview

This guide documents how to get started using the React Native runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-react-native).\
\
This library contains an API for React Native apps to easily integrate Rive assets.

The minimum iOS target is **14.0**

{% hint style="success" %}
See [our documentation](adding-rive-to-expo.md) to add Rive to an Expo app.
{% endhint %}

## Getting Started

Follow the steps below for a quick start on integrating Rive into your React Native app.

### 1. Install the dependency

```
npm install rive-react-native
# or for Yarn
yarn add rive-react-native
```

### 2a. iOS - Pod Install

`cd` inside the `ios` folder and run `pod install` (if deploying to iOS)

{% hint style="info" %}
If you run into issues here, you may need to bump the `ios` deployment version target to at least `14.0`. You can find this version in the `Podfile` of the `ios/` folder.
{% endhint %}

### 2b. Android - Set Kotlin Dependency Resolution

This step may be optional - however, if your Android setup in the React Native project does not have Kotlin `v1.8.0+` set up, you may run into duplicate class issues when building the project. To mitigate these issues, as suggested by [Kotlin docs](https://kotlinlang.org/docs/gradle-configure-project.html#versions-alignment-of-transitive-dependencies), add the following to your dependencies in your application's `build.gradle` file to deal with version alignment:

<pre class="language-gradle"><code class="lang-gradle"><strong>dependencies {
</strong><strong>    implementation platform('org.jetbrains.kotlin:kotlin-bom:1.8.0')
</strong><strong>    ...
</strong><strong>}
</strong></code></pre>

### 3. Add the Rive component

{% code title="App.js" %}
```jsx
import Rive from 'rive-react-native';

function App() {
  return <Rive
      url="https://public.rive.app/community/runtime-files/2195-4346-avatar-pack-use-case.riv"
      artboardName="Avatar 1"
      stateMachineName="avatar"
      style={{width: 400, height: 400}}
  />;
}
```
{% endcode %}

See subsequent runtime pages to learn how to control animation playback, state machines, and more.

## Resources

Github: [https://github.com/rive-app/rive-react-native](https://github.com/rive-app/rive-react-native)\
Examples:

* Demo App: [https://github.com/rive-app/rive-react-native/tree/main/example](https://github.com/rive-app/rive-react-native/tree/main/example)
* Weather App: [https://github.com/rive-app/weather-app-mobile](https://github.com/rive-app/weather-app-mobile)
