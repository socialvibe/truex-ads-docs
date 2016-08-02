TrueX Ad Builder Layout API for JavaScript
==================================================

This page provides documentation for the TrueX Ad Builder's layout API.  The layout API allows a developer to manipulate UI elements and transition between steps that are laid out in the Ad Builder.

The `_layout` Variable
-----------
The `_layout` javascript variable is how you access elements and steps in your ad, and how you make calls to the layout API.  Each section below describes functionality provided by the `_layout` variable.


UI Elements
------------
UI elements are added to each step in our Ad Builder.  There are currently 7 element types:
```
Image
Button
Text
Input
IFrame
Video
YouTube
```

You can access each UI element in your ad by referencing its name in the `_layout` variable.  A jQuery reference to the UI element is returned.
```js
_layout.button_10.css('left', '100px');
_layout.video_8.get(0).play();
_layout.video_8.hide();
```

You may also access UI elements using jQuery selectors.
```js
$('#button_10').css('left', '100px');
$('#video_8').get(0).pause();
$('#video_8').fadeIn();
```


Video
------------
The layout API provides helper functions for directly working with video.

```js
_layout.startVideo(/* video name */);
_layout.stopVideo(/* video name */);
_layout.restartVideo(/* video name */);
_layout.updateVideoURL(/* video name */, /* video url */, /* preview image url */);

```


Step Transitioning
------------
The following functions are used to transition between steps in your layout.  The layout API will add/remove UI elements accordingly as well as perform the necessary step tracking.

```js
_layout.nextStep();  // transitions UI to the next step in the layout.
_layout.gotoStep(/* step name */); // transitions to specific step. Takes a step name as argument (i.e. 'step 2')
```

Tweening / Animation
------------
The layout API provides tweening functions for performaing simple animations.

```js
_layout.tweenTo(/* element name */, /* seconds */, /* values */, /* onComplete */);
_layout.tweenFrom(/* element name */, /* seconds */, /* values */, /* onComplete */);
```
- `element name` - the name of the UI element you want to animate (i.e. button_10).
- `seconds` - the number of seconds to animation should take.
- `values` - a hash of properties on the UI element to tween.
- `onComplete` - an optional handler function, called when the animation is completed.  Useful for animation sequences.

```js
_layout.tweenTo('image_2', 1, {'opacity':0});
_layout.tweenTo('button_5', 1, {'left':100, 'top':100});
```

Note: you may also use [jQuery.animate](http://api.jquery.com/animate/).

Event listening
------------
The layout API provides an event handler for listening to events on UI elements.

```js
_layout.on(/* event name */, /* element name */, /* event handler function */);  // attaches an event handler
_layout.off(/* event name */, /* element name */, /* event handler function */);  // removes an event handler
```
- `event name` - the event name to listen for.  see below for list of possible events.
- `element name` - the name of the UI element you want to animate (i.e. button_10).
- `event handler function` - a callback function that is invoked when the event occurs.

##### UI Events
```
click
mouseDown
mouseUp
mouseMove
rollOut
rollOver
mouseWheel
videoStarted
videoFirstQuartile
videoMidpoint
videoThirdQuartile
videoCompleted
```

##### Timeout Event
`timeout` is a special helper event handler for setting a javascript `setTimeout()`.  It takes milliseconds as the second arg.
```js
_layout.on('timeout' 1000, callback);
```


Misc
------------
Lastly, the layout API provides a way to pre-load other javascript and css files.  These files will be loaded during the preload phase, before your ad beings.  This is useful for loading external javascript libraries such as the Greensock tween engine.

```js
_layout.require(/* url */, /* type */);
```
- `url` - the URL of the file you wish to preload.
- `type` - the type of file (i.e. 'js', 'css', or 'img')


