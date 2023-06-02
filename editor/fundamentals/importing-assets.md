# Importing Assets

Import your assets by dragging and dropping them onto the Rive Editor. You can import SVG, JSON, PNG, PSD, and JPG formats.

## Assets Panel&#x20;

After dragging in your graphics, they now appear in the Assets Panel, located in the left side of the editor UI. Drag and drop them onto an artboard to begin using them.

## Updating image assets

You can make updates to your images after they've been imported.&#x20;

Do this by selecting the image in the Assets Panel; the asset's properties appear in the Inspector, and a "Replace" button will be available for raster assets (PNG, JPG, PSD).&#x20;

Hit the replace button and when prompted, select the updated image. You'll notice that this updates all instances of the graphic used in the file.&#x20;



## Supported formats

Rive supports importing SVG (see limitations below), JSON, PNG, PSD, and JPG formats.

#### Copy and paste directly from Figma

You can use "copy as SVG" and paste it directly into the Rive editor.

<figure><img src="../../.gitbook/assets/2023-04-13 14.06.20.gif" alt=""><figcaption></figcaption></figure>

#### Import Lottie file

You can import your Lottie animations into Rive. To get started, drag and drop your Lottie JSON file into the Rive ediotr. this adds it to your Assets panel.

<figure><img src="../../.gitbook/assets/CleanShot 2023-04-12 at 17.05.03@2x.png" alt=""><figcaption></figcaption></figure>

From there, you can drag it into an exisiting artboard or drag it into an empty space to create a new artboard.

<figure><img src="../../.gitbook/assets/CleanShot 2023-04-12 at 17.08.12@2x.png" alt=""><figcaption></figcaption></figure>

### SVG Tips

SVG is a very flexible and feature-rich format. We aim to support SVG as best we can, however, there are some features that we do not support at this stage.&#x20;

When exporting files as SVG, exporting with inline style as opposed to CSS will work best for our importer.

When exporting from other design tools, look for the option to retain IDs and names of your shapes when you export. This will ensure that your imported file retains the same structure and layer names. Most tools have this option, as in the Figma example below.

![Figma's option to include "id" attribute](../../.gitbook/assets/figma\_export\_id.png)

#### Photoshop:

When exporting from Photoshop, make sure you're only using vector layers. Don't convert or flatten anything to raster.

#### Illustrator:

When using "Save As" to export an SVG from Illustrator, select "Style Attributes" from the CSS Properties instead of the default option. Thanks to [V.lang on our feedback site for figuring this out](https://feedback.rive.app/122)! Be sure to also disable the "Preserve Illustrator Editing Capabilities" as this will make your file much larger and add data that is not recognized by our importer.&#x20;

![Illustrator's Save As SVG panel](<../../.gitbook/assets/image (2).png>)

#### Known Issues:&#x20;

* Embedded images are ignored, we are planning to implement this (for more info [see here](https://feedback.rive.app/69)).
* Gradient transforms are ignored.&#x20;
  * We currently cannot provide equal support for this across our runtimes, so this is not supported.
  * We do support linear and radial gradients, however, which can cover some use cases.
* Rive does not have a concept of point (pt) or millimeter (mm) sizing. An SVG that uses dimensions provided in pt or mm will have its values converted to pixels (px). Points are converted to 1.33 px and millimeters are converted to  3.78 px.&#x20;
* SVG provides `inherit` to let stroke and fills use the color of their ancestors. Rive does not support this and any inherited color defaults to white.
* Other unsupported SVG features:
  * `stroke-dasharray` - you may see a solid stroke line instead
  * `mask` -  we treat this like clipping
  * `filter`
  * `skew`
