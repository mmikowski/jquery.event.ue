# jquery.event.ue
## Use libraries, not frameworks
This is a library that strives to be best-in-class.
If you are considering using an SPA framework, please read [Do you
really want an SPA framework?][0] first.

## Summary
Support both Touch and Desktop interfaces using the same event handlers, including tap (click), long-press, drag, long-press-drag, and pinch/mouse zoom. It is used in commercial SPAs and is featured in the best-selling book [Single Page Web Applications - JavaScript end-to-end][1] which is also [available directly from Manning][2].

Supported motions for both desktop and touch devices include:

- tap
- long-press
- drag
- long-press drag
- zoom

Please see the `ue-test.html` file for a demonstration of the different
motions.  Most test tiles below the second row are negative test cases
and should not be draggable.

Compatible with jQuery 1.7.0+.

## Browser Support
Works with all modern browsers: Chrome, Firefox, Safari, and IE9+.
IE9 requires edge settings:

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

## Motions
The following motions are supported:

- tap
- long-hold
- drag: start, move, end
- long-hold-drag: start, move, end
- zoom: start, move, end

See the `ue-test.html` file for example use of the 
event object.

### Tap
A click or a tap motion results in a `utap` event.
- *Desktop* uses a *short-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *tap* motion.

A click or a tap must be reside within a tolerance radius;
otherwise it starts a drag event. It must also be short in
duration; otherwise it becomes a long-hold event.

You may tweak the tap, drag, and long-hold tolerances
in the `defaultOptMap` at the top of the lib.  The API will have
a method to change these values in the future.

### Long-hold
A long-hold motion results in a `uheld` event.
- *Desktop* uses a *long-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *long-touch* motion.

Long-hold motion is distinguished from a tap by the duration
of the press.  A `uheld` event is triggered when a press duration
exceeds the long-hold tolerance.

### Drag
A drag motion results in a single `udragstart` event, one or more
`udragmove` events, and a final `udragend` event.

- *Desktop* uses a *LMB down - swipe - LMB up* motion
- *Touch* uses a *swiping* motion.

Event details:
- `udragstart` - fires at the start of a drag (LMB down or finger press where motion has moved out of the drag radius)
- `udragmove`  - fires each time the mouse or finger moves
- `udragend`   - fires at the end of a drag (LMB up or finger release)

### Zoom
A zoom motion results in a single 'uzoomstart` event, one or more
`uzoommove` events, and a final `uzoomend` event.

- *Desktop* uses a *shift+LMB down - swipe - LMB up* motion.
- *Touch* uses a *pinching* motion.

Event details:
- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Event Attributes
The event object includes these attributes in addition to those already
provided by jQuery:

- `event.px_start_x` : position for x at start of motion
- `event.px_start_y` : ibid y
- `event.px_current_x` : current x position
- `event.px_current_y` : ibid y
- `event.px_end_x` : position for x at end of motion
- `event.px_end_y` : ibid y
- `event.px_delta_zoom`: the delta in zoom since start of motion
- `event.px_delta_x` : the delta in x position since start of motion
- `event.px_delta_y` : ibid y
- `event.orig_target` : element under cursor at start of motion
- `event.elem_bound` : element on which event was bound
- `event.elem_target` : element under the cursor (used for event delegation)
- `event.timeStamp`
- `event.ms_elapsed` : elapsed time since start of motion
- `ms_timestart` : start time of motion
- `ms_timestop` : stop time of motion

## Error handling
This code does not throw exceptions.

## Namespacing
jQuery event namespacing is fully supported, as shown in this example:

```js
$( '#msg' )
  .on( 'utap.mytap',  onTapMsg  )
  .on( 'uheld.mytap', onHeldMsg );

$( '#msg' ).unbind( '.mytap' );
```

## Release Notes
### Copyright (c) 2013-2016
2013-2016 Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version  1.2.0-3
Changed default option key "ignore_class" to "ignore_select"
  and it now defaults to "" instead of ":input"

### Version 1.1.9 
Updated tests to handle zooming correctly.

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
Support a wider range of motions

## Contribute!
If you want to help out, like all jQuery plugins this is hosted at
GitHub.  Any improvements or suggestions are welcome!
You can reach me at mike[dot]mikowski[at]gmail[dotcom].

## End
[0]:http://mmikowski.github.io/no-frameworks
[1]:http://www.amazon.com/dp/1617290750
[2]:http://manning.com/mikowski

