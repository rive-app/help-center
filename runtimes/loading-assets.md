---
description: Loading and replacing assets dynamically at runtime
---

# Loading Assets

Some Rive files may contain assets that can be embedded within the actual file binary, such as fonts or images. The Rive runtimes may then load these assets when the Rive file gets loaded in. While this makes for easy usage of the Rive files/runtimes, there may be opportunities to load these assets in or even replace them at runtime instead of embedding the assets in the file binary.

There are several benefits to this approach:

* Keep the `.riv` files tiny without potential bloat of larger assets
* Dynamically load an asset for any reason, such as loading an image with a smaller resolution if the `.riv` is running on a mobile device vs. an image of a larger resolution for desktop devices
* Preload assets to have available immediately when displaying your `.riv`
* Use assets already bundled with your application, such as font files
* Sharing the same asset between multiple `.riv`s

## Methods for Loading Assets

There are currently 3 different ways to load assets for your Rive files.

In the Rive editor select the desired asset from the **Assets** tab, and in the inspector choose the desired export option:

<figure><img src="../.gitbook/assets/CleanShot 2023-10-23 at 16.41.40@2x.png" alt="" width="235"><figcaption><p>Rive asset export options</p></figcaption></figure>

See the [#export-options](../editor/text/fonts.md#export-options "mention") section in the editor docs for more details.

### Embedded Assets

In the Rive editor, static assets can be included in the `.riv` file, by choosing the _"Embedded"_ export type. As stated in the beginning of this page, when the Rive file gets loaded, the runtime will implicitly attempt to load in the assets embedded in the `.riv` as well, and you don't need to concern yourself with loading any assets manually.

**Caveat:** Embedded assets may bulk up the file size, especially when it comes to fonts when using [Rive Text](../editor/text/)

{% hint style="info" %}
Embedded is the default option.
{% endhint %}

### Loading via CDN

In the Rive editor, you can mark an imported asset as a _"Hosted"_ export type, which means that when you export the `.riv` file, the asset will not be embedded in the file binary, but will be hosted on Rive's CDN. This means that at runtime when loading in the file, the runtime will see the asset is marked as "Hosted" and load the asset in from the Rive CDN, so that you don't need need to concern yourself with loading anything yourself, and the file can still remain tiny.

**Caveat:** The app will make an extra call to a Rive CDN to retrieve your asset

### Referenced Assets

In the Rive editor, you can mark an imported asset as a _"Referenced"_ export type, which means that when you export the `.riv` file, the asset will not be embedded in the file binary, and the responsibility of loading the asset will be handled by your application at runtime. This option enables you to dynamically load in assets via a handler API when the runtime begins loading in the `.riv` file. This option is preferable if you have a need to dynamically load in a specific asset based on any kind of app/game logic, and especially if you want to keep file size small.

All referenced assets, including the `.riv`, will be bundled as a zip file when you export your animation.

**Caveat:** You will need to provide an asset handler API when loading in Rive which should do the work of loading in an asset yourself. See [#handling-assets](loading-assets.md#handling-assets "mention") below.

## Handling Assets

See below for documentation on how to handle loading in assets at runtime for your Rive file with various runtimes.

{% hint style="info" %}
Note that we will progressively update the table below with docs on other runtimes as the functionality becomes available for each of them
{% endhint %}

{% tabs %}
{% tab title="Web (JS)" %}
### Examples

* [Specify a font asset to load](https://codesandbox.io/s/rive-out-of-band-fonts-d3lzs4)

### Using the Asset Handler API

When instantiating a new Rive instance, add an `assetLoader` callback property to the list of parameters. This callback will be called for every asset the runtime detects from the `.riv` file on load, and will be responsible for either handling the load of an asset at runtime or passing on the responsibility and giving the runtime a chance to load it otherwise.

An instance where you may want to handle loading an asset is if an asset in the file is marked as **Referenced**, and you need to provide an actual asset to render for the graphic, as Rive does not embed it in the `.riv` and thus cannot load it.

An instance where you may want to give the runtime a chance to load the asset is if the asset in the file is marked as **Hosted**, and want to pass the responsibility of loading it to the runtime (which will call into a Rive CDN to do so).&#x20;

```typescript
assetLoader: (asset: rc.FileAsset, bytes: Uint8Array) => boolean;
```

Your provided callback will be passed an `asset` and `bytes`.

* `asset` - Reference to a `FileAsset` object from WASM. You can grab a number of properties from this object, such as the name, asset type, and more. You'll also use this to set a new Rive-specific asset for the dynamically loaded in asset you want to set (i.e. `RenderImage` for an image, or `Font` for a font)
* `bytes` - Array of bytes for the asset (if possible, such as if it's an embedded asset)

**Important**: Note that the return value is a `boolean`, which is where you need to return `true` if you intend on handling and loading in an asset yourself, or `false` if you do not want to handle asset loading for that given asset yourself, and attempt to have the runtime try to load the asset.

{% hint style="warning" %}
When decoding an asset be sure to call `unref` once it is no longer needed - to avoid memory leaks. This allows the engine to clean it up when it is not used by any more animations.&#x20;
{% endhint %}

#### Example Usage

```typescript
// Load a random asset by using a decodeFont API to feed to a
// setFont API on the asset provided in assetLoader
const randomFontAsset = (asset) => {
  const urls = [
    "https://cdn.rive.app/runtime/flutter/IndieFlower-Regular.ttf",
    "https://cdn.rive.app/runtime/flutter/comic-neue.ttf",
    "https://cdn.rive.app/runtime/flutter/inter.ttf",
    "https://cdn.rive.app/runtime/flutter/inter-tight.ttf",
    "https://cdn.rive.app/runtime/flutter/josefin-sans.ttf",
    "https://cdn.rive.app/runtime/flutter/send-flowers.ttf",
  ];
  let randomIndex = Math.floor(Math.random() * urls.length);
  fetch(urls[randomIndex]).then(
    async (res) => {
      // decodeFont creates a Rive-specific Font object that `setFont()` takes
      // on the asset from assetLoader
      const font = await decodeFont(new Uint8Array(await res.arrayBuffer()));
      asset.setFont(font);
      
      // Be sure to call unref on the font once it is no longer needed. This 
      // allows the engine to clean it up when it is not used by any more animations.
      font.unref();
    }
  );
};


const riveInstance = new Rive({
  src: "acqua_text.riv",
  stateMachines: "State Machine 1", // Name of the State Machine to play
  canvas: document.getElementById("rive-canvas"),
  layout: new Layout({
    fit: Fit.Cover,
    alignment: Alignment.Center,
  }),
  autoplay: true,
  // Callback handler to pass in that dictates what to do with an asset found in
  // the Rive file that's being loaded in
  assetLoader: (asset, bytes) => {
    console.log("Asset properties to query", {
      name: asset.name,
      fileExtension: asset.fileExtension,
      cdnUuid: asset.cdnUuid,
      isFont: asset.isFont,
      isImage: asset.isImage,
      bytes,
    });

    // If the asset has a `cdnUuid`, return false to let the runtime handle
    // loading it in from a CDN. Or if there are bytes found for the asset
    // (aka, it was embedded), return false as there's no work needed here
    if (asset.cdnUuid.length > 0 || bytes.length > 0) {
      return false;
    }

    // Here, we load a font asset with a random font on load of the Rive file
    // and return true, because this callback handler is responsible for loading
    // the asset, as opposed to the runtime
    if (asset.isFont) {
        randomFontAsset(asset);
        return true;
    }
  },
  onLoad: () => {
    // Prevent a blurry canvas by using the device pixel ratio
    riveInstance.resizeDrawingSurfaceToCanvas();
  }
});
```
{% endtab %}

{% tab title="React" %}
### Examples

* [Specify a font asset to load](https://codesandbox.io/s/rive-out-of-band-fonts-react-6yljqv?file=/src/styles.css)

### Using the Asset Handler API

When instantiating a new Rive instance with the `useRive` hook, add an `assetLoader` callback property to the list of parameters. This callback will be called for every asset the runtime detects from the `.riv` file on load, and will be responsible for either handling the load of an asset at runtime or passing on the responsibility and giving the runtime a chance to load it otherwise.

{% hint style="info" %}
Note that you can only use the `assetLoader` callback with the `useRive` hook, and not the default-exported `<Rive />` component from the React runtime
{% endhint %}

```typescript
assetLoader: (asset: rc.FileAsset, bytes: Uint8Array) => boolean;
```

See the Web (JS) tab in this table for more API details.
{% endtab %}

{% tab title="Flutter" %}
### Examples

* [Swap out fonts dynamically](https://zapp.run/edit/rive-out-of-band-assets-fonts-zva0062lva10)
* [Swap out images dynamically](https://zapp.run/edit/rive-out-of-band-assets-image-z09q06hl09r0?entry=lib/main.dart\&file=pubspec.yaml:2865-2888)

### Using the Asset Handler API

When instantiating a `RiveFile`, add an `assetLoader` callback property to the list of parameters. This callback will be called for every asset the runtime detects from the `.riv` file on load, and will be responsible for either handling the load of an asset at runtime or passing on the responsibility and giving the runtime a chance to load it otherwise.

An instance where you may want to handle loading an asset is if an asset in the file is marked as **Referenced**, and you need to provide an actual asset to render for the graphic, as Rive does not embed it in the `.riv` and thus cannot load it.

An instance where you may want to give the runtime a chance to load the asset is if the asset in the file is marked as **Hosted**, and want to pass the responsibility of loading it to the runtime (which will call into a Rive CDN to do so).&#x20;

```dart
final riveAsset = await RiveFile.asset(
      'assets/acqua_text.riv',
      assetLoader: CallbackAssetLoader(
        (asset, bytes) async {
          if (asset is FontAsset) {
            asset.font = _fontCache[Random().nextInt(_fontCache.length)];
            _fontAssets.add(asset);
            return true;
          }
          return false;
        },
      ),
    );
```

Your provided callback will be passed an `asset` and `bytes`.

* `asset` - Reference to a `FileAsset` object. You can grab a number of properties from this object, such as the name, asset type, and more. You'll also use this to set a new Rive specific asset for dynamically loaded content
* `bytes` - Array of bytes for the asset (if possible, such as if it's an embedded asset)

**Important**: Note that the return value is a `boolean`, which is where you need to return `true` if you intend on handling and loading in an asset yourself, or `false` if you do not want to handle asset loading for that given asset yourself, and attempt to have the runtime try to load the asset.

#### Example Usage

Load and cache a collection of fonts. Here we demonstrate loading from Rive's CDN, but you can load these font files any way you desire.

```dart
// Load a random font asset by using a decodeFont API to feed to a
// setFont API on the asset provided in assetLoader
/// Create a local cache of random fonts
  Future<void> _warmUpCache() async {
    final futures = <Future>[];

    loadFont(url) async {
      final res = await http.get(Uri.parse(url));
      final body = Uint8List.view(res.bodyBytes.buffer);
      final font = await FontAsset.parseBytes(body);

      if (font != null) {
        _fontCache.add(font);
      }
    }

    for (var url in [
      'https://cdn.rive.app/runtime/flutter/IndieFlower-Regular.ttf',
      'https://cdn.rive.app/runtime/flutter/comic-neue.ttf',
      'https://cdn.rive.app/runtime/flutter/inter.ttf',
      'https://cdn.rive.app/runtime/flutter/inter-tight.ttf',
      'https://cdn.rive.app/runtime/flutter/josefin-sans.ttf',
      'https://cdn.rive.app/runtime/flutter/send-flowers.ttf',
    ]) {
      futures.add(loadFont(url));
    }

    await Future.wait(futures);
  }
```

Swap out the font dynamically:

```dart
class RiveRandomCachedFont extends StatefulWidget {
  const RiveRandomCachedFont({
    Key? key,
    required this.fontCache,
  }) : super(key: key);

  final List fontCache;

  @override
  State<_RiveRandomCachedFont> createState() => RiveRandomCachedFontState();
}

class RiveRandomCachedFontState extends State<RiveRandomCachedFont> {
  List get _fontCache => widget.fontCache;

  @override
  void initState() {
    super.initState();
    _loadRiveFile();
  }

  RiveFile? _riveFontSampleFile;
  final List<FontAsset?> _fontAssets = [];

  Future<void> _loadRiveFile() async {
  
    final riveAsset = await RiveFile.asset(
      'assets/acua_text.riv',
      // Provide an asset loader and manage the font assets manually.
      assetLoader: CallbackAssetLoader(
        (asset, bytes) async {
          if (asset is FontAsset) {
            asset.font = _fontCache[Random().nextInt(_fontCache.length)];
            _fontAssets.add(asset);
            return true;
          }
          return false;
        },
      ),
    );

    setState(() {
      // Keep a reference to the font asset, to swap out the font at any point.
      _riveFontSampleFile = riveAsset;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_riveFontSampleFile == null) {
      return const Center(child: CircularProgressIndicator());
    }

    return Column(
      children: [
        Expanded(
          child: RiveAnimation.direct(
            _riveFontSampleFile!, // Pass in the Rive file
            stateMachines: const ['State Machine 1'],
            fit: BoxFit.cover,
          ),
        ),
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: ElevatedButton(
            onPressed: () {
              for (var element in _fontAssets) {
                element?.font = _fontCache[Random().nextInt(_fontCache.length)];
              }
            },
            child: const Text('Random font asset'),
          ),
        ),
      ],
    );
  }
}
```
{% endtab %}

{% tab title="Android" %}
{% hint style="warning" %}
Note that the APIs are currently marked as experimental, and classes/methods using the asset-loading APIs should be annotated with `@ExperimentalAssetLoader`
{% endhint %}

### Using the Asset Handler API

When instantiating a new `RiveAnimationView`, set a new attribute called `riveAssetLoaderClass` whose value is a string name of the full path to a class that will be responsible for either handling the load of an asset at runtime or passing on the responsibility and giving the runtime a chance to load it otherwise.

**via XML**

```xml
<app.rive.runtime.kotlin.RiveAnimationView
    android:id="@+id/rive_font_load_simple"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:riveStateMachine="State Machine 1"
    app:riveAssetLoaderClass="app.rive.runtime.example.HandleRiveFontAsset"
    app:riveResource="@raw/acqua_text" />
```

In your accompanying activity, create a new class with the name provided to `riveAssetLoaderClass`, who should implement the `ContextAssetLoader` abstract class from the Rive runtime. Here, you can override a `loadContents` function, which will do the work of determining what assets (if any) to load:

* `asset` - Reference to a `FileAsset` object. You can grab a number of properties from this object, such as the name, asset type, and more. You'll also use this to set a new Rive-specific asset for the dynamically loaded in asset you want to set
* `bytes` - Array of bytes for the asset (if possible, such as if it's an embedded asset)

```kotlin
override fun loadContents(asset: FileAsset, inBandBytes: ByteArray): Boolean
```

**Important**: Note that the return value is a `boolean`, which is where you need to return `true` if you intend on handling and loading in an asset yourself, or `false` if you do not want to handle asset loading for that given asset yourself, and attempt to have the runtime try to load the asset.

#### Example Usage

To accompany the XML snippet above, here's an example of what the accompanying activity may look like:

```kotlin


package app.rive.runtime.example

import android.content.Context
import android.os.Bundle
import android.widget.FrameLayout
import androidx.appcompat.app.AppCompatActivity
import app.rive.runtime.kotlin.core.ExperimentalAssetLoader
import app.rive.runtime.kotlin.core.FileAsset


import app.rive.runtime.kotlin.core.ContextAssetLoader
import kotlin.random.Random

class FontLoadActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.rive_font_load_simple)
    }
}

@ExperimentalAssetLoader
open class HandleSimpleRiveAsset(context: Context) : ContextAssetLoader(context) {
    private val fontPool = arrayOf(
        R.raw.montserrat,
        R.raw.opensans,
    )
    /**
     * Override this method to customize the asset loading process.
     */
    override fun loadContents(asset: FileAsset, inBandBytes: ByteArray): Boolean {
        val randFontIndex = Random.nextInt(fontPool.size)
        val fontToLoad = fontPool[randFontIndex]
        context.resources.openRawResource(fontToLoad).use {
            // Load in the font bytes to the asset
            return asset.decode(it.readBytes())
        }
    }
}
```
{% endtab %}
{% endtabs %}

### Additional Resources

* [Exploring out of band assets - livestream](https://www.youtube.com/watch?v=BrWBmZwouQQ)
