# jquery.event.ue #

## Summary ##

A plugin designed to unify mouse and touch input.
The following motions are supported for both desktop
and touch devices:

- tap
- long-press
- drag
- long-press drag
- zoom

Please see the ue-test.html file in this directory which illustrates
the unified inputs.

## Release Notes ##

### Copyright (c)###
2013 Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License ###
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version 0.3.1 ###
This is the first release registered with jQuery plugins.

### Testing ###
I have tested with Android 3.2+ (Chrome only),
iOS5+ Safari, Chrome 15+, Firefox, and IE.

The browser in Windows 8 RT has issues.

*This special event handler breaks in jQuery 1.8+.*
And yes, fixing that is on my TODO list.

## Examples ##

See the ue-test.html file to see plenty of examples.
Examine the HTML source.

## Overview ##

This is not the only unified event handler I have seen recently.
This was originally written in 2012 and changed occassionaly since.
Our target touch platforms were iOS5+ (Stock browser) and
Android 3.2+ (Chrome).

The following motions are supported:

- tap
- long-press
- drag start, drag move, drag end
- long-press drag start, move, and end
- zoom start, zoom move, zoom end

Notice the event object is a little different for these events,
so take care, especially with event delegation.  The example
HTML should help point the way.

## Supported events ##

### utap ###
A click or a tap event results in a `utap` event.
- *Desktop* uses a short-press click on the Left Mouse Button [LMB]
- *Touch* uses a tap on the screen

A click or a tap must be reside within a tolerance radius; 
otherwise it starts a drag event.  And it must be short in
duration - otherwise it becomes a long-press event.

You may configure the drag radius and the long-press
durations.

### uheld ###
A long-press or long-hold event results in a `uheld` event.
- *Desktop* uses a long-press Left Mouse Button [LMB]
- *Touch* uses a long-press on the screen
 
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
- `udragmove`  - fires each time the mouse or finger
- `udragend`   - fires at the end of a drag (LMB up or finger release)


### zoom ###
Zooming events.
- *Desktop* uses a shift-drag motion.
- *Touch* uses a pinching motion.

A zoom motion, like drag, actually consists of three events:

- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Error handling ##

Like many other plugins, this code does not throw exceptions.
Instead, it does its work quietly.

## Other notes ##

Notice that jQuery event namespacing is fully supported.
This works:

      $( '#msg' )
        .on( 'utap.mytap',  onTapMsg  )
        .on( 'uheld.mytap', onHeldMsg );

      $( '#msg' ).unbind( '.mytap' );

## See also ##

jQuery mobile.

## TODO ##

- Add to "See also" sections.

## Contribute! ##

If you want to help out, like all jQuery plugins this is hosted at
GitHub.  Any improvements or suggestions are welcome!
You can reach me at mike[dot]mikowski[at]gmail[dotcom].

Cheers, Mike


END
