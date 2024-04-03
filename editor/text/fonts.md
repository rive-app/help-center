# Fonts

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/fonts/docIMUINFL5T).
{% endhint %}

Select a font in the inspector as part of a Text Style. The default list is populated via Google Fonts, however you can also upload your own fonts to the Assets Panel with a Studio plan.

## Font Options

Font variables and OpenType features can be found in the options fly-out of the Text Styles subsection. If supported by the font, you can find it alongside the font weight dropdown.

{% hint style="success" %}
Font variables can be animated. Open the options fly-out in animate mode to add keys.
{% endhint %}

{% hint style="warning" %}
Not all fonts support variable axes and OpenType features. The options menu will only surface for supported fonts.
{% endhint %}

<figure><img src="../../.gitbook/assets/2023-07-24 15.18.58.gif" alt=""><figcaption><p>Configure variables and features for compatible fonts</p></figcaption></figure>

***

## Font Sources

### Google Fonts

The default font list is populated via Google Fonts. These fonts are available to use on all plans. After selecting a font it'll appear in the Assets Panel under the 'Fonts' category. This is true for both custom _and_ Google fonts. The latter will update automatically based on the fonts you're using within the Rive file.

{% hint style="info" %}
Fonts provided by Google Fonts are marked with a G icon.
{% endhint %}

### Custom Fonts

Upload custom fonts by drag-and-dropping your font file into the editor or via the `+` action in the Assets Panel. Uploaded fonts will show at the top of the font selection drop down in the text inspector.

{% hint style="warning" %}
Custom fonts require a Studio plan.
{% endhint %}

{% hint style="info" %}
Select a font in the Assets Panel to view available metadata and licenses in the Inspector.
{% endhint %}

***

## Export Options

Your Rive file will need to reference a font file if you're looking to dynamically update text at runtime. Configure the available options to suit your needs and optimise your `.riv` files. Select a font in the Assets Panel to reveal its export options in the Inspector.&#x20;

### Export Type

Change the export type option to define where you'd like a font file to export to.

* **Embedded:** Embed the font file inside the `.riv`. Embedding the font inside the Rive file is the simplest option, however will increase the size of the file. Use this option if the font is unique to your animation, and isn't already available inside your app, website, or game.
* **Referenced:** Export the font file alongside the `.riv`. This option is ideal if you have multiple Rive files using the same font, or if you're implementing a Rive file into an app, game, or website that already has the font file available. Using a referenced font file will reduce the size of your Rive files.
* **Hosted:** Upload the font file to Rive's CDN for a runtime to download from on demand. Similar to a referenced font file, choosing to host the font on Rive's CDN will omit it from the `.riv`, thus reducing the `.riv` file size. The Rive runtimes will fetch the font when your animation plays in your app, game, or website.

{% hint style="warning" %}
Assets hosted on Rive's CDN can be accessed by anyone with the link.
{% endhint %}

***

### Glyph / Script Selection

Alongside the font location, you can configure the glyphs you wish to be included. Removing unnecessary glyphs and scripts will reduce the size of the font file (referenced/hosted) or Rive file (embedded). Deciding which scripts to include will depend on whether you want to dynamically update text at runtime, and the languages you want to support.

{% hint style="info" %}
Use the 'View Glyphs' feature to browse what glyphs / scripts are available for your chosen font. You can find it in the Inspector after selecting a font in the Assets Panel.
{% endhint %}

<figure><img src="../../.gitbook/assets/2023-07-24 15.03.49.gif" alt=""><figcaption><p>Use the viewer to browse a font's available glyphs and scripts</p></figcaption></figure>

**Choose from three options**

* **All Glyphs:** Export the entire font file. All the glyphs will be available to use at runtime.
* **Glyphs Used:** Only exports glyphs used within the file. For example, if your text says "Hello World!", only the `H`,`e`,`l`,`o`,`W`,`r`,`d`, and `!` glyphs would be exported. Use this option if you don't intend to change the text at runtime.
* **Custom:** Export chosen scripts. Use the script options fly-out to toggle scripts on and off. Use this option if you want to dynamically update text at runtime, but don't need to support certain languages or alphabets. This will help reduce file size by removing scripts you don't plan to use.

<figure><img src="../../.gitbook/assets/2023-07-24 14.59.20.gif" alt=""><figcaption><p>Remove unwanted scripts to optimize font exports</p></figcaption></figure>
