---
description: Migration notes going from v3 to v4 of the React runtime
---

# Migrating from v3 to v4

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/migrating-from-v3-to-v4/dociIPXVHKFF).
{% endhint %}

Starting in v4 of our React runtimes, Rive will support Rive Text at runtime, which includes the following packages:

* React
  * `@rive-app/react-canvas`
  * `@rive-app/react-webgl`

## Major Changes

{% hint style="success" %}
No breaking API changes!
{% endhint %}

While a new major version has been released for the runtimes without breaking API changes, v4 was released due to a **bundle** **size increase** in the package from the core Web (JS) runtime dependency. See that migration guide [here](../web-js/migrating-from-v1-to-v2.md).

In the future, we may introduce a "lite" package without these larger dependencies if you don't need the text feature, but for now, it will remain the default on the main web runtime packages.
