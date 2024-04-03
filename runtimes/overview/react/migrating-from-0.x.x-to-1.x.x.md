---
description: Migrating to 1.x.x
---

# Migrating from 0.x.x to 1.x.x

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/migrating-from-0xx-to-1xx/docUimWbB61Q).
{% endhint %}

Before v1.0.0 of `rive-react`, the React runtime wrapped the `@rive-app/canvas` runtime dependency, using the backing `CanvasRenderingContext2D`. In order for the React runtime to support some upcoming advanced drawing features like Mesh Deformations, it needed to use the `@rive-app/webgl` runtime.\
\
The major version bump to v1.0.0 involves no breaking API changes, but rather internally uses a different backing context with WebGL. If you have issues, please log them to the issues tab of the `rive-react` runtime [here](https://github.com/rive-app/rive-react/issues).&#x20;
