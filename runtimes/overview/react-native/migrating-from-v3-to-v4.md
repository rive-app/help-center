---
description: Migrating guide from < 4.x
---

# Migrating from v3 to v4

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/migrating-from-v3-to-v4/docGpKiBm5cb).
{% endhint %}

If you are migrating to `rive-react-native` note the following changes below.

## Breaking API Changes

### Ref API

* `.play()` - Invoking the `.play()` API on the Rive `ref`&#x20;
  * `animationNames: [String]` -> `animationName: String`
    * To play a singular linear animation, use the `animationName` property
    * To play (and mix) multiple linear animations together, you will have to do this via a state machine and pass in `true` for the `isStateMachine` flag in this `.play()` API
* `.pause()` - Invoking the `.pause()` API on the Rive `ref`
  * This API no longer requires passing any arguments to invoke, as it will pause a singular playing animation or state machine
* `.stop()` - Invoking the `.stop()` API on the Rive `ref`
  * This API no longer requires passing any arguments to invoke, as it will stop a singular playing animation or state machine

### General Usage

* When rendering the `<Rive>` component, we **highly** recommend specifying either an `animationName` or `stateMachineName` prop explicitly, otherwise, if you do not provide either of these props:
  * `iOS` may play the first state machine it finds in the Artboard as a default
  * `Android` may play the first linear animation it finds in the Artboard as a default

{% hint style="info" %}
Note, the default behavior will change in a next major release, whereby the default play will be a state machine
{% endhint %}

### React Native

* Bumping the `react-native` dependency from `v0.63.4` to `v0.65.0`
* Bumping the `react` dependency from `16.13.1` to `v17.0.2`

### Android

* Bumping the Kotlin version from `1.5.20` to `1.7.10`

{% hint style="warning" %}
If you intend to run Rive on Android devices, please update to `4.0.1` at the least, which includes a crucial bug fix on the render lifecycle
{% endhint %}
