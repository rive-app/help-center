---
description: Out-of-band assets in Rive Unity
---

# Loading Assets

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/loading-assets/doc1etuJJdEC).
{% endhint %}

See the [runtime documentation](../../runtimes/loading-assets.md) for more information on loading assets out-of-band.

{% hint style="warning" %}
Only **embedded** and **referenced** assets are supported in Rive Unity; **hosted** assets are not currently supported.
{% endhint %}

{% hint style="warning" %}
Only **png** image assets are supported. Support for **webp** and **jpeg** is in progress.
{% endhint %}

## Asset export options

Within the Rive Editor, you can select an asset (for example, an image or font) in the **Asset Panel** and configure the export option for that asset. A Rive file can have a mixture of **embedded**, **referenced**, and **hosted** assets.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 15.04.59.png" alt=""><figcaption><p>Asset export options</p></figcaption></figure>

**Embedded** assets are included with the exported `.riv` binary file, while **referenced** assets are packaged separately and must be linked at runtime. Using **referenced** assets enables you to reuse the same asset across multiple animation files or in other parts of your game. This reduces the size of your `.riv` file and the resources needed to run your animations that use a shared asset.

### Embedded Assets

Any asset marked as embedded will automatically be loaded, and you do not need to do anything to configure the asset.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 15.09.34.png" alt=""><figcaption><p>Unity Inspector: riv file with an embedded asset</p></figcaption></figure>

By selecting the riv file you'll get information on the file's assets in the **Unity Inspector**. The example image above shows an embedded font called "Roboto Flex" with a size of 1MB.

### Referenced Assets

Referenced assets need to be linked at runtime. The rive-unity package automatically handles the linking by attempting to find assets within the same directory that match the correct **Name** + **ID** combination. If an asset that matches the criteria is discovered, that asset is automatically converted to a Rive asset and linked.

{% hint style="info" %}
For an asset to be discoverable and linked, the riv file and asset must be in the same Unity directory.
{% endhint %}

#### Let's take a look at an example

When exporting your runtime file from the Rive Editor,  the `.riv` file and referenced assets are exported in a zip.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 15.23.10@2x.png" alt=""><figcaption><p>Extracted Rive zip file with a referenced asset</p></figcaption></figure>

The extracted zip has an `acqua_text.riv` file with a referenced asset named `Roboto Flex-887377.ttf`. The referenced asset file name breaks down as such:

* **Name:** Roboto Flex
* **ID:** 887377

Selecting both files (or the entire folder) and dragging them into the **Unity Assets** folder will automatically link the embedded font file - if the **Name** + **ID** matches what the `.riv` file expects.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 15.49.42.png" alt=""><figcaption><p>Unity Inspector: riv file with a referenced asset</p></figcaption></figure>

In the **Unity Inspector** for the Rive file, you can note that the **Asset** for the "Roboto Flex" font has been linked, and the **Roboto Flex** font file has also been converted to a Rive asset.

This example automatically converted and linked the font file because the animation and font files were added simultaneously. Alternatively, you can add the asset file first and then the riv file.; this will result in the same outcome.

If an asset was added after the riv file, you'll need to manually reimport the riv file by **right-clicking** it and selecting **Reimport**.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 15.56.45.png" alt=""><figcaption><p>Reimport riv file in Unity</p></figcaption></figure>

#### Selecting an importer

Alternatively, you can set the desired Importer for an asset by selecting the correct option in the Inspector. This means you can make an asset a Rive Asset or change back an incorrectly converted asset.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 17.08.06.png" alt=""><figcaption><p>Selecting the Rive Importer</p></figcaption></figure>

#### Sharing Assets

Reusing the same asset in your Rive files should result in a consistent **Name** + **ID** generation when exporting from the Rive Editor.

If a matching asset is not found when importing in Unity, there is a fallback to converting and linking a file that matches only the **Name**. You can optionally use this approach to have more control over asset importing, as an asset filename can be renamed in the Rive Editor.

<figure><img src="../../.gitbook/assets/CleanShot 2023-12-07 at 17.15.45.png" alt=""><figcaption><p>Rive Editor: Change asset filename</p></figcaption></figure>
