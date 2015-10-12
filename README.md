# jquery.event.ue #

## Summary ##

Use this jQuery plugin to handle both desktop and touch input
with unified events. It is currently being used in commercial SPAs 
and is featured in the book
[Single page web applications - JavaScript end-to-end][1]

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

## Browser Support ##

This was originally written in 2012 and has changed occassionaly since.
Works with iOS5+ (Stock browser) and Android 3.2+ (Chrome) and of course
all modern desktop browsers (IE9+ and later version of
Chrome, Safari, and Firefox.

IE9 may require edge settings:

    <html>
    <head>
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      ....

## Examples ##

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

## Motions ##

The following motions are supported:

- tap
- long-press
- drag start, drag move, drag end
- long-press drag start, move, and end
- zoom start, zoom move, zoom end

Notice the event object is a little different,
so take care, especially with event delegation.  The example
HTML should help point the way.


### utap ###
A click or a tap event results in a `utap` event.
- *Desktop* uses a *short-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *tap* motion.

A click or a tap must be reside within a tolerance radius; 
otherwise it starts a drag event.  And it must be short in
duration - otherwise the motion becomes a `uheld` event.

You may configure the drag radius and the long-press
durations.

### uheld ###
A long-press or long-hold event results in a `uheld` event.
- *Desktop* uses a *long-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *long-touch* motion.
 
A `uheld` event is distinguished from a `utap` event by the duration
of the press.  If the press short, the motion is a `utap`
event; if the duration exceeds the configured threshold, a 
`uheld` event is fired *immediately*.

### drag ###
Dragging events.
- *Desktop* uses a *LMB down - swipe - LMB up* motion
- *Touch* uses a *swiping* motion.

There are three events:
- `udragstart` - fires at the start of a drag (LMB down or finger press where motion has moved out of the drag radius)
- `udragmove`  - fires each time the mouse or finger moves
- `udragend`   - fires at the end of a drag (LMB up or finger release)


### zoom ###
Zooming events.
- *Desktop* uses a *shift-drag* motion.
- *Touch* uses a *pinching* motion.

A zoom motion, like drag, actually consists of three events:

- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Event Attributes ##

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

## Error handling ##

Like many other plugins, this code does not throw exceptions.
Instead, it does its work quietly.

## Namespacing ##

Notice that jQuery event namespacing is fully supported.
This works:

      $( '#msg' )
        .on( 'utap.mytap',  onTapMsg  )
        .on( 'uheld.mytap', onHeldMsg );

      $( '#msg' ).unbind( '.mytap' );

## Avoid complex 'SPA framework' libraries ##

jQuery used with this and a few other well-chosen tools forms
a fantastic basis for a lean, easy to use SPA architecture
as detailed in [Single page web applications, JavaScript end-to-end][1].
Here are the recommended tools:

| Capability   | Tool                | Notes                             |
| ------------ | ------------------- | ----------------------------------|
| Websockets   | [Socket.io][6]      | Prefer websockets over AJAX.      |
| AJAX         | jQuery native       | Use jQuery AJAX methods.          |
| Promises     | jQuery native       | Use jQuery promise methods.       |
| Model Events | [Global Events][2]  | jQuery plugin eliminates having   |
|              |                     | to manage multiple event types.   |
| Touch        | [Unified events][3] | Unify desktop and touch events.   |
| Routing      | [uriAnchor][4]      | jQuery plugin for robust routing. |
|              |                     | Includes support for dependent    |
|              |                     | and independent query arguments.  |
| Data Model   | [taffyDB][5]        | A powerful and flexible SQL-like  |
|              |                     | client data management tool.      |
| SVG          | [D3][7]             | Great for easy graphs and charts  |
|              | [SVG][8]            | Low-level jQuery plugin           |
| Templates    | [Dust][9]           | Uses a powerful template DSL that |
|              |                     | minimizes chances to intemingle   |
|              |                     | business and display logic.       |

This suite of tools has all the capabilities of a bleeding-edge 
SPA "framework" library within the reliable and mature jQuery ecosystem.
It can provide an application that is significantly more flexible and
testable since display logic can easily be decoupled from business logic.
Finally, it leverages jQuery's maturity, performance, and excellent
tools instead of competing with them.

## Release Notes ##

### Copyright (c)###
2013 Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License ###
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version 0.3.1 ###
This is the first release registered with jQuery plugins.

### Version 0.3.2 ###
Confirmed compatible with jQuery 1.7.0 through 1.9.1.

### Versions 0.4.1-3 ###
Updated documentation and npm release.

### Versions 0.5.0 ###
Updated docs, jslint compatibility, removed cruft, updated test page
to include zoom support (shift + mb1 hold + mouse up/down ).

### Testing ###
I have tested with Android 3.2+ (Chrome only),
iOS5+ Safari, Chrome 15+, Firefox, and IE.

The browser in Windows 8 RT has issues.

## See also ##

The Hammer touch library, jQuery mobile.

## TODO ##

- Support a wider range of motions

## Contribute! ##

If you want to help out, like all jQuery plugins this is hosted at
GitHub.  Any improvements or suggestions are welcome!
You can reach me at mike[dot]mikowski[at]gmail[dotcom].

## END ##

[1]:http://manning.com/mikowski
[2]:https://github.com/mmikowski/jquery.event.gevent
[3]:https://github.com/mmikowski/jquery.event.ue
[4]:https://github.com/mmikowski/urianchor
[5]:https://github.com/typicaljoe/taffydb
[6]:http://socket.io
[7]:https://github.com/mbostock/d3
[8]:http://keith-wood.name/svg.html
[9]:http://linkedin.github.io/dustjs
