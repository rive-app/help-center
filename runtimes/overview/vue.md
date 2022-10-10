---
description: Using Rive in Vue apps
---

# Vue

## Overview

Currently, there is no official Rive "Vue"-specific runtime available, however, you can utilize the Web runtime packages in your Vue applications. Check out [this example](https://github.com/Coded-Clouds/Rive\_Vue\_ExampleApp) from the community.

{% hint style="info" %}
Additionally, if you're interested in creating a Vue runtime with Rive, see the source for [rive-react](https://github.com/rive-app/rive-react), which just wraps the Web (JS) runtime into convenient hooks and components that make development in React apps easier. We encourage community-driven runtimes and would be happy to look at adding to our docs here!
{% endhint %}

## Getting Started

The following snippet demonstrates how to create a simple Rive component in [Vue](https://vuejs.org) 2.

```markup
<template>
  <div>
    <canvas ref="canvas" width="400" height="400"></canvas>
  </div>
</template>

<script>
import { Rive, Layout } from '@rive-app/canvas';

export default {
  name: 'Rive',
  props: {
    src: String,
    fit: String,
    alignment: String
  },
  mounted() {
    new Rive({
        canvas: this.$refs.canvas,
        src: this.$props.src,
        layout: new Layout({
            fit: this.$props.fit,
            alignment: this.$props.alignment,
        }),
        autoplay: true
    })
  }
}
</script>
```

If you're using Vue 3 with Typescript, the usage is slightly different with respect to their newer composition API:\


```typescript
<template>
  <div>
    <canvas ref="canvas" width="400" height="400"></canvas>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Rive } from '@rive-app/canvas';

export default defineComponent({
  name: 'Rive',
  props: {
    src: String
  },
  mounted() {
    new Rive({
      canvas: this.$refs.canvas,
      src: this.$props.src,
      autoplay: true
    })
  }
})
</script>
```
