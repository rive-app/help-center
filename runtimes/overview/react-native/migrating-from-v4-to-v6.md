---
description: Migrating from v4 to v6
---

# Migrating from v4 to v6

If you are migrating to `rive-react-native` note the following changes below. Since there were not any minor/patch versions released on `v5`, we're consolidating the migration guidance from `v4` to `v5` and `v5` to `v6`, which we recommend migrating to stay up-to-date.

{% hint style="success" %}
No breaking API changes!
{% endhint %}

## Dependencies

### React Native

We updated the `react-native` and `react` devDependencies to support the following versions:

* React: `18.2.0`
* React Native: `0.72.3`

Out of an abundance of caution, we've bumped a major version on `rive-react-native` to account for any discrepancies that arise from going from `v0.66.4` -> `v0.72.3`.

### Rive Android

We also bumped the [rive-android](https://github.com/rive-app/rive-android) runtime dependency from `v5.1.5` to `v8.1.0` to support the Rive follow-path feature and [Text](../../text.md) feature. A slight bundle size increase may be seen as it introduces a Text Engine dependency.

If you run into issues building, you may try setting a higher NDK version.

{% hint style="info" %}
The `rive-android` NDK version defines the `ndkVersion` in `build.gradle` at `"25.1.8937393"`
{% endhint %}

You may want to consult the `rive-react-native` [example app](https://github.com/rive-app/rive-react-native/tree/main/example) included in the repository to see an example configuration to follow if you run into issues.

### Rive iOS

We also bumped the rive-ios runtime dependency from `v4.0.5` to `v5.0.1` to support the [Text](../../text.md) feature. A slight bundle size increase may be seen as it introduces a Text Engine dependency.

## New APIs

### Dynamic Text

Starting in `v6.0.0`, `rive-react-native` has support for not only displaying Rive graphics that include [Rive Text](../../../editor/text/), but the ability to set text dynamically via the Rive ref.

Check the [Rive ref methods page](rive-ref-methods.md) to see how to set text run values.
