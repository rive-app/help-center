# Distance Constraint

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/distance-contraint/doc8CTqEhcbP).
{% endhint %}

The Distance Constraint makes an object stay close, far, or exactly at a specific distance to another object.

{% embed url="https://youtu.be/Nvwf27EIvdw" %}

## How to create a Distance Constraint

### 1. Add a Distance Constraint to an object

Use the Constraints section of the Inspector to add a Distance Constraint to an object.

![](../../.gitbook/assets/2021-08-05-16.52.56.gif)

### 2. Choose a target

Use the new constraint's fly-out menu to select a target for this constraint.

![](../../.gitbook/assets/2021-08-05-16.53.17.gif)

### 3. Test that the Distance Constraint is working

Moving the target object now causes the constrained object to stay close (which is the default [mode](distance-constraint.md#mode)).

![](../../.gitbook/assets/2021-08-05-16.59.24.gif)

## Strength <a href="#target" id="target"></a>

The Strength property determines how much the constrained object is affected.

A Strength of 0% means the constraint won't have any effect.

## Distance <a href="#distance" id="distance"></a>

The distance that the object will be constrained from the target object. A red constraining circle is drawn on the stage to represent this value.

## Mode <a href="#mode" id="mode"></a>

### Closer <a href="#closer-than" id="closer-than"></a>

The owner is constrained closer than the Distance setting. In other words, the owner is constrained inside the constraining sphere.

### Further <a href="#further-than" id="further-than"></a>

The owner is constrained further than the Distance setting. In other words, the owner is constrained outside the constraining sphere.

### Exactly <a href="#exactly" id="exactly"></a>

The owner is constrained exactly at the distance of the constraining sphere.
