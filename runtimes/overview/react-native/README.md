---
description: React Native runtime for Rive
---

# React Native

## Overview

This guide documents how to get started using the React Native runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-react-native).\
\
This library contains an API for React Native apps to easily integrate Rive assets.

The minimum iOS target is **14.0**

{% hint style="warning" %}
The Rive React Native runtime does not have support for Expo at this time
{% endhint %}

## Getting Started

Follow the steps below for a quick start on integrating Rive into your React Native app.

### 1. Install the dependency

```
npm install rive-react-native
# or for Yarn
yarn add rive-react-native
```

### 2. Pod Install

`cd` inside the `ios` folder and run `pod install` (if deploying to iOS)

{% hint style="info" %}
If you run into issues here, you may need to bump the `ios` deployment version target to at least `14.0`. You can find this version in the `Podfile` of the `ios/` folder.
{% endhint %}

### 3. Add the Rive component

{% code title="App.js" %}
```jsx
import Rive from 'rive-react-native';

function App() {
  return <Rive
      url="https://cdn.rive.app/animations/vehicles.riv"
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
