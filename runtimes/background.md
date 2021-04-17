---
description: Our approach to the new Rive runtimes
---

# Background

The new Rive adopts a fresh approach to both the animation files and rendering engines. Our new binary format has been redesigned from the ground up, taking advantage of concepts such as LEB128 to reduce file sizes. Meanwhile, we've opted to support multiple rendering systems to ensure runtimes are best tailored to their respective platforms. For instance, simple web animations can be rendered using Context 2D, whilst more complex ones with the need for shaders can use CanvasKit.

Our C++ animation engine provides a low level, compact, and highly optimized foundation for platform-specific runtimes to build upon. This approach maintains a consistent codebase that spans across all devices and platforms.

### Optimized for size

We recognize the importance of keeping file sizes to a minimum. Currently, our web runtime is less than 50kb \(gzipped\), and our example animation files are typically around 10kb \(gzipped\). The runtime size will increase as more features are added, but this should not be significant and we’re still experimenting with how to compile and optimize Wasm/JS.

### Designed for maximum control

Our runtimes are designed to provide the greatest level of control over your animations. For instance, on the web, you will need to create a canvas, a context, and control the primary animation loop. This is by design, as it gives web developers complete control over how animations play and mix. It’s easy to wrap this in a higher-level API; for example, our [beta website](https://beta.rive.app/) wraps this in a React component.

