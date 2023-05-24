---
description: Migrating guide from < 3.x
---

# Migrating from 2.x.x to 3.x.x

Migrating to v3 of the iOS runtime should be fairly straightforward. See the sections below on concerns you may need to look out for to change to your app if you upgrade.\


## Enums Naming

Starting in v3, the Layout option enums have changed to match enum naming conventions from the other runtimes for consistency. See below for what you should change `Fit` and `Alignment` options to. There are also a few other enums for parameters that have changed slightly regarding loop modes and direction.\


| Fit        | Before          | After        |
| ---------- | --------------- | ------------ |
| Fill       | `.fitFill`      | `.fill`      |
| Contain    | `.fitContain`   | `.contain`   |
| Cover      | `.fitCover`     | `.cover`     |
| Fit Width  | `.fitFitWidth`  | `.fitWidth`  |
| Fit Height | `.fitFitHeight` | `.fitHeight` |
| Scale Down | `.fitScaleDown` | `.scaleDown` |
| None       | `.fitNone`      | `.noFit`     |

<table><thead><tr><th>Alignment</th><th width="268.3333333333333">Before</th><th>After</th></tr></thead><tbody><tr><td>Top Left</td><td><code>.alignmentTopLeft</code></td><td><code>.topLeft</code></td></tr><tr><td>Top Center</td><td><code>.alignmentTopCenter</code></td><td><code>.topCenter</code></td></tr><tr><td>Top Right</td><td><code>.alignmentTopRight</code></td><td><code>.topRight</code></td></tr><tr><td>Center Left</td><td><code>.alignmentCenterLeft</code></td><td><code>.centerLeft</code></td></tr><tr><td>Center</td><td><code>.alignmentCenter</code></td><td><code>.center</code></td></tr><tr><td>Center Right</td><td><code>.alignmentCenterRight</code></td><td><code>.centerRight</code></td></tr><tr><td>Bottom Left</td><td><code>.alignmentBottomLeft</code></td><td><code>.bottomLeft</code></td></tr><tr><td>Bottom Center</td><td><code>.alignmentBottomCenter</code></td><td><code>.bottomCenter</code></td></tr><tr><td>Bottom Right</td><td><code>.alignmentBottomRight</code></td><td><code>.bottomRight</code></td></tr></tbody></table>

| Loop Mode | Before         | After      |
| --------- | -------------- | ---------- |
| One Shot  | `loopOneShot`  | `oneShot`  |
| Loop      | `loopLoop`     | `loop`     |
| Ping Pong | `loopPingPong` | `pingPong` |
| Auto      | `loopAuto`     | `autoLoop` |

| Direction | Before               | After           |
| --------- | -------------------- | --------------- |
| Backwards | `directionBackwards` | `backwards`     |
| Forwards  | `directionForwards`  | `forwards`      |
| Auto      | `directionAuto`      | `autoDirection` |

## Default playing behavior

One changed default behavior in v3 is what plays in the Rive canvas. Before v3, if no state machine or specific animation was specified when setting up the `RiveViewModel`, the first animation made in the Rive file would play. &#x20;

With v3, if no state machine or specific animation is specified, the first state machine (if one is created) in the Rive file will play. So if you prefer to keep your existing default behavior of the first animation, simply set the `animationName` property on `RiveViewModel` when creating it.
