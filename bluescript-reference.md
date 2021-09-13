# BlueScript Reference Guide
_rev. 2021-09-10

## Table of Contents
1. [Overview](#overview)
1. [Structure](#overall-structure)
   1. [Top Level Syntax](#top-level-syntax)
   1. [Step](#step)
      1. [Elements](#elements)
      1. [Behaviors](#behaviors)
      1. [Functions](#functions)
   1. [Examples](#examples)
1. [BlueScript Values and Expressions](#bluescript-values-and-expressions)
   1. [Literal Values](#literal-values)
   1. [Stored Values](#stored-values)
   1. [Expressions](#expressions)
      1. [Arithmetic Operations](#arithmetic-operations)
      1. [String Operations](#string-operations)
      1. [Comparison Operations](#comparison-operations)
      1. [Variable References](#variable-references)
      1. [Date Expressions](#date-expressions)
      1. [Misc Expressions](#misc-expressions)
      1. [Expression Composition](#expression-composition)
1. [BlueScript Elements](#bluescript-element-reference)
   1. [Rectangle](#rectangle)
   1. [Image](#image)
   1. [Video](#video)
   1. [Text](#text)
   1. [Button](#button)
   1. [Audio](#audio)
   1. [QRCode](#qrcode)
1. [BlueScript Behaviors](#bluescript-behavior-reference)
   1. [Events](#behavior-events)
   1. [Actions](#behavior-actions)
1. [Authoring Considerations](#authoring-considerations)
   1. [Memory Management](#memory-management)

## 1 Overview

We've created BlueScript as the true[X] creative format on the Roku platform since the built-in Roku SceneGraph XML cannot be dynamically served down, compiled, and instantiated. BlueScript provides us with the dynamic creative language we need to drive interactive ad experiences on behalf of our advertisers on the Roku platform.

BlueScript is also present for HTML5 now as well, which allows the same ads for Roku and HTML5 to be easily created by simply copying over the same Bluescript code of the Layout JSON section.

For Roku, BlueScript creatives are mastered and served down by the RTB ad server as part of the `asset_args` payload of ads. The example payloads under the [`vastConfigs`](https://github.com/socialvibe/southback/tree/develop/channel_res/vastConfigs) folder in `southback` exemplify this.

For HTML5, the RTB server also serves up the ads, but the window_url in the vast config indicates the engagement web experience for display in an iframe. The engagement SDK will pull in the ad payload including the layoutJSON bluescript code. E.g. here is a [sample vast config url](https://qa-get.truex.com/15c7f5269a09bd8c5007ba98263571dd80c458e5/vast/config?dimension_2=0&dimension_5=hilton&stream_position=preroll&stream_id=1234&network_user_id=5a9f137a-51b2-4e1a-9238-0d0ecfb26c16&env[]=html) that returns a vast config. Once running, with the user selecting Yes in the choice card, the ad payload is returned via an [engagement ad vars query](http://qa-serve.truex.com/engage.json?bid_info=0-FzpTVJVASph-4KyRcZJ8ihWgMxpwC6cWtjTmPSUywE6J7g4SgxagXQyuupEotjap4aJDqirZpzXoqRMtcphcUxxoQHboxj7otPH5wute5zecvVgCxnY51PFpWgEhscpneMVZS4QSWfRDXLnHP8x1z5UfiPMVaptcLSJaJAJ8YJL67sUcti6K1vvFJzC722ZbRWTmiqDUttgLAyXBtcKM24CBxMk7vVsL122ivFn43sgEAcbczkvkgCH7u1TiT&campaign_id=4349&creative_id=8245&currency_amount=1&env%5B%5D=html&impression_signature=cb80d8784964bb0cc6b4ddade7461a2cdff5aabf0f36a6010a45041d555c135d&impression_timestamp=1630694571.2783427&initials=0&internal_referring_source=08AMYXWJZUx3OA3UYWgfWA&ip=172.218.11.127&network_user_id=91a8d3ef-e93d-4b47-93c7-3261708514b7&placement_hash=15c7f5269a09bd8c5007ba98263571dd80c458e5&referrer=http%3A%2F%2Flocalhost%3A8080%2F&session_id=kDtp3T7RTp6tcOCbOhwacA&stream_id=1234&stream_id=1234&stream_position=preroll&stream_position=preroll&user_agent=Mozilla%2F5.0%20%28Macintosh%3B%20Intel%20Mac%20OS%20X%2010_15_7%29%20AppleWebKit%2F537.36%20%28KHTML%2C%20like%20Gecko%29%20Chrome%2F92.0.4515.159%20Safari%2F537.36&vault=0-YxSoUqGhh2-2GDi8oPd6PTgP675F7ezDqrQJ8wjZ9JEMHQBKERrNS5FHVPnFni3tfQj7mxTWmebH79AsaeeYtPHfoUuP7PNuKpJ7ZPzpSV4Cmdr8FeWGg4SDe&truexdm_channel=choicecard_mobile464985&iframed.mraid=1&appId=com.truex.skyline&disableWhiteops=false&disableMoat=false&ctv=1). The Bluescript code for the add is in the `creative_json.asset_args.layout` section.

BlueScript carries forward the "step" paradigm introduced in TVML with the top level structure being a list of "steps" (or _cards_) one can navigate between. For Roku, the entry-point (first step) for true[X] ads is always `main_card`. For HTML5, this is just a convention, the first step is initially shown regardless of its name.

Each step card contains a flat list of BlueScript elements. There is implied hierarchy in the elements being instantiated and added to the scene in order they are declared. Thus, earlier-declared elements will be drawn _underneath_ later-declared elements, if they occupy the same drawing region.

BlueScript elements support event handlers, enabling dynamic behavior. These can be used to trigger a variety of supported actions, such as controlling a video or playing sound effects. This is expressed as lists of behavior actions keyed on events. See below and `southback` repo (for Roku) and `Skyline` repo (for HTML5) for examples of this.

## Overall Structure

### Top Level Syntax
> _**Formatting note**: strings inside of carets (`"< ... >"`) are meta-descriptions, not absolute values_

Schema:
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
        "<ELEMENT NAME>" : {
          "<EVENT>" : [
            "<(LIST OF BEHAVIOR ACTIONS TRIGGERED BY EVENT)>"
          ]
        }
      },
      "functions": {
       "<FUNCTION NAME>": [
          "<(LIST OF BEHAVIOR ACTIONS TO RUN IN THE FUNCTION)>"
       ]
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

### Step

A **step** (or **card**) defines a single, self-contained screen. No two steps can be presented simultaneously.
Steps are recreated afresh whenever they are displayed. The ad developer can preserve state across steps with
the use of global "key variables" if they need to programmatically restore a previous visual state.

Step Property | Default | Description
--- | --- | ---
name | (required) | Name (String) of the step and used to identify it for behaviors.
elements | (required) | An array that contains [elements](#bluescript-element-reference) of this step. Z-indexed by the order, the first element is on the top, the last element is at the bottom.
behaviors | {} (Optional)| An object containing [named event handlers](#behavior-events), which define the behavior of elements, by their element names.
functions | {} (Optional) | An object containing named functions, which can be invoked from behavior events and other functions.

#### Elements

A step's "elements" defines the array of BlueScript visual elements that are to be displayed in each step - each element has the following properties:

Element Property | Default | Description
--- | --- | ---
type | (required) | Type for the BlueScript element. See [possible element types below](#bluescript-element-reference) for what is supported.
name | (required) | Name of the element and used to identify it for behaviors.
x | 0 | Screen-space X position, where x=0 is the left side of the (assumed 1080p) screen
y | 0 | Screen-space Y position, where y=0 is the top of the (assumed 1080p) screen
width | 0 | Specifies the width of the element on the screen. If 0 the width is computed dynamically from the element's visual content.
height | 0 | Specifies the height of the element on the screen. If 0 the width is computed dynamically from the element's visual content.
opacity | 1 | In the range [0, 1] where 0 is fully transparent and 1 is fully opaque

**Note:** More properties are available depending on the type of BlueScript element, detailed in 
the [element reference section](#bluescript-element-reference) below.

#### Behaviors

A step's "behaviors" property contains all behaviors for each element of the step - is an object with keys that match element names to a map of event handlers, where each event handler maps to an array of [behavior actions](#behavior-actions) to execute when the event occurs. BlueScript elements can define any number of behavior actions for any number of events.

For example:
```json
{
  "behaviors": {
    "button1": {
      "appear": [ <ACTIONS> ],
      "disappear": [ <ACTIONS> ],
      "onSelect": [ <ACTIONS> ]
    }
  }
}
```

#### Functions

A step's "functions" property contains all reusable functions, keyed by function name, that can be called from other event action execution, expressions, and even other functions.

One uses the `invoke` behavior action or expression to execute functions. Arguments are passed by name, and are referred to with the function via the `arg` expression.

The `return` host action can be used to return from the function immediately.

Local variable values can be assigned via the `local` assign action. Such variables exist only during the current invocation's execution.

<summary>For example:</summary>

```json
{
  "elements": [...],
  "functions": {
    "testFunction": [
      {"host": "assign", "local": "v", "value": {"arg": "x"}},
      {"host": "return", "value": {"+": [{"local": "v"}, {"local": "v"}]}}
    ]
  },
  "behaviors": {
    "testButton": {
      "appear": [
        {"host": "invoke", "function": "testFunction", "args": {"x": 123}},
        {"host": "debugLog", "value": {"invoke": "testFunction", "args": {"x": 123}}}
      ]
    }
  }
}
```

---

### Examples

For Roku: See the [example payloads](https://github.com/socialvibe/southback/tree/develop/channel_res/vastConfigs) in the `southback` repo, `vastConfig` folder.

For HTML5: See the [sample-bluescript.json](https://github.com/socialvibe/truex-bluescript-js/blob/develop/src/bluescript/__tests__/sample-bluescript.json) in the `truex-bluescript` repo.

## BlueScript Values and Expressions

Behavior actions take various values as input to their actions. There are 3 types:
1 - Literal Values
2 - Stored Values
3 - Expressions

NOTE: Boolean values are used as literals for attributes (e.g. a `Text` element's `wrap` attribute) and 
in expressions like in the `if` host action. When non-boolean values are used where a boolean value is expected, they
are evaluated in a "truthy" manner according to the following rules:
* false values are: `false`, 0, `null`, `undefined`, "", and "false".
* true values are `true`, 1, and in fact any value that is not a false value.
* in particular, non-null object and array values are evaluated as `true` in boolean contexts.

### Literal Values

The basic value types supported by BlueScript are those of the JSON format like strings, booleans, numbers, 
object literals, arrays.

Note that arrays are evaluated as expressions, objects may or may not be depending on where it is used. The general rule
is that all host action attributes are evaluated as expressions, all operation parameters are evaluated as expressions,
object literals that do not match any expression format are used as is.

If one is unclear whether or not an object is evaluated or not, they can used the "literal" expression to force the
unevaluated use of the value.

For example:
```json
 { "host" : "debugLog", "value" : "this is a string" },
 { "host" : "debugLog", "value" : 1234.56 },
 { "host" : "debugLog", "value" : true },
 { "host" : "assign", "key": "testObject", "value" : {"this": "this value", "that": "that value"} },
 { "host" : "assign", "key": "testArray", "value" : [1, 2, 3] },
 { "host" : "assign", "key": "testArray2", "value" : [1, 2, {"+": [3, 4]}] }, // final element is 7
 { "host" : "assign", "key": "testObject2", "value" : {"key": "testObject"} }, // testObject's value is used
 { "host" : "assign", "key": "testObject3", "value" : {"literal": {"key": "testObject"} } },
 { "host" : "debugLog", "value" : {"key": "testObject3.key"} } , // outputs "testObject" string
```

### Stored Values

Stored values are either global key variables, or local variables. Global values are available across steps, local values 
are available only within the scope of the currently executing behavior event handler, or current function invocation.

One uses the `assign` host action to set a stored value, and a `key` or `local` variable expression to access it.

Note that the obj.field "dot" selection notation can be used to assign values to and reference sub fields of an object value. 
The fields can also refer to numbers which are then interpreted as array indices.

For example:
```json
 { "host" : "assign", "key": "globalValue", "value":  123},
 { "host" : "debugLog", "value" : { "key": "globalValue" } },
 { "host" : "assign", "local": "localValue", "value": {"field_1": 1, "field_2": {"subField": [1, 2, 3]}}},
 { "host" : "debugLog", "value" : { "local": "localValue.field_1" } },
 { "host" : "debugLog", "value" : { "local": "localValue.field1_2.subField.2" } },
 { "host" : "assign", "local": "localValue.field_2.subField.2", "value":  4},
```

Certain implied global key variables always exist as read only values:

Name | Description
--- | ---
`userHasMetTIme` | The minimum time for ad interaction has been met
`userHasInteracted` | User has interacted with the ad
`userHasCredit` | User has gained credit for the ad
`timerTickValue` | Countdown remaining for the engagement ad or ChoiceCard/SkipCard
`host.ad` | Json object of the selected ad (Roku only, not supported on HTML5 yet)
`host.tagRotationRandom` | A fixed, random number from 0-1 per instance for (video) rotation. Ad author is supposed to read this number and change their ad accordingly.
`host.adParameters` | Contains all the adParameters from Truex Exchange and can be accessed by key.  EG. `host.adParameters.some_key`
`host.allVars` | Contains all of the ad variables queries from the RTB server. EG. `host.allVars.locationJSON.country_code`
`host.rotationVideos` | An object with numerous subproperties related to the set of rotation videos
`host.rotationVideos.<0-N>` | to get a specific video with that index (key: video_1 corresponds to `.0`)
`host.rotationVideos.length` | to get the number of rotational videos
`host.rotationVideos.currentIndex` | to get the index (base 0) of the current random video
`host.rotationVideos.currentUrl` | to get the url of the current random video

### Expressions

Expressions are used to compare values, perform arithmetic operations, logical operations, and perform functions that return value (random, for example), given an array of `values` as input.

NOTE: the older form of operation expressions was like:
```json
{ "host" : "debugLog", "value" : {"operation": "+", "values": [123, 123] } }, 
```
This form is still supported for backwards compatibility so as not to break existing ads. A simplified form is
now recommended, as shown with the examples below:

#### Arithmetic Operations
```json
{ "host" : "debugLog", "value" : {"+": [123, 123] } }, // 246
{ "host" : "debugLog", "value" : {"+": [1, 2, 3] } }, // 6
{ "host" : "debugLog", "value" : {"-": [10, 5] } },   // 5
{ "host" : "debugLog", "value" : {"*": [2, 4, 3] } }, // 24
{ "host" : "debugLog", "value" : {"/": [10, 2] } },   // 5
{ "host" : "debugLog", "value" : {"%": [17, 10] } },  // 7
{ "host" : "debugLog", "value" : {"floor": 2.3 } },   // 2
{ "host" : "debugLog", "value" : {"ceil": 2.3 } },    // 3
{ "host" : "debugLog", "value" : {"round": 2 } },     // 2
{ "host" : "debugLog", "value" : {"round": 2.3 } },   // 2
{ "host" : "debugLog", "value" : {"round": 2.5 } },   // 3
{ "host" : "debugLog", "value" : {"round": 2.8 } },   // 3
```

#### String Operations
```json
 { "host" : "debugLog", "value" : {"+": ["full name: ", "John", " ", "Doe"] } }, // full name: John Doe
 { "host" : "debugLog", "value" : {"replace": ["This is badass", "badass", "awesome"]}}, // This is awesome 
```

#### Comparison Operations
```json
{ "host" : "debugLog", "value" : {"==": [1, 1] } },           // true
{ "host" : "debugLog", "value" : {"==": ["this", "that"] } }, // false
{ "host" : "debugLog", "value" : {"!=": [1, 1] } },           // false
{ "host" : "debugLog", "value" : {"!=": ["this", "that"] } }, // true
{ "host" : "debugLog", "value" : {"<": [2, 3] } },           // true
{ "host" : "debugLog", "value" : {"<=": [3, 3] } },           // true
{ "host" : "debugLog", "value" : {">": [5, 4] } },           // true
{ "host" : "debugLog", "value" : {">=": [5, 5] } },           // true
{ "host" : "debugLog", "value" : {"&&": [true, false] } },    // false
{ "host" : "debugLog", "value" : {"||": [true, false] } },    // true
{ "host" : "debugLog", "value" : {"!": true } },              // false
```

#### Variable References
```json
{"host": "assign", "key": "globalVar", "value": 123},
{"host": "debugLog", "value": {"key": "globalVar"}},
{"host": "assign", "key": "localVar", "value": "something else"},
{"host": "debugLog", "value": {"local": "localVar"}},
{
  "host": "assign", "key": "objectValue", "value": {
    "literal": {
      "this": 123,
      "that": {"a": true, "b": false}
    }
  }
},
{"host": "debugLog", "value": {"key": "objectValue.that.a"}},
{"host": "assign", "key": "objectValue.that.b", "value": true},
        
{"host": "debugLog", "value": {"arg": "x"}}, // error if "x" not supplied when function invoked
{"host": "debugLog", "value": {"arg": "x", "default": 0}}, // default is used if not supplied
```

#### Date Expressions
```json
{"host": "assign", "local": "now", "value":  {"date": "now"}}, // from clock
{"host": "assign", "local": "now", "value":  {"date": 1631316792480}}, // from timestamp
{"host": "assign", "local": "now", "value":  {
   "date": {
      "year": 2021, "month":  9, "day": 10, "hours": 16, "minutes": 33, "seconds": 12, "milliseconds": 480
   }
}},
{ "host" : "debugLog", "value" : {"local":  "now.year"} },           // 2021
{ "host" : "debugLog", "value" : {"local":  "now.month"} },          // 9
{ "host" : "debugLog", "value" : {"local":  "now.day"} },            // 10
{ "host" : "debugLog", "value" : {"local":  "now.hours"} },          // 16
{ "host" : "debugLog", "value" : {"local":  "now.minutes"} },        // 33
{ "host" : "debugLog", "value" : {"local":  "now.seconds"} },        // 12
{ "host" : "debugLog", "value" : {"local":  "now.milliseconds"} },   // 480
{ "host" : "debugLog", "value" : {"local":  "now.weekDay"} },        // 5
{ "host" : "debugLog", "value" : {"local":  "now.timezoneOffset"} }, // 480
{ "host" : "debugLog", "value" : {"local":  "now.time"} },           // 1631316792480, e.g. millis since Jan 1, 1970

{"host": "assign", "local": "millisPerWeek", "value": {"*": [7, 24, 60, 60, 1000]}},
{
  "host": "assign", "local": "nextWeek", 
  "value": {"date": {"+": [{"local": "now.time"}, {"local": "millisPerWeek"}]}}
},

{ "host": "assign", "local": "yearStart", "value":  {"date": {"year": 2021}} },
{ "host" : "debugLog", "value" : {"local": "yearStart.month"} },   // 1
{ "host" : "debugLog", "value" : {"local": "yearStart.day"} },     // 1
{ "host" : "debugLog", "value" : {"local": "yearStart.hours"} },   // 0
{ "host" : "debugLog", "value" : {"local": "yearStart.minutes"} }, // 0
{ "host" : "debugLog", "value" : {"local": "yearStart.seconds"} }, // 0
{ "host" : "debugLog", "value" : {"local": "yearStart.time"} },    // 1612166400000

{ "host": "assign", "local": "dec1999", "value":  {"date": {"year": 1999, "month": 12}} },
{ "host" : "debugLog", "value" : {"local": "dec1999.year"} },   // 1999
{ "host" : "debugLog", "value" : {"local": "dec1999.month"} },  // 12
{ "host" : "debugLog", "value" : {"local": "dec1999.day"} },    // 1
{ "host" : "debugLog", "value" : {"local": "dec1999.time"} },   // 944035200000
```

#### Misc Expressions
```json
{ "host" : "debugLog", "value" : {"element": "button1", "attribute": "image_url"} }, // element attributes

{"host": "debugLog", "value": {"invoke": "testFunction", "args": {"x": 123}}},

{ "host" : "debugLog", "value" : {"random": 10 } },                // random 0 to 9, excluding 10

{ "host" : "assign", "value" : {"literal": {"random": 10}} }, // {"random": 10}, i.e. unevaluated expression

{ "host" : "debugLog", "value" : {"length": ["abcde"] } },           // 5
{ "host" : "debugLog", "value" : {"length": "abcde" } },           // single arg convenience
{ "host" : "debugLog", "value" : {"length": [["a", "b", "c"]] } }, // 3 ; note array args require explicit array wrapper

{ "host" : "debugLog", "value" : {"toFixed": [3.1415, 3]} },       // 3.142

{ "host" : "debugLog", "value" : {"toTrimFixed": [2.000001, 4]} }, // 2

{ "host" : "debugLog", "value" : {"zeroFill": [123, 5]} },         // 00123

{ "host" : "debugLog", "value" : {"formatMinutesSeconds": 3599} },      // "59:59"
{ "host" : "debugLog", "value" : {"formatHoursMinutesSeconds": 3599} }, // "0:59:59"
```

#### Expression Composition
```json
{ "host" : "debugLog", "value" : {"+": [1, {"*": [2, {"local": "testValue"}] } ] } }
```

---

## BlueScript Element Reference

This section documents the available elements available for creating the diplayed UX of a step, 
i.e. the possible elements of a step's "elements" array.

### Rectangle

Renders a single-colored rectangle at the specified screen coordinates.

For example:
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

Additional Rectangle Properties:

Property | Default | Description
--- | --- | ---
color | 0xFFFFFFFF | Specifies the RGBA color drawn to the rectangle region
blendingEnabled | true | Specifies if the rectangle should be alpha blended with the nodes that are behind it

---

### Image

Renders an image at the specified screen coordinates, such that the image is scaled to fit within the element's 
specified size.

For example:
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

Additional Image Properties:

Property | Default | Description
--- | --- | ---
image_url | (required) | Specifies the URI to the image file.
forceHighResolution | false | Use the image's native / high resolution on memory constrained low end Roku devices. See below [Memory Management](#memory-management) note.

---

### Video

Renders a video at the specified screen coordinates.

For example:
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

Additional Video Properties:

Property | Default | Description
--- | --- | ---
video_url | (required) | Specifies the URI to the video file. MP4 format is expected.
loop | true | If set to true, the video will be restarted from the beginning after the end is reached.
autoplay | true | If set to true, the video will be automatically started on card display.
replayable | false | If set to true, makes the video player focusable on playback completion, enabling replay of the video.
mute | false | Set to true to mute the audio of the video currently playing. Set to false to restore audio.
fullscreen | false | When toggled, the video will be animated to fullscreen, or back to window
zoomAnimationDuration | 0.35 | The time it takes to animated between window and fullscreen. The 0.35 is copie from tvOS TAR. (Note: must be > 0)
easeFunction | "inOutCubic" | This string defines the animation curve. For full list of easeFunction, see: https://sdkdocs.roku.com/display/sdkdoc/Animation#Animation-Fields
currentTime | 0 | This value is the floating point time of the current stream.  This has a margin of error of 500ms due to how position is handled.
trackingName | | When set, tracking calls utilize this name instead of the uri to identify videos. Greatly facilitates analysis. Note that when `useRotation` set to true, a `_n` suffix will be added to the trackingName, where `n` is the index of the video with "1 indexing".
preview_image_url | | When set, implements a static image to overlay on top of the video player on video playback completion.
focusable | false | Set to true in order to make the video focusable.
leftFocus | invalid | ID of element to focus when left directional button is pressed.
rightFocus | invalid | ID of element to focus when right directional button is pressed.
upFocus | invalid | ID of element to focus when up directional button is pressed.
downFocus | invalid | ID of element to focus when down directional button is pressed.
autoFocus | false | If set to true, the video (if focusable) will be focused by default.
useRotation | false | If set to true, will ignore the video url and try to apply the value of `video_n` from the ad parameters.

NOTE: On Roku, only a single Media node may be actively playing or buffering media at a given time. This means that it is not possible to have multiple inline videos playing concurrently or a background / ambient video with inline window windows on top. This also precludes the use of background audio playing together with a video.

Related video actions: [playVideo](#playVideo), [pauseVideo](#pauseVideo), [resetVideo](#resetVideo), [stopVideo](#stopVideo)

---

### Text

Renders a text label at the specified screen coordinates.

For example:
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

Additional Text Properties:

Property | Default | Description
--- | --- | ---
text |  | Specifies the text string to render in the Text element.
color | 0xddddddff | Specifies the color of text rendered in the text element.  Supports HTML code formats.  RGB formats such as rgb(r,g,b) rgba(r,g,b,a) rgba(r%,g%,b%,a%) with commas or whitespace.  And Hex formats such as  #RGB #RGBA #RRGGBB #RRGGBBAA.
backgroundColor | 0x00000000 | Specifies the background color for the text element, covers the final calculated width and height of the element.  Supports HTML code formats (see color above)
fontName | (system default) | (Roku only) Specifies the font name for the text rendered in the text element. That is either a [built-in](https://sdkdocs.roku.com/display/sdkdoc/Typography) system font, or a TTF/OTF font we ship with the library. For portability with HTML5 ads, a font_url asset is recommended to use instead. 
font_size | 24 | Size of the font, in points. Note that this is only supported for OTF/TTF files, as the default system fonts imply a given font size.
font_url |   | Fully qualified URL of an OTF/TTF font asset to be dynamically loaded.  Externally hosted unlike fontName and must end with extension .otf/.ttf.
alignment | center | Horizontal alignment, one of `left`, `center` or `right`.
vertical_alignment | top | Vertical alignment, one of `top`, `center` or `bottom`.
wrap | true | The wrap field is used to control how the text is broken into multiple lines. If false, a single line of text will be displayed. Else, the text is broken over multiple lines each of the calculated width, up to the specified height.

NOTE:
* Manipulating properties of the text element such as `text` `color` and `backgroundColor` can be achieved using the `setAttribute` behavior action.
* On Roku, the Label element is a thin wrapper on top of a `<Label/>` SceneGraph node. Refer to the [Roku documentation](https://sdkdocs.roku.com/display/sdkdoc/Label) for detailed description of layout and wrapping behavior.

---

### Button

Renders a focusable button at the specified screen coordinates.

For example:
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

Additional Button Properties:

Property | Default | Description
--- | --- | ---
image_url | (required) | Specifies the URI to the image file used for the button.
hover_image_url |  | Specifies the URI to the image file used for the button's focused state.
checked_image_url |  | Specifies the URI to the image file used for the button's enabled or checked state.
hover_checked_image_url |  | Specifies the URI to the image file used for the button's focused state when it is used as a toggle style button.
checked | false | Whether this toggle style button is set to "on" by default.
hover_effect | true | If set to true, upon focus button grows by a scale factor of the hover_scale attribute, with no change in the image to the hover_image_url.
hover_scale | 1.1 | A float value that indicates the how big, in percentage, this button should grow on hover.
focusable | true | Set to false in order to make the button not focusable.
forceHighResolution | false | Use the button's native / high resolution on memory constrained low end Roku devices. See below [Memory Management](#memory-management) note.
leftFocus | invalid | ID of element to focus when left directional button is pressed.
rightFocus | invalid | ID of element to focus when right directional button is pressed.
upFocus | invalid | ID of element to focus when up directional button is pressed.
downFocus | invalid | ID of element to focus when down directional button is pressed.
autoFocus | false | If set to true, the button will be focused by default.

Note: manipulating properties of the button such as `checked` can be achieved using the `setAttribute` behavior action.

---

### Audio

Plays background audio.

For example:
```json
{
   "type"      : "Audio",
   "name"      : "backgroundAudio",
   "audio_url"       : "http://development.scratch.truex.com.s3.amazonaws.com/roku/simon/audio_tester/audio_test.mp3",
   "autoplay"  : false,
   "loop"      : true
}
```

Additional Audio Properties:

Property | Default | Description
--- | --- | ---
audio_url | (required) | Specifies the URI to the audio file. MP3 format* is expected. Always use the Asset Uploader.
loop | true | If set to true, the audio will restart from the beginning after the end is reached.
autoplay | true | If set to true, the audio will be automatically started on card display.
global | false | If true, audio sticks/continues playing across step card transitions.

NOTES:
* Visual properties are not relevant for the Audio element, e.g. x, y, width, height, opacity.
* On Roku, only a single Media node may be actively playing or buffering media at a given time. This means that it is not possible to have multiple inline videos playing concurrently or a background / ambient video with inline window windows on top. This also precludes the use of background audio playing together with a video. Any additional stream started after the first one will fail silently.
* For the MP3 format. On the PlayStation 5 and some other platforms, MP3 is not supported. BlueScript engine will try to get the MP4 version (same location, same name) of the given audio instead. Asset Uploader in True Exchange is encoding that MP4 file automatically. Be sure to include a MP4 version of your audio file if you cannot use the Asset Uploader.

Related audio actions: [playActiveAudio](#playActiveAudio), [pauseActiveAudio](#pauseActiveAudio), 
[resetActiveAudio](#resetActiveAudio), [stopActiveAudio](#stopActiveAudio)

---

### QRCode

Renders a QR Code as image using the [qr-code-generator](https://github.com/socialvibe/ui_misc/tree/master/lambda_functions/qr-code-generator).

For example:
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

Additional QRCode Properties:

Property | Default | Description
--- | --- | ---
url | | Specifies the information that is encoded in the QR Code image. This is also the fallback url for the tagLabel option. Using this field without setting the tagLabel will generate a QR Code contains only the information in this field, with no 1st party tracking. One should give the tagLabel a value (that does not have "Click" tag set) to get the trackings.
size | dynamic | Specifies the native size in pixels of the QR Code image being returned.
margin | 4 | Specifies the margin, in number of dots in QR Code matrix.
color | 0xddddddff | Specifies the color of dots rendered in the QR Code.
backgroundColor | 0x00000000 | Specifies the background color for the QR Code, covers the final calculated width and height of the element.
minify | false | Specifies if we would pass final url to `multidevice.truex.com/minify` for minimize.
tagLabel | (empty) | Specifies if this QR Code should take its url from the tag manager with the matching label. When set, the `url` field is ignored, and TAR will try to get the last matching label from the Tag Mangager, with Trigger "QR Code" (`qr_code`), and Type "Click"; all the other 3rd party pixels with Trigger "QR Code" (`qr_code`), and Type "Pixel"; as well as a 1st party pixel. We will use the `interact.live` redirect service to redirect, and fires all the necessary pixel. If there is no matching "Click" with this given the tagLabel, TAR will use the url field as the fallback redirect url.

---

## BlueScript Behavior Reference

### Behavior Events

The following are the top-level events that behaviors can be triggered from:

Event | Description
--- | ---
appear | Triggered when the element's parent card is displayed. Note that appear does not tie to visibility.
disappear | Triggered when the element's parent card is dismissed, such as on a card transition or when ending the ad flow.
onSelect | Triggered when a button with the focus is clicked on or the select/enter button is pressed on the remote.
onFocusGained | Triggered when a button or video gains the remote/keyboard focus.
onFocusLost | Triggered when a button or video loses the remote/keyboard focus.
timerTick | Triggered when the countdown timer updates its value. <br/> Use the `{"key": "timerTickValue"}` expression to access the current timer value.
videoStarted | Triggered when a video starts playback.
videoFirstQuartile | Triggered when 25% of the video has played.
videoSecondQuartile | Triggered when 50% of the video has played.
videoThirdQuartile | Triggered when 75% of the video has played.
videoCompleted | Triggered when the video has completed playback.
videoLooped | Triggered when the video has automatically restarted playback.
videoDidEnterFullscreen | Triggered when the video is to fullscreen size.
videoDidExitFullscreen | Triggered when video is set back to its original size.

### Behavior Actions

This section details the behavior actions that can be scripted. Actions are invoked when Events occur, 
or when a BlueScript function is invoked.

##### `allDoneButtonPushed`

Triggers the "Return to Content" button, exiting the ad flow for a completed ad.

---

##### `animateElement`

Animates Bluescript element attributes. This action is only supported on rendered elements (not Audio), and only with certain fields (see below).

Parameter | Default | Description
--- | --- | ---
attributes | (required) | and object defining which attributes are to be animated; (x, y, width, height and opacity. position and size are supported legacy attributes)
duration | 0.35 | the length of time (in seconds) it will take to play the animation
easeFunction | `outCubic` | how the values will evolve throughout the animation duration
optional | false | (Roku only) whether or not the animation can be ignored for low-end devices under stress

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

---

##### `assign`

Behavior Action `assign` is an assignment statement that sets and/or re-sets the value stored in the storage denoted by a variable name (named by key); in other words, it copies a value into the variable.

For example:
```json
 { "host" : "assign",
   "key" : "i",
   "value" : 0 },
 { "host" : "assign",
   "key" : "i",
   "value" : { "+": [{ "key": "i" }, 1] } },
 { "host" : "assign",
   "key" : "object.array.0",
   "value" : "Hello World" },
 { "host" : "assign",
   "key" : { "+": ["object.array.", { "key": "i" } ] ,
   "value" : { "key": "i" }},
 }
```

Parameter | Description
--- | ---
key | Name of global variable. It can be a dot seperated list to indicate variable inside objects, or array. (could be [value or expression](#values-and-expressions))
local | Name of a local variable. Either `key` or `local` must be specified.
value | Value of the variable (could be an expression).

All globals values are maintained across step displays. Local variables are maintained during the current event handler or function invocation.

---


##### `bringToFront`

Changes child order of a TXElement so that it draws on top of all other views.

Parameter | Description
--- | ---
name | Name of the node to bring to the front of the view

---


##### `debugLog`

On Roku, prints a log message to BrightScript terminal. Attach ` | grep "debugLog: "` at the end of `telnet` comment to filter for `debugLog`.

On HTML5, prints a log message to the browser's console log. Examine the console log of the browser the ad is running in.

For example:
```json
{ "host" : "debugLog", "value" : "This is a String." }, // Prints: This is a String.
{ "host" : "debugLog", "value" : {"key": "keyOfValue"}  }, // Prints: what ever is in keyOfValue.
{ "host" : "debugLog", "value" : {"+": ["Plus ", "operation."]}  }, // Prints: Plus operation.
```

Parameter | Description
--- | ---
value | The (string) value to print out. This can be a [Value or Expression](#values-and-expressions).

NOTES: We would always try to cast any types into string.

---


##### `disableUserInput`

Prevent user input from being handled.

---


##### `disableUserNavigation`

Prevent user directional input from being handled.

---


##### `enableUserInput`

Allow user input to be handled.

---


##### `enableUserNavigation`

Allow user directional input to be handled

---


##### `flagActivityForAttention`

Flags the engagement as having been interacted with, fulfilling the interaction requirement of true[ATTENTION].

---


##### `focusElement`

Sets the focus to a specified button or video. onFocusGained and onFocusLost are triggered when switching focus.

Parameter | Description
--- | ---
name | Name of the node that will capture focus

---


##### `for`

Behavior Action `for` is a control flow statement for specifying iteration, which allows Behavior Actions to be executed repeatedly. Requires a control variable with `from` and `to` values, and a `do` Behavior Actions Array. An optional `value` can be used to specified a "key" or "local" variable to assign the loop value to.

For example:
```json
 { "host" : "for",
   "from": 0,
   "to": 3,
   "do" : [
       { "host" : "debugLog", "value" : "test" }
   ] },

 { "host" : "for",
   "value": { "key": "i" },
   "from": 0,
   "to": { "key": "ta" },
   "do" : [
       { "host" : "debugLog", "value" : { "key": "i" } }
   ] },
```

Parameter | DescriptionG
--- | ---
value | (optional) Local or key variable that the counter should save to. Default `{ "key": "forI" }`
from | An integer value that the loop counter would start from (could be [Value or Expression](#values-and-expressions))
to | An integer value that the loop counter would count to (inclusive) (could be [Value or Expression](#values-and-expressions))
do | An array of Behavior Actions that should be executed

NOTE: One can break out of the loop via a `break` action, or even a `return` action to exist the current event handler or function entirely.

---

##### `if/else`

The `if/else` Behavior Action is a conditional statement, which perform different actions depending on whether the `expression` boolean condition evaluates to true or false.

For example:
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
   "expression" :  {"==": [{ "key": "myObject.txLogLevel" }, 3] },
   "then" : [{  "host" : "debugLog",
                "value" : "4) txLogLevel is 3" }],
   "else" : [{  "host" : "debugLog",
                "value" : "4) txLogLevel is not 3" }] }
```

Parameter | Description
--- | ---
expression | Expression is a [Value or Expression](#values-and-expressions), which determines the flow of the Behavior Actions
then | An array of Behavior Actions that should be executed if expression is true
else | (optional) An array of Behavior Actions that should be executed if expression is NOT true

NOTE: There is no `else if` as of now.

---

##### `makeWebRequest`

Enables Behavior Actions to read the HTTP values sent by a client during a Web request.

For example:
```json
 { "host" : "makeWebRequest",
   "url" : "https://api.weather.gov/gridpoints/SEW/124,67/forecast/hourly",
   "responseAsJson": true,
   "assignResponseTo": { "key": "weatherJson" },
   "onload": [
             { "host" : "debugLog",
               "value" : { "key": "weatherJson.properties.periods.0" } } ] }
```

Parameter | Description
--- | ---
url | The server/file location.
assignResponseTo | take a json with a `key` or `local` entry, which points to variable that the returned string/json will be assigned to.
responseAsJson | (optional, default: false) will try to parse the return as JSON before save.
onload | An array of Behavior Actions to be executed on call successed.
onerror | An array of Behavior Actions to be executed on call failed.

---

##### `pauseActiveAudio`

Pauses playback for the active audio element.

---

##### `pauseVideo`

Pauses playback for the target video.

Parameter | Description
--- | ---
target | Name of the video to pause.

---

##### `playActiveAudio`

Starts or resumes playback (if currently paused) for the active audio element.

---

##### `playSoundEffect`

Triggers a sound effect for playback.

Parameter | Description
--- | ---
uri | Uri of the sound file to trigger playback for.

NOTES:
* On Roku, `playSoundEffect` is a wrapper on top of the [`<SoundEffect/>`](https://sdkdocs.roku.com/display/sdkdoc/SoundEffect) SceneGraph component. It is notably limited to WAV files as a format for the sound effects.
* On HTML5, the standard .mp3, .wav, etc. file types work.

---

##### `playVideo`

Starts or resumes playback (for a paused video) for the target video.

Parameter | Description
--- | ---
target | Name of the video to play.

---

##### `popStep`

[Deprecated: "step stacking" is no longer supported, not even available on HTML5. Use explicit `replaceStep` actions instead.]

Removes the current BlueScript step from the stack, replacing it with the next step on the navigational stack.

---

##### `replaceStep`

Navigates to a different BlueScript step, replacing the current step being displayed.

Parameter | Description
--- | ---
cardName | Name of the step to transition to, as defined in the top level list of BlueScript steps.

---

##### `resetActiveAudio`

Resets playback for the active audio element by setting the play head to the beginning.

---

##### `resetFocus`

Re-initializes the focus handler and ensures a default control is focused, which is usually the visually top most / left most button.
This can be set programmatically in an "appear" event handler via the `focusElement` action.

---

##### `resetVideo`

Resets playback for the target video by setting the play head to the beginning.

Parameter | Description
--- | ---
target | Name of the video to reset.

---

##### `setAttribute`

Assigns a new value to given property for the script element, typically controlling a visual aspect of the underlying component.

Parameter | Description
--- | ---
name | Name of the BlueScript element that will have one of its underlying properties changed.
key | Name of the underlying property to update.
value | New value for the underlying property.

NOTE: On Roku, `setAttribute` manipulates the SceneGraph component underlying the BlueScript element. In effect, it allows access to underlying implementation for the element. This could have adverse effects.

---

##### `setBounds`

Instantly sets the bounds / position of an element.  An alternative to animateElement when duration = 0

Parameter | Default | Description
--- | --- | ---
target |  | the name of the target node
x |  | new x position
y |  | new y position
width |  | new width of element
height |  | new height of element

NOTES: All new values are optional by excluding them in the action.  For example, if you only want to change x,y and leave width,height alone.

---

##### `setTimeout`

Behavior Action `setTimeout` allows execution of a set of Behavior Actions at specified time intervals. (One time, or repeat)

For example:
```json
 { "host" : "setTimeout",
   "duration" : 3,
   "do": [ { "host" : "debugLog", "value" : "timeout fired after 3 seconds" } ] }
```

Parameter | Description
--- | ---
repeat | (optional, default false) A boolean value that indicate if the timer will fire repeatedly.
duration | `duration` indicates the number of seconds before execution. Note: we will tolerate `timeout`, or `delay`
do | An array of Behavior Actions to be executed.

NOTES: Creative is expected to stop their repeated timers.
Currently there are no menthod to stop an individual timer.

---

##### `showStep`

[Deprecated: "step stacking" is no longer upported, not even available on HTML5. Use explicit `replaceStep` actions instead.]

Navigates to a different BlueScript step, pushing the new step onto the navigational stack. A viewer is able to get back to the previous step using the BACK key or equivalent.

Parameter | Description
--- | ---
cardName | Name of the step to transition to, as defined in the top level list of BlueScript steps.


##### `stopAllTimers`

The `stopAllTimers` stops the execution of all the timers specified in setTimeout(), and clear all the saved timers.

---

##### `stopActiveAudio`

Stops playback for the active audio element.

---

##### `stopVideo`

Stops playback for the target video.

Parameter | Description
--- | ---
target | Name of the video to stop.

##### `trackCustomEvent`

Tracks an event back to the true[X] server. This can be used to report back custom events and activity done by a viewer in the unit, such that it can be used for client-facing reports later on.

For example:
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

Parameter | Default | Description
--- | --- | ---
category | `fep_roku_layout` | The tracking taxonomy category to use for the tracking call. Typically omitted so default value is used.
name | (required) | The name of the tracking event.
value | | A value to use for the event, if appropriate.

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
