---
description: Migration notes going from v1 to v2 of the JS runtime
---

# Migrating from v1 to v2

Starting in v2 of our Web (JS) runtimes, Rive will support Rive Text at runtime, which includes the following packages:

* Web (JS) / WASM
  * `@rive-app/canvas`
  * `@rive-app/canvas-advanced`
  * `@rive-app/webgl`
  * `@rive-app/webgl-advanced`
  * ...and other `@rive-app/*-single` variants

## Major Changes

{% hint style="success" %}
No breaking API changes!
{% endhint %}

While a new major version has been released for the runtimes without breaking API changes, v2 was released due to a **bundle** **size increase** in the package. The reason for this is new internal dependencies added to the Web Assembly (WASM) build that powers Rive, specifically for supporting the powerful Rive Text feature. You may find that the request for the WASM file when loading Rive is _\~261kb_ brotli-compressed as of `v2.0.0`.

In the future, we may introduce a "lite" package without these larger dependencies if you don't need the text feature, but for now, it will remain the default on the main web runtime packages.
