# Text Runs

{% embed url="https://www.youtube.com/watch?v=a4drmh2S-eg&list=PLujDTZWVDSsFGonP9kzAnvryowW098-p3&index=41" %}

Runs allow you break your text up into sections — typically, they're used to apply a variety of styles to a single block of text. Whilst most tools manage text runs behind the scenes, Rive exposes them for greater control when dynamically changing text at runtime.

You may want to split your text into multiple runs to apply a different style (such as font, font size, color etc.) to a certain part of your text, or if you only want to [replace a specific word or section at runtime](../../runtimes/text.md#read-update-text-runs-at-runtime).

{% hint style="info" %}
A Text Run may only have one Text Style applied at a time.
{% endhint %}

For example, an animation welcoming a user to an app or website may greet them by their name. In the Rive Editor, you may design and animate the text to read "Welcome back, username". Defining "username" as it's own run means you can target it with the Rive Runtimes and replace it with the user's name.

<figure><img src="../../.gitbook/assets/2023-07-24 14.05.14.gif" alt=""><figcaption><p>Update text for a specific run</p></figcaption></figure>

***

## Creating a Text Run

To create a Run, select the desired portion of text and select the 'Run from Selection' button in the Inspector. You can see Text Runs listed beneath the text object in the hierarchy.

{% hint style="info" %}
Double click or press `Enter` with the text box selected to start editing text.
{% endhint %}

{% hint style="info" %}
Toggle the 'Highlight Text Runs' option in the inspector for a visual guide of your current Text Runs. Hovering a run in the hierarchy also highlights its location within the text.
{% endhint %}

<div data-full-width="false">

<figure><img src="../../.gitbook/assets/2023-07-24 13.54.09.gif" alt=""><figcaption><p>Split text into multiple runs</p></figcaption></figure>

</div>

***

## Managing Text Runs

Select a Text Run in the hierarchy for inspector options:

* **Text Value:** Update the text value for the run. Key this value in animate mode.
* **Edit Text Run:** Initiate the text editor with the run pre-selected.
* **Merge with Next:** Combine the selected run with the next run.
* **Merge with Previous:** Combine the selected run with the previous run.
* **Delete text Run:** Delete the run and its contents.
* **Style:** Assign one of the Text Styles defined on the text object. Key this value in animate mode.

<figure><img src="../../.gitbook/assets/2023-07-24 14.05.56.gif" alt=""><figcaption><p>Assign a Text Style to a Text Run</p></figcaption></figure>
