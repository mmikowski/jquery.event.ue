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
and should not be draggable, with the one exception being **held+helddrag**.

Compatible with jQuery 1.7.0+.

## Browser Support
This was originally written in 2012 and has been updated regularly in 2015 and
2016.  Works with iOS5+ (Stock browser) and Android 3.2+ (Chrome) and of course
all modern desktop browsers - IE9+ and later version of Chrome, Safari, and Firefox.

IE9 requires edge settings:

```html
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  ....
```

````js
## Examples
    // bind to a mouse click or tap event
    $( '#utap' ) .on( 'utap.utap',   onTap   );

    // bind to a long-press event
    $( '#uheld' ).on( 'uheld.uheld', onHeld  );

    // bind to zoom events
    $( '#uzoom' )
       .bind( 'uzoomstart', onZoomstart )
       .bind( 'uzoommove',  onZoommove  )
       .bind( 'uzoomend',   onZoomend   )
       ;

    // bind to drag events
    $( '#udrag' )
      .bind( 'udragstart.udrag', onDragstart )
      .bind( 'udragmove.udrag',  onDragmove  )
      .bind( 'udragend.udrag',   onDragend   )
      ;

    // bind to hold-drag events
    $('#uhelddrag')
      .bind('uheldstart.uheldd', onDragstart )
      .bind('uheldmove.uheldd',  onDragmove  )
      .bind('uheldend.uheldd',   onDragend   )
      ;
```

## Motions
The following motions are supported:

- tap
- long-press
- drag start, drag move, drag end
- long-press drag start, move, and end
- zoom start, zoom move, zoom end

Notice the event object is a little different, so take care, 
especially with event delegation - target is currently different.
The example HTML should help point the way.

### Taps
A click or a tap event results in a `utap` event.
- *Desktop* uses a *short-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *tap* motion.

A click or a tap must be reside within a tolerance radius;
otherwise it starts a drag event.  And it must be short in
duration - otherwise the motion becomes a `uheld` event.

You may configure the drag radius and the long-press
durations.

### Long-press
A long-press or long-hold event results in a `uheld` event.
- *Desktop* uses a *long-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *long-touch* motion.

A `uheld` event is distinguished from a `utap` event by the duration
of the press.  If the press short, the motion is a `utap`
event; if the duration exceeds the configured threshold, a
`uheld` event is fired *immediately*.

### Drag
Dragging events.
- *Desktop* uses a *LMB down - swipe - LMB up* motion
- *Touch* uses a *swiping* motion.

There are three events:
- `udragstart` - fires at the start of a drag (LMB down or finger press where motion has moved out of the drag radius)
- `udragmove`  - fires each time the mouse or finger moves
- `udragend`   - fires at the end of a drag (LMB up or finger release)


### Zoom
Zooming events.
- *Desktop* uses a *shift+LMB down - swipe - LMB up* motion.
- *Touch* uses a *pinching* motion.

A zoom motion, like drag, actually consists of three events:

- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Event Attributes
These are the event object attributes:

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
Like many other plugins, this code does not throw exceptions.
Instead, it does its work quietly.

## Namespacing
Notice that jQuery event namespacing is fully supported.
This works:

```js
$( '#msg' )
  .on( 'utap.mytap',  onTapMsg  )
  .on( 'uheld.mytap', onHeldMsg );

$( '#msg' ).unbind( '.mytap' );
```

## Release Notes
### Copyright (c)
2013 Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version  1.2.0
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

