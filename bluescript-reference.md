# BlueScript Documentation
_rev. 2020-06-08_

## Table of Contents
1. [Overview](#overview)
1. [Structure](#overall-structure)
   1. [Top Level Syntax](#top-level-payload-syntax)
   1. [Element Syntax](#basic-bluescript-element-syntax)
   1. [Examples](#examples)
1. [Element Reference](#bluescript-element-reference)
   1. [Step](#step)
   1. [Value Object](#value-object)
   1. [Rectangle](#rectangle)
   1. [Image](#image)
   1. [Video](#video)
   1. [Label](#label)
   1. [Button](#button)
   1. [Audio](#audio)
   1. [QRCode](#qrcode)
1. [Authoring Considerations](#authoring-considerations)
   1. [Memory Management](#memory-management)

## Overview

We've created BlueScript as the true[X] creative format on the Roku platform since the built-in Roku SceneGraph XML cannot be dynamically served down, compiled, and instantiated. BlueScript provides us with the dynamic creative language we need to drive interactive ad experiences on behalf of our advertisers on the Roku platform.

BlueScript creatives are mastered and served down by the RTB ad server as part of the `asset_args` payload of ads. The example payloads under the [`vastConfigs`](https://github.com/socialvibe/southback/tree/develop/channel_res/vastConfigs) folder in `southback` exemplify this.

BlueScript carries forward the "step" paradigm introduced in TVML with the top level structure being a list of "steps" (or _cards_) one can navigate between. Then entry-point (first step) for true[X] ads is always `main_card`.

Each card contains a flat list of BlueScript elements. There is implied hierarchy in the elements being instantiated and added to the scene in order they are declared. Thus, earlier-declared elements will be drawn _underneath_ later-declared elements, if they occupy the same drawing region.

BlueScript elements support event handlers, enabling dynamic behavior. These can be used to trigger a variety of supported actions, such as controlling a video or playing sound effects. This is expressed as lists of behavior actions keyed on events. See below and `southback` repo for examples of this.

## Overall Structure

### Top Level Payload Syntax
> _**Formatting note**: strings inside of carets (`"< ... >"`) are meta-descriptions, not absolute values_

**Schema:**
```json
{
   "steps": [
      {
         "name": "main_card",
         "__comment__": "optional comment about this step",
         "elements": [
            "<(LIST OF BlueScript ELEMENTS (JSON OBJECTS))>"
         ],
         "behaviors": {
            "<ELEMENT ID>" : {
               "<EVENT>" : [
                  "<(LIST OF BEHAVIOR ACTIONS TRIGGERED BY EVENT)>"
               ]
            }
         }
      },
      {
         "name": "second_card",
         "elements": [
            "<(LIST OF BlueScript ELEMENTS (JSON OBJECTS))>"
         ]
      }
      ...]
}
```

### Basic BlueScript Element Syntax

```json
{
   "type" : "<TYPE of the BlueScript ELEMENT, [Image](#image), [Video](#video), [Label](#label), [Button](#button), [Audio](#audio), [Rectangle](#rectangle)>",
   "name" : "<unique STRING ID of the BlueScript ELEMENT>",
   "__comment__": "optional comment about this element",
   "<ADDITIONAL ELEMENT-SPECIFIC PROPERTIES>": "e.g. 'width' AND 'height'>"
}
```

### Examples

[See example payloads in the `southback` repo, `vastConfig` folder.](https://github.com/socialvibe/southback/tree/develop/channel_res/vastConfigs)

## BlueScript Element Reference

This section documents the base properties and behaviors inherited by all other BlueScript elements.

### Step

A **step** (or **card**) defines a single, self-contained screen. No two steps can be presented simultaneously, but a step can be cached so that its state is preserved.

#### Step Properties

Property | Default | Description
--- | --- | ---
name | N/A (Required) | ID(String) for the step and used to identify it for behaviors.
elements | N/A (Required) | An array that contains elements of this step. Z-indexed by the order, the first element is on the top, the last element is at the bottom.
behaviors | {} | An object, which define the behavior of elements, by their IDs.

#### Elements Properties

Elements - the BlueScript objects defined in each step - has the following properties:

Property | Default | Description
--- | --- | ---
type | N/A (Required) | Type for the BlueScript element. See below for what is supported.
name | N/A (Required) | ID for the element and used to identify it for behaviors.

**Note:** More properties are available depending on the TYPE of BlueScript element, detailed per element below.

#### Behaviors Properties

Behaviors - the **step** property that contains all behaviors for each element of the step - is an object with keys that match element IDs:
* **\<STRING ID\>** - ID of the BlueScript element that the child behaviors are associated with
  and has an array value that defines the behaviors of the object for each event (see below).

Property | Default | Description
--- | --- | ---
Element ID | {} | An object that defines the behavior on events. Each base behavior contains an array of actions.

<details>
<summary>For example:</summary>

```
{
  "behaviors": {
    "fooLabel": {
      <BEHAVIORS>
    }
  }
}
```
</details>

#### Base Behavior Properties

For each event, an array of **actions** can be defined. BlueScript elements can define any number of behavior actions for any number of events.

Property | Default | Description
--- | --- | ---
behavior | [ ] | Defines the list of behavior actions to be triggered on specific events. List of event names to triggered behavior action lists.

<details>
<summary>For example:</summary>

```
{
  "behaviors": {
    "fooLabel": {
      "appear": [ <ACTIONS> ],
      "disappear": [ <ACTIONS> ]
    }
  }
}
```
</details>

#### Base Behavior Events

The following are the top-level events that behaviors can be triggered from:

Behavior Event | Description
--- | ---
appear | Triggered when the element's parent card is displayed. Note that appear does not tie to visibility.
disappear | Triggered when the element's parent card is dismissed, such as on a card transition or when ending the ad flow.

#### Base Behavior Actions

This section details the **Behavior Actions** that are available to any BlueScript element. Actions are invoked when Events occur.

##### (1) - `allDoneButtonPushed`

Triggers the "Watch Your Show" button, exiting the ad flow for a completed ad.

###### Parameters

N/A

##### (2) - `showStep`

Navigates to a different BlueScript step, pushing the new step onto the navigational stack. A viewer is able to get back to the previous step using the BACK key or equivalent.

###### Parameters

Parameter | Description
--- | ---
cardName | Name of the step to transition to, as defined in the top level list of BlueScript steps.

##### (3) - `replaceStep`

Navigates to a different BlueScript step, replacing the current step on the navigational stack.

###### Parameters

Parameter | Description
--- | ---
cardName | Name of the step to transition to, as defined in the top level list of BlueScript steps.

##### (4) - `resetFocus`

Re-initializes the focus handler and ensures a proper control is focused.

###### Parameters

N/A

##### (5) - `setAttribute`

Assigns a new value to given property for the native component underlying the BlueScript element.

###### Parameters

Parameter | Description
--- | ---
name | Name of the BlueScript element that will have one of its underlying properties changed.
key | Name of the underlying property to update.
value | New value for the underlying property.

###### Notes

On Roku, `setAttribute` manipulates the SceneGraph component underlying the BlueScript element. In effect, it allows access to underlying implementation for the element. This could have adverse effects.

##### (6) - `flagActivityForAttention`

Flags the engagement as having been interacted with, fulfilling the interaction requirement of true[ATTENTION].

###### Parameters

N/A

##### (7) - `playSoundEffect`

Triggers a sound effect for playback.

###### Parameters

Parameter | Description
--- | ---
uri | Uri of the sound file to trigger playback for.

###### Notes

On Roku, `playSoundEffect` is a wrapper on top of the [`<SoundEffect/>`](https://sdkdocs.roku.com/display/sdkdoc/SoundEffect) SceneGraph component. It is notably limited to WAV files as a format for the sound effects.


##### (8) - `debugLog`

Prints log to BrightScript terminal. (Attach ` | grep "debugLog: "` at the end of `telnet` comment to filter for `debugLog`)

###### Example
```json
{ "host" : "debugLog", "value" : "This is a String." },     // Prints: This is a String.
{ "host" : "debugLog", "value" : { "key": "keyOfValue" }  },        // Prints: what ever is in keyOfValue.
{ "host" : "debugLog", "value" : { "operation": "+", "values": ["Plus ", "operation."] }  },        // Prints: Plus operation.
```

###### Parameters

Parameter | Description
--- | ---
value | The (string) value to print out. This can be a Value Object, see Value Object session for useage.

###### Notes

We would always try to cast any types into string.

##### (9) - `if/else`

The `if/else` Behavior Action is a conditional statement, which perform different actions depending on whether the `expression` boolean condition evaluates to true or false.

###### Example
```json
 { "host" : "if",
   "expression" : true,
   "then" : [{  "host" : "debugLog",
      "value" : "1) if true, print this" }] },
{ "host" : "if",
"expression" : false,
"then" : [{  "host" : "debugLog",
"value" : "1) if false, print this (should not print this)" }] },

{ "host" : "if",
"expression" : false,
"then" : [{  "host" : "debugLog",
"value" : "2) if true, print this (should not print this)" }],
"else" : [{  "host" : "debugLog",
"value" : "2) else, print this" }] },

{ "host" : "if",
"expression" : { "key": "didAchieveTrueAttention" } ,
"then" : [{  "host" : "debugLog",
"value" : "3) user DID achieve True[Attention]" }],
"else" : [{  "host" : "debugLog",
"value" : "3) user HAS NOT achieve True[Attention]" }] },

{ "host" : "if",
"expression" :  {"operation": "==", "values": [{ "key": "myObject.txLogLevel" }, 3] },
"then" : [{  "host" : "debugLog",
"value" : "4) txLogLevel is 3" }],
"else" : [{  "host" : "debugLog",
"value" : "4) txLogLevel is not 3" }] }
```

###### Parameters

Parameter | Description
--- | ---
expression | Expression is a Value Object, which determines the flow of the Behavior Actions
then | An array of Behavior Actions that should be executed if expression is true
else | (optional) An array of Behavior Actions that should be executed if expression is NOT true

###### Notes

There is no `else if` as of now.

##### (10) - `for`

Behavior Action `for` is a control flow statement for specifying iteration, which allows Behavior Actions to be executed repeatedly. Requires a control variable with `from` and `to` values, and a `do` Behavior Actions Array.

###### Example
```json
 { "host" : "for",
   "from": 0,
   "to": 3,
   "do" : [
      { "host" : "debugLog",
         "value" : "test" }
   ] },

{ "host" : "for",
"value": { "key": "i" },
"from": 0,
"to": { "key": "ta" },
"do" : [
{ "host" : "debugLog",
"value" : { "key": "i" } }
] },
```

###### Parameters

Parameter | Description
--- | ---
value | (optional) Value Object variable that the counter should save to. Default `{ "key": "forI" }`
from | An interger value that the loop counter would start from (could be Value Object)
to | An interger value that the loop counter would count to (inclusive) (could be Value Object)
do | An array of Behavior Actions that should be executed

###### Notes

There's no way to break out of a loop early just as of writing of this doc (_aka `break` keyword in C-based languages_)

##### (11) - `assign`

Behavior Action `assign` is an assignment statement that sets and/or re-sets the value stored in the storage denoted by a variable name (named by key); in other words, it copies a value into the variable.

###### Example
```json
 { "host" : "assign",
   "key" : "i",
   "value" : 0 },
{ "host" : "assign",
"key" : "i",
"value" : { "operation": "+", "values": [{ "key": "i" }, 1] } },
{ "host" : "assign",
"key" : "object.array.0",
"value" : "Hello World" },
{ "host" : "assign",
"key" : { "operation": "+", "values": ["object.array.", { "key": "i" } ] ,
"value" : { "key": "i" }},
}
```

###### Parameters

Parameter | Description
--- | ---
key | Name of variable. It can be a dot seperated list to indicate variable inside objects, or array. (could be Value Object)
value | Value of the variable. (could be Value Object)

###### Notes

On roku, all values hidden in a sandbox. Also, `host` would be the object that contains data from the host app. These includes

Parameter | Description
--- | ---
`host.ad` | Json object of the selected ad (Roku only, not supported on HTML5 yet)
`host.tagRotationRandom` | A fixed, random number from 0-1 per instance for (video) rotation. Ad author is supposed to read this number and change their ad accordingly.
`host.adParameters` | Contains all the adParameters and can be accessed by key.  EG.  host.adParameters.some_key
`host.rotationVideos` | An object with numerous subproperties related to the set of rotation videos
`host.rotationVideos.<0-N>` to get a specific video with that index (key: video_1 corresponds to `.0`)
`host.rotationVideos.length` to get the number of rotational videos
`host.rotationVideos.currentIndex` to get the index (base 0) of the current random video
`host.rotationVideos.currentUrl` to get the url of the current random video

There are also predefined keys that provide additional information dynamically
`userHasMetTIme` | The minimum time for ad interaction has been met
`userHasInteracted` | User has interacted with the ad
`userHasCredit` | User has gained credit for the ad
`timerTickValue` | Countdown remaining (Choice/Skip Card)

##### (12) - `setTimeout`

Behavior Action `setTimeout` allows execution of a set of Behavior Actions at specified time intervals. (One time, or repeat)

###### Example
```json
 { "host" : "setTimeout",
   "duration" : 3,
   "do": [ { "host" : "debugLog",
      "value" : "timeout fired after 3 seconds" } ] }
```

###### Parameters

Parameter | Description
--- | ---
repeat | (optional, default false) A boolean value that indicate if the timer will fire repeatedly.
duration | `duration` indicates the number of seconds before execution. Note: we will tolerate `timeout`, or `delay`
do | An array of Behavior Actions to be executed.

###### Notes

Creative is expected to stop their repeated timers.
Currently there are no menthod to stop an individual timer.

##### (13) - `stopAllTimers`

The `stopAllTimers` stops the execution of all the timers specified in setTimeout(), and clear all the saved timers.

##### (14) - `makeWebRequest`

Enables Behavior Actions to read the HTTP values sent by a client during a Web request.

###### Example
```json
 { "host" : "makeWebRequest",
   "url" : "https://api.weather.gov/gridpoints/SEW/124,67/forecast/hourly",
   "responseAsJson": true,
   "assignResponseTo": { "key": "weatherJson" },
   "onload": [
      { "host" : "debugLog",
         "value" : { "key": "weatherJson.properties.periods.0" } } ] }
```

###### Parameters

Parameter | Description
--- | ---
url | The server/file location.
assignResponseTo | take a json with a `key` entry, which points to destination that the returned string/json will save to.
responseAsJson | (optional, default: false) will try to parse the return as JSON before save.
onload | An array of Behavior Actions to be executed on call successed.
onerror | An array of Behavior Actions to be executed on call failed.

##### (15) - `bringToFront`

Changes child order of a TXElement so that it draws on top of all other views.

###### Parameters

Parameter | Description
--- | ---
name | Name of the node to bring to the front of the view

##### (16) - `focusElement`

Sets the focus to a specified TXElement. onFocusGained and onFocusLost are triggered when switching focus.

###### Parameters

Parameter | Description
--- | ---
name | Name of the node that will capture focus

##### (17) - `disableUserInput`

Prevent user input from being handled.

###### Parameters

N/A

##### (18) - `enableUserInput`

Allow user input to be handled.

###### Parameters

N/A

##### (19) - `disableUserNavigation`

Prevent user directional input from being handled.

###### Parameters

N/A

##### (20) - `enableUserNavigation`

Allow user directional input to be handled

###### Parameters

N/A

##### (21) - `popStep`

Removes the current BlueScript step from the stack, replacing it with the next step on the navigational stack.

##### Parameters

N/A

##### (22) - `animateElement`

Animates Bluescript element attributes. This action is only supported on rendered elements (not Audio), and only with certain fields (see below).

###### Parameters

Parameter | Default | Description
--- | --- | ---
attributes | N/A (required) | and object defining which attributes are to be animated; (x, y, width, height and opacity. position and size are supported legacy attributes)
duration | 0.35 | the length of time (in seconds) it will take to play the animation
easeFunction | `outCubic` | how the values will evolve throughout the animation duration
optional | false | whether or not the animation can be ignored for low-end devices under stress

###### Notes

Elements attributes are animated to the specified value **from their current value**!

More than one attribute can be animated with one call to `animateElement`.

```json
 { "host": "animateElement",
   "name": "Video_Player",
   "attributes": {
      "x": 110,
      "y": 217,
      "width": 1150,
      "height": 647
   },
   "duration": 0.5
}
```

##### (23) - `trackCustomEvent`

Tracks an event back to the true[X] server. This can be used to report back custom events and activity done by a viewer in the unit, such that it can be used for client-facing reports later on.

###### Example

```json
 { "host" : "trackCustomEvent",
   "name" : "button_clicked",
   "value": "button_1"
 }
```

```json
 { "host" : "trackCustomEvent",
   "category" : "click",
   "name" : "video_click_to_play",
   "value": "product_1_video"
 }
```

###### Parameters

Parameter | Default | Description
--- | --- | ---
category | `fep_roku_layout` | The tracking taxonomy category to use for the tracking call. Typically omitted so default value is used.
name | N/A (required) | The name of the tracking event.
value | | A value to use for the event, if appropriate.

### Value Object

The variable Behavior Actions take as input. There are 3 types:
1 - Simple Values
2 - Stored Values
3 - Operations

##### (24) - `setBounds`

Instantly sets the bounds / position of an element.  An alternative to animateElement when duration = 0

###### Parameters

Parameter | Default | Description
--- | --- | ---
target |  | the name of the target node
x |  | new x position
y |  | new y position
width |  | new width of element
height |  | new height of element

###### Notes

All new values are optional by excluding them in the action.  For example, if you only want to change x,y and leave width,height alone.

##### (1) - Simple Values

The basic variables like String, Boolean, or Number

###### Example
```json
 { "host" : "debugLog",
   "value" : "this is a string" },
{ "host" : "debugLog",
"value" : 1234.56 },
{ "host" : "debugLog",
"value" : true },
```

##### (2) - Stored Values

The local variables stored in the Behavior Action sandbox.

###### Example
```json
{ "host" : "debugLog", "value" : { "key": "weatherJson" } },
{ "host" : "debugLog",
  "value" : { "key": "weatherJson.properties.periods.0" } },
```

###### Parameters

Parameter | Description
--- | ---
key | Name of variable. It can be a dot seperated list to indicate variable inside objects, or array. (could be another Value Object)

##### (3) - Operations

Operations are used to compare values, perform arithmetic operations, logical operations, and perform functions that return value (random, for example), given an array of `values` as input.

###### Arithmetic Operations Example
```json
 { "host" : "debugLog",
   "value" : {"operation": "+", "values": [123, 123] } },
{ "host" : "debugLog",
"value" : {"operation": "+", "values": [1, 2, 3] } },
{ "host" : "debugLog",
"value" : {"operation": "+", "values": [1,  {"operation": "*", "values": [2, 3] } ] } },
```

###### String Operations Example
```json
 { "host" : "debugLog",
   "value" : {"operation": "+", "values": ["string", 123] } },
{ "host" : "debugLog",
"value" : "input: replace(\"This is a badass.\", \"badass\", \"awesome\")" },
```

###### Comparison Operations Example
```json
 { "host" : "debugLog",
   "value" : {"operation": "&&", "values": [true, false] } },
{ "host" : "debugLog",
"value" : {"operation": "||", "values": [true, false] } },
{ "host" : "debugLog",
"value" : {"operation": "!", "values": [true] } },
```

###### Functions Operations Example
```json
 { "host" : "debugLog",
   "value" : {"operation": "random", "values": [10] } },
```

###### Parameters

Parameter | Description
--- | ---
operation | Name of operation.
values | An array of Simple Values, or Value Object as input. Like Polish notation, operator comes before the values; however, Operations will only take 1 operation at a time.

###### Operation
Operator | Description | Number of input | Input type | Example | Means
-------  | ----------- | --------------- | ---------- | ------- | -----
\+ | Addition | >=2 | Number | `{"operation": "+", "values": [123, 123] }` | 123 + 123
\+ | Concatenate String | >=2 | String | `{"operation": "+", "values": ["hello", "_world"] }` | "hello" + "\_world"
\- | Subtraction | >=2 | Number | `{"operation": "-", "values": [123, 1, 23] }` | 123 - 1 - 23
\* | Multiplication | >=2 | Number | `{"operation": "*", "values": [123, 2] }` | 123 \* 2
/ | Division | >=2 | Number | `{"operation": "/", "values": [123, 2] }` | 123 / 2
% | Modulus (division remainder) | 2 | Number | `{"operation": "%", "values": [123, 2] }` | 123 % 2
== | Equal to | 2 | String, Number, or Boolean | `{"operation": "==", "values": [123, 2] }` | 123 == 2
!= | Not equal | 2 | String, Number, or Boolean | `{"operation": "!=", "values": [123, 2] }` | 123 != 2
\> | Greater than | 2 | Number | `{"operation": ">", "values": [123, 2] }` | 123 > 2
< | Less than | 2 | Number | `{"operation": "<", "values": [123, 2] }` | 123 < 2
\>= | Greater than or equal to | 2 | Number | `{"operation": ">=", "values": [123, 2] }` | 123 >= 2
<= | Less than or equal to | 2 | Number | `{"operation": "<=", "values": [123, 2] }` | 123 <= 2
&& | And | 2 | Boolean | `{"operation": "&&", "values": [true, false] }` | true && false --> (false)
\|\| | Or | 2 | Boolean | `{"operation": "\|\|", "values": [true, false] }` | true \|\| false --> (true)
! | Not | 1 | Boolean | `{"operation": "!", "values": [true] }` | !true --> (false)
replace | String replace | 3 | String | `{"operation": "replace", "values": ["Original String to Replace", "Replace", "Modify"] }` | "Original String to Replace".replace("Replace", "Modify")
random | Random number | 1 | Number | `{"operation": "random", "values": [10] }` | random(10)
length | Length | 1 | Natural | `{"operation": "length", "values": ["abcde"] }` | "abcde".length()
max | Maximum | >=2 | Number | `{"operation": "max", "values": [1, 2] }` | max(1, 2)
min | Minimum | >=2 | Number | `{"operation": "min", "values": [1, 2] }` | min(1, 2)
floor | Floor | 1 | Number | `{"operation": "floor", "values": [1.5] }` | Math.floor(1.5)
ceil | Ceiling | 1 | Number | `{"operation": "ceil", "values": [1.5] }` | Math.ceil(1.5)
round | Round | 1 | Number | `{"operation": "round", "values": [1.5] }` | Math.round(1.5)
toFixed | Shows specified # of digits after the decimal point | 2 | Number | `{"operation": "toFixed", "values": [3.1415, 3]}` returns `"3.142"` | (3.1415).toFixed(3)
toTrimFixed | Like {toFixed}, but also strips trailing zeroes | 2 | Number | `{"operation": "toTrimFixed", "values": [2.000001, 4]}` returns `"2"` | Number((2.000001).toFixed(4)).toString()
zeroFill | Fill number with leading 0s | 2 | Number | `{"operation": "zeroFill", "values": [123, 5]}`  returns `"00123"` | "00" + (123).toString()
formatMinutesSeconds | Format seconds as `MM:SS`, or `H:MM:SS` if over an hour | 1 | Number | `{"operation": "formatMinutesSeconds", "values": [3599]}` returns `"59:59"` | -
formatHoursMinutesSeconds | Format seconds as `H:MM:SS` | 1 | Number | `{"operation": "formatMinutesSeconds", "values": [3599]}` returns `"0:59:59"` | -

---

### Rectangle

Renders a single-colored rectangle at the specified screen coordinates.

TXRectangle is a simple sub-class of Brightscript's [Rectangle](https://developer.roku.com/docs/references/scenegraph/renderable-nodes/rectangle.md) component that allows us to assign behaviors and play animations. All of the original Rectangle functionality is supported.

#### Example

```json
{
   "type": "Rectangle",
   "name": "sampleOverlay",
   "x": "0",
   "y": "0",
   "width": "1920",
   "height": "1080",
   "color": "0x00000080"
}
```

#### Rectangle Properties

Property | Default | Description
--- | --- | ---
x | 0 | Screen-space X position, where x=0 is the left side of the (assumed 1080p) screen
y | 0 | Screen-space Y position, where y=0 is the top of the (assumed 1080p) screen
width | 0 | Specifies the width of the Rectangle on the screen
height | 0 | Specifies the height of the Rectangle on the screen
color | 0xFFFFFFFF | Specifies the RGBA color drawn to the rectangle region
blendingEnabled | true | Specifies if the rectangle should be alpha blended with the nodes that are behind it

#### Rectangle Behavior Events

N/A

#### Rectangle Behavior Actions

N/A

---

### Image

Renders an image at the specified screen coordinates.

#### Example

```json
{
   "type": "Image",
   "name": "hiltonBackgroundImage",
   "x": "0",
   "y": "0",
   "width": "1920",
   "height": "1080",
   "image_url": "https://media.truex.com/image_assets/2018-05-22/504a601c-60ef-449c-8281-6d973310d053.jpg"
}
```

#### Image Properties

Property | Default | Description
--- | --- | ---
x | 0 | X coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
y | 0 | Y coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
width | 0 | Specifies the width of the image in local coordinates. If set to 0, the width of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that width.
height | 0 | Specifies the height of the image in local coordinates. If set to 0, the height of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that height.
image_url | N/A (Required) | Specifies the URI to the image file.
opacity | 1 | In the range [0, 1] where 0 is full transparent and 1 is fully opaque
forceHighResolution | false | Use the image's native / high resolution on memory constrained low end Roku devices. See below [Memory Management](#memory-management) note.

#### Image Behavior Events

N/A

#### Image Behavior Actions

N/A

---

### Video

Renders a video at the specified screen coordinates.

#### Example

```json
{
   "type"      : "Video",
   "name"      : "videoPlayer",
   "x"         : "125",
   "y"         : "175",
   "width"     : "1138",
   "height"    : "640",
   "video_url"       : "https://media.truex.com/video_assets/2018-05-09/e0baa5cc-641f-4773-9ade-156a53f9608c_large.mp4",
   "loop"      : false,
   "autoplay"  : false,
   "trackingName" : "hilton_worktrip_video"
}
```

#### Video Properties

Property | Default | Description
--- | --- | ---
x | 0 | X coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
y | 0 | Y coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
width | 0 | Specifies the width of the video in local coordinates. If set to 0, the video play window is set to the width of the entire display screen.
height | 0 | Specifies the height of the video in local coordinates. If set to 0, he video play window is set to the height of the entire display screen.
video_url | N/A (Required) | Specifies the URI to the video file. MP4 format is expected.
loop | true | If set to true, the video will be restarted from the beginning after the end is reached.
autoplay | true | If set to true, the video will be automatically started on card display.
replayable | false | If set to true, makes the video player focusable on playback completion, enabling replay of the video.
mute | false | Set to true to mute the audio of the video currently playing. Set to false to restore audio.
fullscreen | false | When toggled, the video will be animated to fullscreen, or back to window
zoomAnimationDuration | 0.35 | The time it takes to animated between window and fullscreen. The 0.35 is copie from tvOS TAR. (Note: must be > 0)
easeFunction | "inOutCubic" | This string defines the animation curve. For full list of easeFunction, see: https://sdkdocs.roku.com/display/sdkdoc/Animation#Animation-Fields
currentTime | N/A | This value is the floating point time of the current stream.  This has a margin of error of 500ms due to how position is handled.
trackingName | | When set, tracking calls utilize this name instead of the uri to identify videos. Greatly facilitates analysis. Note that when `useRotation` set to true, a `_n` suffix will be added to the trackingName, where `n` is the index of the video with "1 indexing".
preview_image_url | | When set, implements a static image to overlay on top of the video player on video playback completion.
focusable | false | Set to true in order to make the video focusable.
leftFocus | invalid | ID of element to focus when left directional button is pressed.
rightFocus | invalid | ID of element to focus when right directional button is pressed.
upFocus | invalid | ID of element to focus when up directional button is pressed.
downFocus | invalid | ID of element to focus when down directional button is pressed.
autoFocus | false | If set to true, the video (if focusable) will be focused by default.
useRotation | false | If set to true, will ignore the video url and try to apply the value of `video_n` from the ad parameters.

#### Notes

On Roku, only a single Media node may be actively playing or buffering media at a given time. This means that it is not possible to have multiple inline videos playing concurrently or a background / ambient video with inline window windows on top. This also precludes the use of background audio playing together with a video.

#### Video Behavior Events

Behavior Event | Description
--- | ---
videoStarted | Triggered when the active video has started playback.
videoCompleted | Triggered when the active video has completed playback.
videoFirstQuartile | Triggered when the active video has completed 25% of its playback.
videoSecondQuartile | Triggered when the active video has completed 50% of its playback.
videoThirdQuartile | Triggered when the active video has completed 75% of its playback.
videoLooped | Triggered when the active video has looped playback.
videoDidEnterFullscreen | Triggered when video animated into fullscreen
videoDidExitFullscreen | Triggered when video animated back to window

#### Video Behavior Actions

##### (1) - `playVideo`

Starts or resumes playback (for a paused video) for the target video.

###### Parameters

Parameter | Description
--- | ---
target | Name of the video to play.

##### (2) - `pauseVideo`

Pauses playback for the target video.

###### Parameters

Parameter | Description
--- | ---
target | Name of the video to pause.

##### (3) - `resetVideo`

Resets playback for the target video by setting the play head to the beginning.

###### Parameters

Parameter | Description
--- | ---
target | Name of the video to reset.

##### (4) - `stopVideo`

Stops playback for the target video.

###### Parameters

Parameter | Description
--- | ---
target | Name of the video to stop.

---

### Label

Renders a text label at the specified screen coordinates.

#### Example

```json
{
   "type"      : "Text",
   "name"      : "firstLabel",
   "x"         : "200",
   "y"         : "150",
   "width"     : "800",
   "height"    : "0",
   "text"      : "This is the TXAudio Tester"
}
```

#### Label Properties

Property | Default | Description
--- | --- | ---
x | 0 | X coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
y | 0 | Y coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
width | 0 | Specifies the width of the label in local coordinates. If the width field is greater than zero, the computed width is the value of the width field. If the width field is equal to zero, the computed width is the rendered width of the text.
height | 0 | Specifies the height of the label in local coordinates. If set to 0, the height of the label is computed based on the height of the number of lines of text that need to be rendered. That is, one line of text, unless wrapping is turned on.
text |  | Specifies the text string to render in the label.
color | 0xddddddff | Specifies the color of text rendered in the label.  Supports HTML code formats.  RGB formats such as rgb(r,g,b) rgba(r,g,b,a) rgba(r%,g%,b%,a%) with commas or whitespace.  And Hex formats such as  #RGB #RGBA #RRGGBB #RRGGBBAA.
backgroundColor | 0x00000000 | Specifies the background color for the label, covers the final calculated width and height of the element.  Supports HTML code formats (see color above)
fontName | (system default) | Specifies the font name for the text rendered in the label. That is either a [built-in](https://sdkdocs.roku.com/display/sdkdoc/Typography) system font, or a TTF/OTF font we ship with the library.
font_size | 24 | Size of the font, in points. Note that this is only supported for OTF/TTF files, as the default system fonts imply a given font size.
font_url |   | Fully qualified URL of an OTF/TTF font asset to be dynamically loaded.  Externally hosted unlike fontName and must end with extension .otf/.ttf.
alignment | center | Horizontal alignment, one of `left`, `center` or `right`.
vertical_alignment | top | Vertical alignment, one of `top`, `center` or `bottom`.
wrap | true | The wrap field is used to control how the text is broken into multiple lines. If false, a single line of text will be displayed. Else, the text is broken over multiple lines each of the calculated width, up to the specified height.

#### Label Behavior Events

N/A

#### Label Behavior Actions

N/A. Note: manipulating properties of the label such as `text` `color` and `backgroundColor` can be achieved using the `setAttribute` behavior action.


###### Notes

On Roku, the Label element is a thin wrapper on top of a `<Label/>` SceneGraph node. Refer to the [Roku documentation](https://sdkdocs.roku.com/display/sdkdoc/Label) for detailed description of layout and wrapping behavior.

---

### Button

Renders a focusable button at the specified screen coordinates.

#### Example

```json
{
   "type"     : "Button",
   "name"     : "pauseButton",
   "x"        : "1389",
   "y"        : "491",
   "width"    : "403",
   "height"   : "120",
   "image_url": "http://development.scratch.truex.com.s3.amazonaws.com/roku/simon/video_tester/pause_button_unsel.png",
   "behavior" : {
      "onselect": [
         { "host" : "flagActivityForAttention" },
         { "host" : "pauseActiveVideo" }
      ]
   }
}
```

#### Button Properties

Property | Default | Description
--- | --- | ---
x | 0 | X coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
y | 0 | Y coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
width | 0 | Specifies the width of the button in local coordinates. If set to 0, the width of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that width.
height | 0 | Specifies the height of the button in local coordinates. If set to 0, the height of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that width.
image_url | N/A (Required) | Specifies the URI to the image file used for the button.
hover_image_url |  | Specifies the URI to the image file used for the button's focused state.
checked_image_url |  | Specifies the URI to the image file used for the button's enabled or checked state.
hover_checked_image_url |  | Specifies the URI to the image file used for the button's focused state when it is used as a toggle style button.
checked | false | Whether this toggle style button is set to "on" by default.
hover_effect | true | If set to true, upon focus button grows by a factor of 25%, with no change in the image to the hover_image_url.
hover_scale | 1.1 | A float value that indicates the how big, in percentage, this button should glow on hover.
focusable | true | Set to false in order to make the button not focusable.
forceHighResolution | false | Use the button's native / high resolution on memory constrained low end Roku devices. See below [Memory Management](#memory-management) note.
leftFocus | invalid | ID of element to focus when left directional button is pressed.
rightFocus | invalid | ID of element to focus when right directional button is pressed.
upFocus | invalid | ID of element to focus when up directional button is pressed.
downFocus | invalid | ID of element to focus when down directional button is pressed.
autoFocus | false | If set to true, the button will be focused by default.

#### Button Behavior Events

Behavior Event | Description
--- | ---
onFocusGained | Triggered when the button receives focus.
onFocusLost | Triggered when the button loses focus.
onSelect | Triggered when the button is selected.

#### Button Behavior Actions

N/A. Note: manipulating properties of the button such as `checked` can be achieved using the `setAttribute` behavior action.

---

### Audio

Plays background audio.

#### Example

```json
{
   "type"      : "Audio",
   "name"      : "backgroundAudio",
   "audio_url"       : "http://development.scratch.truex.com.s3.amazonaws.com/roku/simon/audio_tester/audio_test.mp3",
   "autoplay"  : false,
   "loop"      : true
}
```

#### Audio Properties

Property | Default | Description
--- | --- | ---
audio_url | N/A (Required) | Specifies the URI to the audio file. MP3 format* is expected. Always use the Asset Uploader.
loop | true | If set to true, the audio will restart from the beginning after the end is reached.
autoplay | true | If set to true, the audio will be automatically started on card display.
global | false | If true, audio sticks/continues playing across card transitions.

#### Notes

On Roku, only a single Media node may be actively playing or buffering media at a given time. This means that it is not possible to have multiple inline videos playing concurrently or a background / ambient video with inline window windows on top. This also precludes the use of background audio playing together with a video. Any additional stream started after the first one will fail silently.

Note on MP3 format. On PlayStation, and some other platforms, MP3 is not supported. BlueScript engine will try to get the MP4 version (same location, same name) of the given audio instead. Asset Uploader in True Exchange is encoding that MP4 file automatically. Be sure to include a MP4 version of your audio file if you cannot use the Asset Uploader.

#### Audio Behavior Events

N/A

#### Audio Behavior Actions

##### (1) - `playActiveAudio`

Starts or resumes playback (for a paused file) for the active audio element.

###### Parameters

N/A

##### (2) - `pauseActiveAudio`

Pauses playback for the active audio element.

###### Parameters

N/A

##### (3) - `resetActiveAudio`

Resets playback for the active audio element by setting the play head to the beginning.

###### Parameters

N/A

##### (4) - `stopActiveAudio`

Stops playback for the active audio element.

###### Parameters

N/A

---

### QRCode

Renders a QR Code as image using the [qr-code-generator](https://github.com/socialvibe/ui_misc/tree/master/lambda_functions/qr-code-generator).

#### Example

```json
{
   "type"     : "QRCode",
   "name"     : "qr_code",
   "x"        : "824",
   "y"        : "312",
   "width"    : "264",
   "height"   : "264",
   "size"     : "250",
   "margin"   : "4",
   "url"      : "https://www.engagementshowcase.com/#sponsored-ad-break-ctv/ca1f7fa6d/",
   "color"    : "000000",
   "backgroundColor" : "ffffff",
   "minify"   : "true",
   "tagLabel" : "main_qr"
}
```

#### Button Properties

Property | Default | Description
--- | --- | ---
x | 0 | X coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
y | 0 | Y coordinate for the component, assumes origin at the top left, and a 1080p sized canvas.
width | 0 | Specifies the width of the button in local coordinates. If set to 0, the width of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that width.
height | 0 | Specifies the height of the button in local coordinates. If set to 0, the height of the bitmap from the image file is used. If set to a value greater than 0, the bitmap is scaled to that width.
url | | Specifies the information that is encoded in the QR Code image. This is also the fallback url for the tagLabel option. Using this field without setting the tagLabel will generate a QR Code contains only the information in this field, with no 1st party tracking. One should give the tagLabel a value (that does not have "Click" tag set) to get the trackings.
size | dynamic | Specifies the native size in pixels of the QR Code image being returned.
margin | 4 | Specifies the margin, in number of dots in QR Code matrix.
color | 0xddddddff | Specifies the color of dots rendered in the QR Code.
backgroundColor | 0x00000000 | Specifies the background color for the QR Code, covers the final calculated width and height of the element.
minify | false | Specifies if we would pass final url to `multidevice.truex.com/minify` for minimize.
tagLabel | (empty) | Specifies if this QR Code should take its url from the tag manager with the matching label. When set, the `url` field is ignored, and TAR will try to get the last matching label from the Tag Mangager, with Trigger "QR Code" (`qr_code`), and Type "Click"; all the other 3rd party pixels with Trigger "QR Code" (`qr_code`), and Type "Pixel"; as well as a 1st party pixel. We will use the `interact.live` redirect service to redirect, and fires all the necessary pixel. If there is no matching "Click" with this given the tagLabel, TAR will use the url field as the fallback redirect url.

---

## Authoring Considerations

This section documents additional considerations to take into account when authoring creatives for the Roku platform using BlueScript.

### Memory Management

Roku devices have limited memory available to load and display images. Compounding the problem is the fact that Roku stores images in uncompressed form, so that a loaded image takes up WIDTH x HEIGHT x 4 bytes, regardless of its size on disk. On lower end devices, this can be problematic, and rather than cause a critical error, exceeding this limit leads to flickering and other artifacts as the Roku system aggressively cycles images in and out of texture memory. This is all documented in [detail here](https://developer.roku.com/docs/developer-program/performance-guide/memory-management.md).

To mitigate this, we've added logic to make it so that images are scaled down to 720p, respecting aspect ratio, when loaded on to lower end devices. This mitigates memory use particularly in the case of full screen image backgrounds of resolutions 1080p and above. Note that we also cap out images to 1080p since that's the maximum resolution Roku devices can manage for static content, even when on the video side the capability for 4K video exists.

We've provided a way for the ad author to override this logic and force an image to use its native resolution, bypassing the 720p limit on lower end devices. This can be useful when maximum clarity is desired, particularly if there are just a limited number of images to account for overall. This can be done by setting the `forceHighResolution` property on the corresponding `TXImage` and `TXButton` to `true`.

### List of Low End Roku Device Models

The following Roku device models are treated as low end which impacts the behavior of certain capabilities and memory management as called out above.

- `"2700X", "2710X", "2720X", "3500X", "3600X", "3700X", "3710X", "3800X", "3810X", "3900X", "3910X", "3930X", "3931X", "4200X", "4210X", "4230X", "5000X", "8000X", "8103X"`

---

## Misc

### A/B Testing
Assuming there is knowledge of what the A/B experiment object looks like, all information can be accessed with the following notation: `testVariations.<key>`.  This is similar to accessing information in the `host` object.  For example, with the following variation returned from the ad:
```json
      "variations": {
"truex_first_experiment": "control",
"FAP-460": "oldTT",
"tvml_cc_test": "T0"
},
```
Within the Bluescript, the value of tvml_cc_test can be acquired with Bluescript such as `"value": { "key": "testVariations.tvml_cc_test" }`

### Usage of container_args
`container_args` are settings for Engagement, per ad. These are set in the trueExchange, under Settings > TAGS & PARAMETERS > Parameters. These settings are return under the ad respond, under `container_args.settings.KEY`

#### Roku and HTML5
Key | Default | Value
--- | --- | ---
footer_timer_label | "YOUR INTERACTIVE AD" | Override for footer (right) text, for state when user has not reached time requirnment, and not interacted.
footer_timer_interacted_label | "YOUR INTERACTIVE AD" | Override for footer (right) text, for state when user has not reached time requirnment, but already interacted.
footer_timer_timeout_label | "INTERACT WITH THIS AD" | Override for footer (right) text, for state when user has reached time requirnment, but not interacted.
video_n | | Rotational Video. See Video for more info.
* Roku 1.4.0 or above
* HTML5 (pending) or above

#### container_args for `ctv_footer_test`
Key | Default | Value
--- | --- | ---
footer_prompt_interact_label | "INTERACT" | Override for footer (left) text, for state when user has not reached time requirnment, and not interacted.
footer_prompt_timeout_label | "PRESS RIGHT TO INTERACT" | Override for footer (left) text, for state when user has reached time requirnment, but not interacted.
footer_prompt_can_return_label | "PRESS DOWN TO SELECT RETURN TO CONTENT" | Override for footer (left) text, for state when user has reached true attrentation, but not focusing on the Return To Content button.
footer_prompt_is_on_return_label | "PRESS SELECT TO EXIT AD" | Override for footer (left) text, for state when the Return To Content button has focus.
* Roku 1.4.0 or above
* HTML5 (pending) or above
* Override only applies when the `ctv_footer_test` AB Test is on.

#### Roku 1.3 or below
Key | Default | Value
--- | --- | ---
footer_custom_text | "YOUR INTERACTIVE AD" | Override for footer text, for state when user has not reached time requirnment, and not interacted.
footer_custom_time_text | "YOUR INTERACTIVE AD" | Override for footer text, for state when user has not reached time requirnment, but already interacted.
footer_custom_interact_text | "INTERACT WITH THIS AD" | Override for footer text, for state when user has reached time requirnment, but not interacted.
* These are supported in Roku 1.4 as legacy fallback

#### HTML5 1.2 or below
Key | Default | Value
--- | --- | ---
footer_custom_image | https://media.truex.com/image_assets/2020-10-03/d7a64c50-c0ad-481b-88ba-3e6cbfc92c76.png | Override for footer image, for state when user has not reached time requirnment, and not interacted. Image will be scaled to 21px of height (in 1080p canvas).
footer_custom_time_image | https://media.truex.com/image_assets/2020-10-03/d7a64c50-c0ad-481b-88ba-3e6cbfc92c76.png | Override for footer image, for state when user has not reached time requirnment, but already interacted. Image will be scaled to 21px of height (in 1080p canvas).
footer_custom_interact_image | https://media.truex.com/image_assets/2020-06-16/34ddff51-0057-49f2-ab48-4e7075cc1505.png | Override for footer image, for state when user has reached time requirnment, but not interacted. Image will be scaled to 21px of height (in 1080p canvas).
* These are NOT supported in HTML5 (pending) or above
