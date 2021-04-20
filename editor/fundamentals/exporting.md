---
description: Learn how to export your Rive files
---

# Exporting files

## **Via the Editor**

![](../../.gitbook/assets/screen-shot-2021-04-20-at-3.30.35-pm.png)

Export your file via the export menu within the toolbar. Select Download from the dropdown menu.

### For newest runtime

This option downloads a **.riv** file that is compatible with the latest major version of the Rive runtimes \(number displayed on the right\). You can use the [Get runtimes](../../runtimes/overview.md) button to find all our latest runtimes. 

{% hint style="info" %}
Note that **.riv** files are optimized for runtime so any non-runtime-related data \(like x/y coordinates of states\) are stripped out.
{% endhint %}

### Previous

This option downloads a **.riv** file that is compatible with the previous major version of the Rive runtimes \(number displayed on the right\). Use this if you haven't updated to the latest major version. 

### For backup

This option downloads a **.rev** file that is intended for backup. It contains all the data that gets stripped out from runtime files. If you drag this file back into the Editor, it'll restore all properties \(including coordinates of state machine states\). 

## **Via the File Browser**

![](../../.gitbook/assets/export%20%281%29.png)

Right-click files from within the browser and select Export from the contextual menu. Use marquee selection by clicking and dragging to export multiple files at once.

Alternatively, right-click on a folder to export all the containing files.

