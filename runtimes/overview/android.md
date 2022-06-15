---
description: Android runtime for Rive
---

# Android

## Overview

This guide documents how to get started using the Android runtime library. Rive runtime libraries are open-source. The source is available in its [GitHub repository](https://github.com/rive-app/rive-android).\
\
This library contains an API for Android apps to easily integrate Rive assets.

## Getting Started

Follow the steps below for a quick start on integrating Rive into your Android app.

### 1. Add the Rive dependency

Add the following dependencies to your `build.gradle` file in your project:

```
dependencies {
    implementation 'app.rive:rive-android:3.0.10'
    // During initialization, you may need to add a dependency
    // for Jetpack Startup
    implementation "androidx.startup:startup-runtime:1.1.0"
}
```

### 2. Initializing Rive

Rive needs to initialize its runtime when your app starts.

It can be done via an [initializer](https://developer.android.com/topic/libraries/app-startup) that does this for you automatically. The initialization provider can be set up directly in your app's manifest file:

```xml
<provider
  android:name="androidx.startup.InitializationProvider"
  android:authorities="${applicationId}.androidx-startup"
  android:exported="false"
  tools:node="merge">
    <meta-data android:name="app.rive.runtime.kotlin.RiveInitializer"
      android:value="androidx.startup" />
</provider>
```

Otherwise this can be achieved by calling the initializer in your code:

```kotlin
AppInitializer.getInstance(applicationContext)
  .initializeComponent(RiveInitializer::class.java)
```

If you want to initialize Rive yourself, this can be done in code:

```kotlin
Rive.init(context)
```

### 3. Add RiveAnimation to your layout

The simplest way to get a Rive animation into your application is to include it as part of a layout. The following will consist of the Rive file loaded from the raw resources and auto-play its first animation. This assumes you have taken a downloaded `.riv` file (i.e `off_road_car_blog.riv`) and placed it in your raw resources folder.

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:riveResource="@raw/off_road_car_blog" />
```

Another way to load a Rive file in is by referencing the URL where the asset lives (see Internet Permissions section below for an extra step in setup):

{% code title="simple.xml" %}
```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:riveUrl="https://cdn.rive.app/animations/vehicles.riv" />
```
{% endcode %}

When setting your context views, this might look like

```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class SimpleActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.simple)
    }
}
```

### Internet permissions

If you're retrieving Rive files over a network, your app will need permission to access the internet:

{% code title="AndroidManifest.xml" %}
```markup
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
{% endcode %}

Note that this isn't necessary if you include the files in your Android project and load these in as a raw resource.

See subsequent Runtime pages to learn how to control animation playback, state machines, and more.

## Resources

Github: [https://github.com/rive-app/rive-android](https://github.com/rive-app/rive-android)\
Examples: [https://github.com/rive-app/rive-android/tree/master/app/src/main/java/app/rive/runtime/example](https://github.com/rive-app/rive-android/tree/master/app/src/main/java/app/rive/runtime/example)
