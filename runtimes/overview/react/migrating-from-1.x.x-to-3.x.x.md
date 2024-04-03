---
description: Migrating to v3.x.x
---

# Migrating from 1.x.x to 3.x.x

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/migrating-from-1xx-to-3xx/docB2sJyj3b3).
{% endhint %}

For the most part, if you're using v1.x.x of `rive-react`, you should be able to upgrade to the new dependencies in v3.x.x without many changes.

{% hint style="info" %}
Note: Migrating from v2.x.x to 3.x.x can be done safely without any changes on your end.
{% endhint %}

## Dependency Change

Previous to v2.x.x, you could use Rive in React via the `rive-react` package. This package was a wrapper around the `@rive-app/webgl` dependency. In version 2.x.x+, we enable the React runtime to wrap around both the `@rive-app/webgl` and `@rive-app/canvas` dependencies through 2 main new React packages:

* **(Recommended)** `@rive-app/react-canvas`&#x20;
* `@rive-app/react-webgl`

The `rive-react` package will still be published to regularly along with the above packages, but it has both of the web runtime dependencies set as `dependencies` and may result in a larger bundle. Because of this, we recommend switching from `rive-react` to `@rive-app/react-canvas` or the WebGL variant if you need to use a WebGL backing context.

**Before:**

```
npm i rive-react
```

**After:**

```
npm i @rive-app/react-canvas
```

{% hint style="info" %}
There are no changes regarding the way you import from the React runtime between `rive-react` and `@rive-app/react-canvas`
{% endhint %}

## Prop Spreading

The React runtime provides a `RiveComponent` whether you use the default export or the `useRive` hook provided. This component should be passed into the JSX to render the canvas out. The DOM encompasses a wrapping div around the canvas that helps to handle the styling and sizing of the canvas. Most props spread on the `RiveComponent` would be spread onto that wrapping `<div>` element.\
\
Starting in v2.x.x, certain props will be spread onto the wrapping `<div>` that concern styling (i.e `className` and `style`), but any other props will now be spread onto the `<canvas>` element, so that you can set `role`, `aria-*` attributes, etc.\
\
**Before:**

```jsx
<RiveComponent className="foo" aria-label="Label" />
```

Would render the (simplified) DOM such as:

```html
<div className="foo" aria-label="Label">
  <canvas></canvas>
</div>
```

**After:**

```
<RiveComponent className="foo" aria-label="Label" />
```

Now yields the following (simplified) DOM:

```html
<div className="foo">
  <canvas aria-label="Label"></canvas>
</div>
```
