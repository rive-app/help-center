---
description: How to use Rive files with the Rive React Native runtime
---

# Loading in Rive Files

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/loading-in-rive-files/doc8P5oDeLlH).
{% endhint %}

There are two ways to include Rive files in your React Native projects:

* Option 1: URL where a Rive file is hosted
* Option 2: Add the asset to the asset bundles of the native iOS and Android projects

When you render the `<Rive />` component, you must supply the `url` or `resourceName` prop respectively to the options above, or your component will fail to load.

Read more below to see more on each of the options.

### Option 1: URL

<pre class="language-jsx"><code class="lang-jsx">&#x3C;Rive
<strong>  url="https://cdn.rive.app/animations/vehicles.riv"
</strong>/>;
</code></pre>

When using the Rive React Native runtime to load in a RIve file, one option is to reference the URL where the Rive file may be hosted (i.e AWS S3 bucket, Google Storage, etc.). This can be done via the `url` parameter when instantiating the `<Rive />` component.

### Option 2: Asset Bundle

```jsx
<Rive
  resourceName="weather_app" // weather_app.riv
/>;
```

Another alternative to loading in a Rive file for the `<Rive />` component is to reference the name of the resource/asset in the respective `ios/` and `android/` projects.

#### Adding to iOS

In the `ios/` folder of your React Native project, open the `.xcodeproj` file in XCode. This will open up the native iOS project.

Create a _New Group_ under the root of this project and name it whatever asset folder name you'd like to give it (i.e., _Assets_). Drop your `.riv` file into this group, and when prompted by XCode, add it to the _Target_ of your app. This ensures that the Rive file gets included in the bundle resources.

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-08-25 at 10.20.00 PM.png" alt=""><figcaption><p>Adding <code>weather_app.riv</code> to the iOS project</p></figcaption></figure>

#### Adding to Android

In the `android/` folder of your React Native project, open the whole folder in Android Studio. This will open up the Android project.&#x20;

Under the `/app/src/main/res/` directory, create a new _Android Resource Directory_, which is where you'll store Rive file assets, and when prompted to select a name for the folder and resource type, select `raw` from the resource type dropdown.\
\
Drop your `.riv` file into this new folder; this ensures that the Rive file gets included in the bundle resources.

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-08-25 at 10.29.37 PM.png" alt=""><figcaption><p>Adding <code>weather_app.riv</code> to the Android project</p></figcaption></figure>

Once the Rive files are added to the asset/resource bundles of the iOS and Android projects in the React Native app, you should be free to start referencing the name of the file (without the `.riv` extension) when creating the `<Rive />` component, using that `resourceName` prop.
