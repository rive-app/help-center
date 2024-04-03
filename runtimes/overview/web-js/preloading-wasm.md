# Preloading WASM

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/preloading-wasm/docZkAViuNLJ).
{% endhint %}

## Background

When rending a `new Rive()` instance from the `@rive-app/*` packages, or the `<RiveComponent />` from the `@rive-app/react-*` packages, your browser is making a network request to `https://unpkg.com/@rive-app/canvas@x.x.x/rive.wasm` which retrieves a [Web Assembly](https://developer.mozilla.org/en-US/docs/WebAssembly) (WASM) file that contains Rive-specific APIs to build the render loop. [unkpg](https://unpkg.com/) is a global CDN that quickly allows for loading in NPM packages, which in this case includes a WASM file. This allows for a smaller bundle size when pulling in the Rive JS-based runtimes, while only loading in WASM when Rive instances are created.\
\
While `unpkg` should provide WASM quickly and can cache across sites that load from this CDN, you may want to preload and host the WASM file yourself for any of the following reasons:

* Better reliability of the WASM powering the Rive animations
* Quicker load times for your animations
* Control over when resources get loaded into your web apps

## Steps

Depending on the JS-based runtime you're using (i.e. JS, React, etc.), follow the sections below to self-host the WASM that gets loaded in from the base JS runtime.

### JS

If you're using the base `@rive-app/canvas` or `@rive-app/webgl` or any of the plain JS-variant Rive runtimes, import the WASM resource into your app, as well as the `RuntimeLoader` API:

```javascript
import riveWASMResource from '@rive-app/canvas/rive.wasm';
import { Rive, RuntimeLoader } from '@rive-app/canvas';

RuntimeLoader.setWasmUrl(riveWASMResource);
...
const riveInstance = new Rive({
  src: 'foo.riv',
  ...
});
```

The `RuntimeLoader` is a singleton that manages loading in the WASM file behind the scenes when loading in the `Rive` instance. By calling the `setWasmUrl` API, you can load in WASM for the Rive runtime with the data URI from the direct import of the WASM file. Run this API on any page that has a Rive animation to load.

{% hint style="info" %}
You may need to add configuration into your build tool to import WASM files and inline it as a data URI. See the [original blog post](https://dev.to/alex\_barashkov/optimization-techniques-for-rive-animations-in-react-apps-1a8p) that inspired us to add these to docs for some guidance here
{% endhint %}

### React

If you're using `@rive-app/react-canvas` or `@rive-app/react-webgl`, import the WASM resource into your app, as well as the `RuntimeLoader` API:

```tsx
import riveWASMResource from '@rive-app/canvas/rive.wasm';
import { useRive, RuntimeLoader } from '@rive-app/react-canvas';

RuntimeLoader.setWasmUrl(riveWASMResource);

const MyComponent = () => {
  const { rive, RiveComponent } = useRive({
    src: 'foo.riv',
    ...
  });
};
```

The `RuntimeLoader` is a singleton that manages loading in the WASM file behind the scenes when loading in the `Rive` instance. By calling the `setWasmUrl` API, you can load in WASM for the Rive runtime with the data URI from the direct import of the WASM file. Run this API on any page that has a Rive animation to load.

{% hint style="info" %}
You may need to add configuration into your build tool to import WASM files. See the [original blog post](https://dev.to/alex\_barashkov/optimization-techniques-for-rive-animations-in-react-apps-1a8p) that inspired us to add these to docs for some guidance here
{% endhint %}

You can additionally preload the WASM module if you set up your build tools to load Web Assembly for your application in relevant pages where you create Rive instances to instance your Rive animations quickly.

For example, with Next.js, you may attach the following to your main page layout:

```tsx
import { Html, Head } from "next/document";
import riveWASMResource from '@rive-app/canvas/rive.wasm';

export default function Document() {
  return (
    <Html>
      <Head>
        <link rel="preload" href={riveWASMResource} as="fetch" crossOrigin="anonymous" />
      </Head>
    </Html>
  );
}
```

You may need to install `@rive-app/canvas` at the version tied down with the correlated React version on `@rive-app/react-canvas` or you may notice issues at runtime. For example, `@rive-app/react-canvas@3.0.33` ties down the JS dependency at `@rive-app/canvas@1.0.95`; therefore you may want to install that specific version of the JS runtime to ensure compatibility.

## Special Thanks

Special thanks to Alex Barashkov for their [original blog post](https://dev.to/alex\_barashkov/optimization-techniques-for-rive-animations-in-react-apps-1a8p) that inspired us to add this tip.
