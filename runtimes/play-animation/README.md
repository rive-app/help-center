# Play an animation

It’s helpful to understand the difference between an animation and an artboard. An animation is a set of keyframes that can be applied to a set of objects in an artboard. The animation instance tracks the keyframes as they change over time.

{% tabs %}
{% tab title="Web" %}
### 1. Create a canvas and context

First, you'll need to create a canvas and content for the renderer. Add a canvas to the body of your page.

```text
<div>
  <canvas id="riveCanvas"></canvas>
</div>
```

Then reference that canvas in your Javascript.

```javascript
const canvas = document.getElementById('riveCanvas');
const ctx = canvas.getContext('2d');
const renderer = new rive.CanvasRenderer(ctx);
```

Advance the artboard to internally render a frame; it won't be drawn just yet.

```javascript
artboard.advance(0);
```

### 2. Set your alignment and fit

The renderer has different fit \(_fill_, _contain_, _cover_, _fitWidth_, _fitHeight_, _scaleDown_, and _none_\) and align \(_topLeft_, _topCenter_, _topRight_, _centerLeft_, _center_, _centerRight_, _bottomLeft_, _bottomCenter_, and _bottomRight_\) options available to it. Here we center it and proportionately fit it to the canvas.

```javascript
ctx.save();
renderer.align(rive.Fit.contain, rive.Alignment.center, {
  minX: 0,
  minY: 0,
  maxX: canvas.width,
  maxY: canvas.height
}, artboard.bounds);
```

Now we can draw the first frame of our animation on the canvas

```javascript
artboard.draw(renderer);
ctx.restore();
```

### 3. Setup your animation loop

The recommended method for creating a render loop in Javascript is to use `requestAnimationFrame`. You can find [more information on this](https://developer.mozilla.org/en-US/docs/Games/Anatomy) from MDN. Start by keeping track of the last time a frame was rendered.

```javascript
let lastTime = 0;
```

Then define your draw method.

```javascript
function draw(time) {
  if (!lastTime) {
    lastTime = time;
  }
  const elapsedTime = (time - lastTime) / 1000;
  lastTime = time;

  myAnimInstance.advance(elapsedTime); 
  myAnimInstance.apply(artboard, 1.0);
  artboard.advance(elapsedTime);

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.save();
  renderer.align(rive.Fit.contain, rive.Alignment.center, {
    minX: 0,
    minY: 0,
    maxX: canvas.width,
    maxY: canvas.height
  }, artboard.bounds);
  artboard.draw(renderer);
  ctx.restore();

  requestAnimationFrame(draw);
}
```
{% endtab %}

{% tab title="Flutter" %}

{% endtab %}

{% tab title="iOS" %}
Stay tuned for information on our upcoming iOS runtime!
{% endtab %}

{% tab title="Android" %}
Stay tuned for information on our upcoming Android runtime!
{% endtab %}
{% endtabs %}

You can now start the animation by calling `requestAnimationFrame(draw)` outside `draw(time)` .

```text
requestAnimationFrame(draw);
```

Your Rive animation should now play. If the animation you selected is a looping animation, it will repeatedly loop automatically.

