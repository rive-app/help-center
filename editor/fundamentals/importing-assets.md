# Importing Assets

Import your assets by dragging and dropping them onto the Rive editor. In doing so, the editor generates an artboard and associated hierarchy for you to begin working with.&#x20;

![](../../.gitbook/assets/import.gif)

### Assets panel&#x20;

After dragging in your graphics, they now appear in the Assets Panel, located at the bottom of the hierarchy. Drag and drop them onto an artboard to begin using them.

## Updating graphics

You can make updates to your graphics after they've been imported.&#x20;

Do this by selecting the image in the Assets Panel. Notice that the assets properties appear in the inspector.&#x20;

Hit the replace button and when prompted, select the updated image. You'll notice that this updates all instances of the graphic used in the file.&#x20;



## Supported formats

Currently, Rive supports importing SVG (see limitations below), png, PSD, and jpeg/jpg formats. We'll be adding a range of other formats soon!

## SVG Tips

SVG is a very flexible and feature-rich format. We aim to support SVG as best we can, however, there are some features that we do not support at this stage.&#x20;

When exporting files as SVG, exporting with inline style as opposed to CSS will work best for our importer.

When exporting from other design tools, look for the option to retain ID's and names of your shapes when you export. This will ensure that your imported file retains the same structure and layer names. Most tools have an option for this, as in the Figma example below.

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

**Coming soon**

We plan to add raster image support, [track the feature on our public roadmap and feedback site](https://feedback.rive.app/69).
