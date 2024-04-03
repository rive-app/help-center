---
description: Subscribing to Rive Events at runtime
---

# Rive Events

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/rive-events/docbOnaeffgr).
{% endhint %}

With Rive events, you have the ability to subscribe to meaningful signals that get reported from animations, state machines, and Rive listeners, all created at design time from the Rive editor. These signals can be subscribed to at runtime and have a specific name, type, and various custom metadata that may accompany the event to help inform the context surrounding its meaning.&#x20;

For more on the Events feature in general, check out the [Events](../editor/events.md) page in the editor section of the docs.

For example, in a Rive graphic simulating a loader, there may be an event named `LoadComplete` fired when transitioning from a `complete` timeline animation state to an `idle` state. You can subscribe to Rive events with a callback that the runtime may invoke, and from there, your callback can handle extra functionality at just the right moment when the event fired.

Other practical use cases for events:

* Coordinating audio playback at specific moments in an animation
* Opening a URL when specific interactions have occurred
* Adding haptic feedback on meaningful touch interactions
* Implementing functionality on Buttons and other UI elements
* Send semantic information
* Communicate any information your runtime needs at the right moment&#x20;

## Subscribing to Events

When you subscribe to Rive events at runtime, you subscribe to **all** Rive events that may be emitted from an animation/state machine, and you can parse through each event by name or type to execute conditional logic.

Let's use a 5-star rater Rive example to set any text supplied with events and open a URL if one is given.

{% tabs %}
{% tab title="Web" %}
### Examples

* [Star rating example](https://codesandbox.io/s/rive-events-js-wsgcqd?file=/src/index.mjs)
* [Neostream example (Chrome only)](https://codesandbox.io/s/neostream-rive-events-2f6sny?file=/src/index.mjs)

### High-level API usage

#### Adding an Event Listener

Similar to the `addEventListener()` / `removeEventListener()` API for DOM elements, you'll use the Rive instance's `on()` / `off()` API to subscribe to Rive events. Simply supply the RiveEvent enum and a callback for the runtime to call at the appropriate moment any Rive event gets detected.

#### Example Usage

```typescript
import { Rive, EventType, RiveEventType } from '@rive-app/canvas'

const r = new Rive({
  src: "/static-assets/star-rating.riv"
  artboard: "my-artboard-name",
  autoplay: true,
  stateMachines: "State Machine 1",
  // automaticallyHandleEvents: true, // Automatically handle OpenUrl events
  onLoad: () => {
    r.resizeDrawingSurfaceToCanvas();
  },
});

function onRiveEventReceived(riveEvent) {
  const eventData = riveEvent.data;
  const eventProperties = eventData.properties;
  if (eventData.type === RiveEventType.General) {
    console.log("Event name", eventData.name);
    // Added relevant metadata from the event
    console.log("Rating", eventProperties.rating);
    console.log("Message", eventProperties.message);
  } else if (eventData.type === RiveEventType.OpenUrl) {
    console.log("Event name", eventData.name);
    window.open(eventData.url);
  }
}

// Add event listener and provide callback to handle Rive Event
r.on(EventType.RiveEvent, onRiveEventReceived);
// Can unsubscribe to Rive Events at any time via the off() API like below
// r.off(EventType.RiveEvent, onRiveEventReceived);
```

### Low-level API usage

When using the low-level APIs (i.e. `@rive-app/canvas-advanced`), you'll need to catch Rive events reported during the render loop yourself via your created state machine instance (see [docs](overview/web-js/low-level-api-usage.md) on low-level API usage). To achieve this, before advancing the state machine:

* Determine the number of Rive events reported since the last frame via the state machine's `reportedEventCount()` API
* Iterate over the events and grab a reference to an Event via the state machine's `reportedEventAt(idx)` API

```javascript
import RiveCanvas, {RiveEventType} from '@rive-app/canvas-advanced';

...
// render loop
function myCustomRenderLoop(timestamp) {
    ...
    const elapsedTimeSec = (timestamp - prevTimestamp) / 1000;
    if (stateMachine) {
      const numFiredEvents = stateMachine.reportedEventCount();
      for (let i = 0; i < numFiredEvents; i++) {
        const event = stateMachine.reportedEventAt(i);
        // Run any Event-based logic now
        if (event.type === RiveEventType.OpenUrl) {
          const a = document.createElement("a");
          a.setAttribute("href", event.url);
          a.setAttribute("target", event.target);
          a.click();
        }
      }
    }
    // Now advance
    stateMachine.advance(elapsedTimeSec);
    ...
    rive.requestAnimationFrame(myCustomRenderLoop);
}
rive.requestAnimationFrame(mycustomRenderLoop);
```
{% endtab %}

{% tab title="React" %}
### Examples

* [Star rating example](https://codesandbox.io/s/rive-events-react-2pk8qk?file=/src/App.js:399-470)

### Adding an Event Listener

Similar to the `addEventListener()` / `removeEventListener()` API for DOM elements, you'll use the Rive instance's `on()` / `off()` API to subscribe to Rive events from the `rive` object returned from the `useRive` hook.  Simply supply the RiveEvent enum and a callback for the runtime to call at the appropriate moment any Rive event gets detected.

{% hint style="info" %}
**Note:** You must use the `useRive()` hook to subscribe to Rive events
{% endhint %}

#### Example Usage

```tsx
import { useRive, EventType, RiveEventType } from '@rive-app/canvas';
import { useCallback, useEffect } from 'react';

const MyTextComponent = () => {
  const {rive, RiveComponent} = useRive({
    src: "/static-assets/star-rating.riv",
    artboard: "my-artboard-name",
    autoplay: true,
    // automaticallyHandleEvents: true, // Automatically handle OpenUrl events
    stateMachines: "State Machine 1",
  });
  
  const onRiveEventReceived = (riveEvent) => {
    const eventData = riveEvent.data;
    const eventProperties = eventData.properties;
    if (eventData.type === RiveEventType.General) {
      console.log("Event name", eventData.name);
      // Added relevant metadata from the event
      console.log("Rating", eventProperties.rating);
      console.log("Message", eventProperties.message);
    } else if (eventData.type === RiveEventType.OpenUrl) {
      console.log("Event name", eventData.name);
      // Handle OpenUrl event manually
      window.location.href = data.url;
    }
  };
  
  // Wait until the rive object is instantiated before adding the Rive
  // event listener
  useEffect(() => {
    if (rive) {
      rive.on(EventType.RiveEvent, onRiveEventReceived);
    }
  }, [rive]);
  
  return (
    <RiveComponent />
  );
};
```
{% endtab %}

{% tab title="React Native" %}
### Adding a Rive Event Listener

Similar to other callback functions you can provide on the `<Rive>` component, such as `onPlay` or `onStateChange`, you can now provide an `onRiveEventReceived` callback which will be invoked any time a Rive Event gets reported during the render loop.

The API signature is as follows:

```typescript
onRiveEventReceived?: (event: RiveGeneralEvent | RiveOpenUrlEvent) => void;
```

#### Example Usage

```tsx
import React, { useRef, useState } from 'react';
import {
  SafeAreaView,
  ScrollView,
  Linking,
  Text,
} from 'react-native';
import Rive, { Fit, RiveOpenUrlEvent, RiveRef } from 'rive-react-native';

export default function Events() {
  const riveRef = useRef<RiveRef>(null);
  const [eventMessage, setEventMessage] = useState('');

  return (
    <SafeAreaView>
      <ScrollView>
        <Rive
          ref={riveRef}
          autoplay={true}
          fit={Fit.Cover}
          resourceName={'rating'}
          stateMachineName="State Machine 1"
          onRiveEventReceived={(event) => {
            // These are properties added to the event at Design Time in the
            // Rive editor
            const eventProperties = event.properties;
            if (eventProperties?.message) {
              setEventMessage(eventProperties.message as string);
            }
            
            // If an event has an accompanying URL, open it
            if ('url' in event) {
              Linking.openURL((event as RiveOpenUrlEvent).url || '');
            }
          }}
        />
        <Text>{eventMessage}</Text>
      </ScrollView>
    </SafeAreaView>
  );
}
```
{% endtab %}

{% tab title="Flutter" %}
### Adding an Event Listener

After creating a `StateMachineController` instance, you'll use the `addEventListener` / `removeEventListener` API and provide a callback to subscribe to Rive events.

The API signature:

```dart
void addEventListener(OnEvent callback)
void removeEventListener(OnEvent callback)
typedef OnEvent = void Function(RiveEvent);
```

{% hint style="info" %}
You may add multiple event listener callbacks if you need to, but all event listeners need to be removed. All event listeners will be removed when the controller is disposed.
{% endhint %}

\
Rive's event object provided to the callback will vary depending on the type of event (i.e. `RiveGeneralEvent` vs `RiveOpenURLEvent`, and others in the future). These derive from `RiveEvent` which has:

* &#x20;`name` - Name of the event.
* `secondsDelay` - Time since the event was reported and the callback received the event.
* `properties` - Custom properties are extra data that can be supplied with an event at design time.&#x20;

The `RiveOpenURLEvent` has additional fields: `url` and `target` (enum of type `OpenUrlTarget`).

For example, a sample callback to handle logging a Rive event may look like the following:

```dart
void onRiveEvent(RiveEvent event) {
  print(event);
}

...

RiveAnimation.asset(
  'assets/rating.riv',
  onInit: (Artboard artboard) {
    // Get State Machine Controller for the state machine
    final controller = StateMachineController.fromArtboard(artboard, 'State Machine 1');
    controller.addEventListener(onRiveEvent);
    artboard.addController(controller!);
  },
)
```

#### Example Usage - Star Rating

This example demonstrates how to retrieve custom properties set on a Rive event and update a Flutter UI component.

{% hint style="info" %}
Note that a Rive event could be triggered **during** a Flutter frame update. Calling `setState` at this stage will result in an exception being thrown by Flutter. Instead you should schedule a `setState` to be called on the next frame using `WidgetsBinding.instance.addPostFrameCallback`
{% endhint %}

```dart
class EventStarRating extends StatefulWidget {
  const EventStarRating({super.key});

  @override
  State<EventStarRating> createState() => _EventStarRatingState();
}

class _EventStarRatingState extends State<EventStarRating> {
  late StateMachineController _controller;

  @override
  void initState() {
    super.initState();
  }

  String ratingValue = 'Rating: 0';

  void onInit(Artboard artboard) async {
    _controller =
        StateMachineController.fromArtboard(artboard, 'State Machine 1')!;
    artboard.addController(_controller);

    _controller.addEventListener(onRiveEvent);
  }

  void onRiveEvent(RiveEvent event) {
    // Access custom properties defined on the event
    var rating = event.properties['rating'] as double;
    // Schedule the setState for the next frame, as an event can be
    // triggered during a current frame update
    WidgetsBinding.instance.addPostFrameCallback((_) {
      setState(() {
        ratingValue = 'Rating: $rating';
      });
    });
  }

  @override
  void dispose() {
    _controller.removeEventListener(onRiveEvent);
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Event Star Rating'),
      ),
      body: Column(
        children: [
          Expanded(
            child: RiveAnimation.asset(
              'assets/rating_animation.riv',
              onInit: onInit,
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8.0),
            child: Text(
              ratingValue,
              style: const TextStyle(fontSize: 22, fontWeight: FontWeight.w600),
            ),
          )
        ],
      ),
    );
  }
}
```

#### Example Usage - Open a URL

A common action you may want to do is open a URL from a Rive Event. For that Rive provides a custom event type `RiveOpenURLEvent`.

```dart
class EventOpenUrlButton extends StatefulWidget {
  const EventOpenUrlButton({super.key});

  @override
  State<EventOpenUrlButton> createState() => _EventOpenUrlButtonState();
}

class _EventOpenUrlButtonState extends State<EventOpenUrlButton> {
  late StateMachineController _controller;

  @override
  void initState() {
    super.initState();
  }

  void onInit(Artboard artboard) async {
    _controller = StateMachineController.fromArtboard(artboard, 'button')!;
    artboard.addController(_controller);

    _controller.addEventListener(onRiveEvent);
  }

  void onRiveEvent(RiveEvent event) {
    if (event is RiveOpenURLEvent) {
      try {
        final Uri url = Uri.parse(event.url);
        launchUrl(url);
      } on Exception catch (e) {
        debugPrint(e.toString());
      }
    }
  }

  @override
  void dispose() {
    _controller.removeEventListener(onRiveEvent);
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Event Open URL'),
      ),
      body: Column(
        children: [
          Expanded(
            child: RiveAnimation.asset(
              'assets/url_event_button.riv',
              onInit: onInit,
            ),
          ),
          const Center(
            child: Padding(
              padding: EdgeInsets.all(8.0),
              child: Text('Open URL: https://rive.app'),
            ),
          ),
        ],
      ),
    );
  }
}
```

&#x20;
{% endtab %}

{% tab title="iOS/macOS" %}
### Subscribing to Events via State Machine Delegate

To subscribe to Rive events, implement the `onRiveEventReceived` protocol from `StateMachineDelegate`.&#x20;

`@objc optional func onRiveEventReceived(onRiveEvent riveEvent: RiveEvent)`

This implementation may be invoked when a Rive event is fired from the render loop and provides a generic `RiveEvent` data type, of which you can type check to cast to a specific event for further parsing, such as a `RiveGeneralEvent` or a `RiveOpenUrlEvent`.&#x20;

For example:

```swift
@objc func onRiveEventReceived(onRiveEvent riveEvent: RiveEvent) {
    debugPrint("Event Name: \(riveEvent.name())")
    debugPrint("Event Type: \(riveEvent.type())")
    if let openUrlEvent = riveEvent as? RiveOpenUrlEvent {
        // i.e., open the URL
    } else if let generalEvent = riveEvent as? RiveGeneralEvent {
        // i.e., print the string data provided in a Text widget
    }
}
```

{% hint style="warning" %}
**Note:** Events of type `RiveOpenUrlEvent` will not automatically open links in the user's preferred browser. You will need to add the logic to grab the `url` property of the `riveEvent` passed into the delegate and open the link.
{% endhint %}

#### Example Usage

```swift
import SwiftUI
import RiveRuntime

struct SwiftEvents: DismissableView {
    var dismiss: () -> Void = {}
    @StateObject private var rvm = RiveEventsVMExample()

    var body: some View {
        VStack {
            rvm.view()
            Text("Event Message")
                .font(.headline)
                .padding(.bottom, 10)
            Text(rvm.eventText)
                .padding()
                .background(rvm.eventText.isEmpty ? Color.clear : Color.black)
                .foregroundColor(.white)
                .cornerRadius(10)
        }
    }
}

class RiveEventsVMExample: RiveViewModel {
    @Published var eventText = ""

    init() {
        super.init(fileName: "rating_animation")
    }

    func view() -> some View {
        return super.view().frame(width: 400, height: 400, alignment: .center)
    }

    // Subscribe to Rive events and this delegate will be invoked
    @objc func onRiveEventReceived(onRiveEvent riveEvent: RiveEvent) {
        if let openUrlEvent = riveEvent as? RiveOpenUrlEvent {
            if let url = URL(string: openUrlEvent.url()) {
                #if os(iOS)
                UIApplication.shared.open(url)
                #else
                NSWorkspace.shared.open(url)
                #endif
            }
        } else if let generalEvent = riveEvent as? RiveGeneralEvent {
            let genEventProperties = generalEvent.properties();
            if let msg = genEventProperties["message"] {
                eventText = msg as! String
            }
        }

    }
}
```
{% endtab %}

{% tab title="Android" %}
### Adding an Event Listener

Use the `addEventListener` and `removeEventListener` on `RiveAnimationView` to subscribe/unsubscribe a`RiveFileController.RiveEventListener`.

This listener receives either an `OpenURLRiveEvent` or `GeneralRiveEvent` of type `RiveEvent`.

<pre class="language-kotlin"><code class="lang-kotlin"><strong>/// Access the RiveAnimationView
</strong><strong>private val yourRiveAnimationView: RiveAnimationView by lazy(LazyThreadSafetyMode.NONE) {
</strong>    findViewById(R.id.your_animation_view)
}

...

/// Create a RiveEventListener
val eventListener = object : RiveFileController.RiveEventListener {
    override fun notifyEvent(event: RiveEvent) {
        when (event) {
            is OpenURLRiveEvent -> {
                Log.i("RiveEvent", "Open URL Rive event: ${event.url}")
            }
            is GeneralRiveEvent -> {
                Log.i("RiveEvent", "General Rive event")
            }
        }
        Log.i("RiveEvent", "name: ${event.name}")
        Log.i("RiveEvent", "type: ${event.type}")
        Log.i("RiveEvent", "properties: ${event.properties}")
        // `data` contains all information in the event
        Log.i("RiveEvent", "data: ${event.data}");
    }
}

/// Attach the listener
yourRiveAnimationView.addEventListener(eventListener);

...

/// Remove when no longer needed
override fun onDestroy() {
    yourRiveAnimationView.removeEventListener(eventListener);
    super.onDestroy()
}
</code></pre>

{% hint style="warning" %}
The Rive Android Runtime is executed on a separate thread. Any UI updates that are triggered from a Rive event will need to be manually marked to run on the UI thread using `runOnUiThread`. See examples below.
{% endhint %}

### Opening a URL

{% hint style="warning" %}
Events of type `OpenUrlRiveEvent` will not automatically open links. The code needs to be added manually in your project.
{% endhint %}

The following is an example that demonstrates how to open a URL on Android when an `OpenUrlRiveEvent` is received:

```kotlin
val eventListener = object : RiveFileController.RiveEventListener {
    override fun notifyEvent(event: RiveEvent) {
        when (event) {
            is OpenURLRiveEvent -> {
                runOnUiThread {
                    try {
                        val uri = Uri.parse(event.url);
                        val browserIntent =
                            Intent(Intent.ACTION_VIEW, uri)
                        startActivity(browserIntent)
                    } catch (e: Exception) {
                        Log.i("RiveEvent", "Not a valid URL ${event.url}")
                    }
                }
            }
        }
    }
}
yourRiveAnimationView.addEventListener(eventListener);
```

You can also access `event.target` to get the target destination of the URL, as set in the editor.

### Example

The following demonstrates how to update UI in response to some Rive event (named "StarRating") that contains a custom number property (named "Rating"). Note the `runOnUiThread`:

```kotlin
val eventListener = object : RiveFileController.RiveEventListener {
    override fun notifyEvent(event: RiveEvent) {
        when (event) {
            is GeneralRiveEvent -> {
                runOnUiThread {
                    // This event contains a number value with the name "rating"
                    // to indicate the star rating selected
                    if (event.name == "StarRating" && event.properties.containsKey("rating")) {
                        starRatingTextView.text = "Star rating: ${event.properties["rating"]}"
                    }
                }
            }
        }
    }
}
```

It's possible to evaluate which event has come through by checking the `name` and type of event (`GeneralRiveEvent` vs `OpenURLRiveEvent`).

By calling `event.properties` you'll get a `HashMap` that contains any custom properties defined on the event.
{% endtab %}
{% endtabs %}

### Additional Resources

* [Rive events - workshop](https://www.youtube.com/watch?v=e2bshfKuu8U)

###
