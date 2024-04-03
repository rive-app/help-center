---
description: Community-driven Angular runtime for Rive
---

# Angular

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/angular/docGiKUC4AXY).
{% endhint %}

## Overview

This guide documents how to get started using the community-built Angular runtime library. The source is available in its [GitHub repository](https://github.com/dappsnation/ng-rive).\
\
This library contains modules to help integrate Rive into your Angular web application.

## Getting Started

Follow the steps below for a quick start on integrating Rive into your Angular app.

### 1. Install the dependency

```
npm install ng-rive
```

### 2. Import the module

```typescript
import { RiveModule } from 'ng-rive';

@NgModule({
  declarations: [AnimationComponent],
  imports: [
    CommonModule,
    RiveModule,
  ],
})
export class AnimationModule { }
```

### 3. Add the Rive file to assets

```
|-- assets
|   |--rive
|      |-- vehicles.riv
```

### 4. Render the Rive template

{% code title="animation-component.html" %}
```html
<canvas riv="vehicles" width="500" height="500">
  <riv-animation name="idle" play></riv-animation>
</canvas>
```
{% endcode %}

## Resources

Github: [https://github.com/dappsnation/ng-rive](https://github.com/dappsnation/ng-rive)\
API Docs: [https://github.com/dappsnation/ng-rive#api](https://github.com/dappsnation/ng-rive#api)
