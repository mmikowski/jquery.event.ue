# jquery.event.ue

## Summary
Use this **unified event** plugin to applications that easily and seamlessly
support mobile (touch) **and** desktop (mouse) environments. 

The plugin recognizes mouse, keyboard, and touch motions that have 
the same **intent** and publishes **unified events** for them. For example,
a long-press **intent** is expressed with a mouse by pressing-and-holding
on the left mouse button for over half a second without substantially moving
the mouse.  The same **intent** on a touch device is expressed by placing a
finger on the screen for over half a second without substantially moving the
finger.  This pugin recognizes both **motions** as having the same **intent**
and publishes a single `uheld` event for either.

Here are the full list of **intents** supported by this **unified event**
plugin:

- `utap`  - Mouse-click (mouse) / Finger tap (touch)
- `uheld` - Long-mouse-press / Finger-press-and-hold
- `udrag` - Mouse-click-and-drag / Finger Swipe 
- `uhelddrag` - Long-mouse-press-and-drag / Finger-press-and-hold-and-drag
- `zoom`    - Mouse-click-and-drag-Y + shift / Finger-pinch

This plugin is used in multiple commercial SPAs and is featured in the
best-selling book [Single Page Web Applications - JavaScript end-to-end][1],
also [available on Amazon][2].

Please see the `ue-test.html` file for a demonstration of the different intents.
Expected behavior is expressed in the BR corner of the tiles. If you see any
unexpected behavior while testing motions on your device please file a bug report
so we can fix it!

## Browser Support
This plugin works with jQuery 1.7.0+, and has been tested on most jQuery 
versions through 3.0.0.  It works with the latest versions of popular browsers: 
Chrome 15+, Firefox 23+, Safari 5+, and IE 9+. IE9 requires edge settings:

```html
  <head><meta http-equiv="X-UA-Compatible" content="IE=edge" />
  ....
```

## Examples
```js
    // bind to a mouse click or tap event
    $( '#utap' ) .on( 'utap',  onTap   );

    // bind to a long-press event
    $( '#uheld' ).on( 'uheld', onHeld  );

    // bind to zoom events
    $( '#uzoom' )
       .bind( 'uzoomstart', onZoomstart )
       .bind( 'uzoommove',  onZoommove  )
       .bind( 'uzoomend',   onZoomend   )
       ;

    // bind to drag events
    $( '#udrag' )
      .bind( 'udragstart', onDragstart )
      .bind( 'udragmove',  onDragmove  )
      .bind( 'udragend',   onDragend   )
      ;

    // bind to hold-drag events
    $('#uhelddrag')
      .bind('uheldstart', onDragstart )
      .bind('uheldmove',  onDragmove  )
      .bind('uheldend',   onDragend   )
      ;
```

## Intents
See the `ue-test.html` file for example use of the 
event object.

### The click intent
A click **intent** results in a `utap` event.
- The *Desktop* motion is *Left Mouse Button [LMB] down-up*
  where the mouse-down time is less than the tap tolerance and
  the location movement is less than the drag tolerance.
- The *Touch* motion is *touchstart-move-end* where
  the touch time is less than the tap tolerance and the location
  movement is less than the drag tolerance.

You may tweak the tap, drag, and long-press tolerances
in the `defaultOptMap` at the top of the lib. The API will have
a method to change these values in the future.

### The long-press intent
A long-press **intent** results in a `uheld` event.
- The *Desktop* motion is *Left Mouse Button [LMB] down-up*
  where the mouse-down time exceeds the long-press tolerance and
  the location movement is less than the drag tolerance.
- The *Touch* motion is *touchstart-move-end* where 
  the touch time exceeds the long-press tolerance and
  the location movement is less than the drag tolerance.

### The drag intent
A drag **intent** results in a single `udragstart` event, one or more
`udragmove` events, and a final `udragend` event.
- The *Desktop* motion is *LMB down - mouse-move - LMB up*
  where the mouse-down time exceeds the tap tolerance and 
  the location movement is greater than the drag tolerance.
- The *Touch* is *touchstart-move-end*, where 
  the touch time exceeds drag tolerance,

The event order is as follows:
- `udragstart` - fires at the start of a drag
- `udragmove`  - fires each time the pointer moves
- `udragend`   - fires at the end of a drag 

### The held-drag intent
The held-drag **intent** results in a single `uheldstart` event, one or more
`uheldmove` events, and a final `uheldend` event.
- The *Desktop* motion is *LMB down+hold - mouse-move - LMB up*
  where the mouse-down time exceeds the held tolerance and the 
  mouse has stayed within the drag tolerance, after which
  the location movement is greater than the drag tolerance.
- The *Touch* is *touchstart+hold-move-end*, where 
  the touch time exceeds the held tolerance and the touch as
  stayed within the drag tolerance, after which the touch
  location is greater than the the drag tolerance.

The event order is as follows:
- `uheldstart` - fires at the start of a held-drag
- `uheldmove`  - fires each time the pointer moves
- `uheldend`   - fires at the end of a held-drag

### The zoom intent
A zoom **intent** results in a single `uzoomstart` event, one or more
`uzoommove` events, and a final `uzoomend` event.

- The *Desktop* motion is *shift+LMB down - move-mouse - LMB up* where
  the mouse-down time exceeds the tap tolerance and
  the location movement is greater than the drag tolerance.
- The *Touch* motion is *two-point-touch - pinch - two-point-release*.

The event order is as follows:
- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Event Attributes
The event object for all **ue** events include these attributes in addition 
to those already provided by jQuery:

```js
var attr_list = [
  event.px_start_x,    // X-position at start of motion
  event.px_start_y,    // Y-ibid
  event.px_current_x,  // X-position, current
  event.px_current_y,  // Y-ibid
  event.px_end_x,      // X-position for x at end of motion
  event.px_end_y,      // Y-ibid
  event.px_delta_zoom, // Zoom delta since last motion event
  event.px_delta_x,    // X-ibid
  event.px_delta_y,    // Y-ibid
  event.px_tdelta_x,   // X-delta since beginning of motion
  event.px_tdelta_y,   // Y-ibid
  event.orig_target,   // Element under cursor at start of motion
  event.elem_bound,    // Element on which event was bound
  event.elem_target,   // Element under the cursor for *this* event
  event.timeStamp,     // Timestamp of this event
  event.ms_elapsed,    // Elapsed time since start of motion
  event.ms_timestart,  // Start time of motion
  event.ms_timestop,   // top time of motion
];
```

## Error handling
This code ignores bad input, and does not throw exceptions.  As JS implementations are (finally) seeing improved exception handling, expect this to change in 2.x.

## Namespacing
jQuery event namespacing is fully supported, as shown in this example:

```js
$( '#msg' )
  .on( 'utap.mytap',  onTapMsg  )
  .on( 'uheld.mytap', onHeldMsg );

$( '#msg' ).unbind( '.mytap' );
```
## Use libraries, not frameworks
This is a library that strives to be best-in-class.
If you are considering using an SPA framework, please read [Do you
really want an SPA framework?][0] first.

## Release Notes
### Copyright (c) 2013-2016
Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version 1.3.2
Updated to ignore most input elements (input, textarea, select) by default.
See the `ignore_select` setting.

### Version 1.3.0
- Removed all references to `console` object
- Changed deprecated `bind` and `unbind` to `on` and `off`

### Version  1.2.0-5
- Changed default option key `ignore_class` to `ignore_select`
- The value is now "" instead of ":input"
- Updated README and published to npm (1.2.5)

### Version 1.1.9 
Updated `ue-test.html` to handle zooming correctly.

### Version  1.1.8
Stopped preventDefault() from firing on events not controlled by the plugin.

### Versions 1.1.0-7
**No code changes.**  Updated npm keywords. Fixed typos.
Bumped version number to represent maturity and stability.
Redirected library discussion to blog post.

### Versions 0.6.1
The default `px_radius` parameter was doubled from 5px to 10px.  This made `utap`
and `uheld` events work much better on smaller screens.  Confirmed compatible with
with jQuery 2.1.4, thus 1.7.0 - 2.1.4 appear safe.

### Versions 1.1.x
**No code changes.**  Updated npm keywords. Fixed typos.
Bumped version number to represent maturity and stability.
Redirected library discussion to blog post.

### Versions 0.6.0
Fixed held logic, added `px_tdelta_x` and `px_tdelta_y` to attributes.
Updated test page to include expected results.

### Versions 0.5.0-0.5.2
Updated docs, jslint compatibility, removed cruft, updated test page
to include zoom support (shift + mb1 hold + mouse up/down ).

### Versions 0.4.1-3
Updated documentation and npm release.

### Version 0.3.2
Confirmed compatible with jQuery 1.7.0 through 1.9.1.

### Version 0.3.1
First release registered with jQuery plugins.

### Testing
Tested with Android 3.2 Chrome, iOS5+ Safari, Chrome 15+, Firefox 23, and IE 9+.

## See also
The Hammer touch library, jQuery mobile.

## TODO
Support a wider range of **intents**

## Contribute!
If you want to help out, like all jQuery plugins this is hosted at
GitHub.  Any improvements or suggestions are welcome!
You can reach me at mike[dot]mikowski[at]gmail[dotcom].

## End
[0]:http://mmikowski.github.io/no-frameworks
[1]:https://www.manning.com/books/single-page-web-applications
[2]:http://www.amazon.com/dp/1617290750


