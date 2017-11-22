## Choice Card Configuration Parameters

This page lists and describes all the possible parameters for configuring a choice card for mobile and desktop.  For connected device choice cards see [Connected Device Choice Card VAST Wrapper Query Parameters](https://github.com/socialvibe/truex-ads-docs/blob/master/connected_device_vast_wrapper_query_args.md)

| Parameter | Type | Description |
| ------------- | ------------- | ------------- |
| name  | `string`  | Tracking label used in all tracking events for the specific card configuration.  Useful for a/b testing.  Note: not used in Roku tracking.  |
| background_color  | `hex`  | Hex value used as the base background color.  Example: "#000000".  |
| background_image  | `image`  | Object to configure background image. See below for `image` properties.  |
| background_video  | `video`  | Object to configure background video. Video will be sized to dimensions of the player and does not loop.  See below for `video` properties.  |
| brandable_image  | `image`  | Object to configure the brandable image.  See below for `image` properties.  |
| watch_button  | `button`  | Object to configure the watch normal ads button. See below for `button` properties.  |
| interact_button  | `button`  | Object to configure the interact with engagement button. See below for `button` properties.  |
| button_delay  | `integer`  | Number of seconds to delay the showing of all buttons within a card configuration.  Decimal values are accepted.  |
| voiceover  | `URL`  | Array of MP3s URLs to use for the voice over.  |
| timer_text  | `text`  | Object to configure the countdown timer.  See below for `text` properties.  |
| timer_seconds  | `integer`  | Number of seconds to count down from for the auto advance timer.  |
| preroll_override  | `object`  | Special Object that allows the Survey Card to override parameters in the pre-roll position.  See below for syntax.  |

##### `image` object
```javascript
{
    "width": 1920,
    "height": 1080,
    "x": 0,
    "y": 0,
    "image_url": "https://"
}
```

##### `video` object
```javascript
{
    "video_url": "https://"
}
```

##### `button` object
```javascript
{
    "image_url": "https://",
    "hover_image_url": "https://",
    "width": 395,
    "height": 133,
    "x": 1036,
    "y": 699
}
```

##### `button` object
```javascript
{
    "image_url": "https://",
    "hover_image_url": "https://",
    "width": 395,
    "height": 133,
    "x": 1036,
    "y": 699
}
```

##### `text` object
```javascript
{
    "text": "Normal break with multiple commercials in [countdown]",
    "width": 730,
    "height": 50,
    "x": 670,
    "y": 810,
    "color": "#DDDDDD",
    "family": "Arial, sans-serif",
    "size": 33
}
```

##### `preroll_override` object
```javascript
{
    "watch_button": {
        "image_url": "//",
        "hover_image_url": "//"
    },
    "interact_button": {
        "image_url": "//",
        "hover_image_url": "//"
    },
    "voiceover": "https://",
    "timer_text": {
        "text": ""
    }
}
```
---
##### Example
```javascript
{
    "name": "generic_80_20_survey_card",
    "background_color": "#000000",
    "background_image": {
        "image_url": "//media.truex.com/image_assets/2017-07-31/a14c192a-0959-45d8-b9e4-e695b75f6ed3.jpg"
    },
    "watch_button": {
        "width": 1236,
        "height": 159,
        "x": 599,
        "y": 752,
        "image_url": "//media.truex.com/image_assets/2017-06-22/f8b11269-04ca-44f8-9bf9-1fbb5ae1f02c.png",
        "hover_image_url": "//media.truex.com/image_assets/2017-06-22/f073eed0-889d-4b40-9849-bbaa1c9e5960.png"
    },
    "interact_button": {
        "width": 1236,
        "height": 537,
        "x": 600,
        "y": 177,
        "image_url": "//media.truex.com/image_assets/2017-06-27/7e0018e2-9ca5-4b8e-89b5-27fde77bee9f.png",
        "hover_image_url": "//media.truex.com/image_assets/2017-06-27/90395f4d-d246-4854-8721-7e478f93ba78.png"
    },
    "voiceover": "https://media.truex.com/audio_assets/2017-08-25/b3db4828-a109-469d-b6cf-c73633c6af06.mp3",
    "timer_text": {
        "text": "Normal break with multiple commercials in :[countdown]",
        "width": 730,
        "height": 50,
        "x": 670,
        "y": 810,
        "color": "#DDDDDD",
        "family": "Arial, sans-serif",
        "size": 33
    },
    "timer_seconds": 15,
    "preroll_override": {
        "watch_button": {
            "image_url": "//media.truex.com/image_assets/2017-06-22/a8a4a57b-3791-48ff-a3ee-2b815d2dce25.png",
            "hover_image_url": "//media.truex.com/image_assets/2017-06-22/910b6360-e510-4b12-ada9-36554cf4eb08.png"
        },
        "interact_button": {
            "image_url": "//media.truex.com/image_assets/2017-06-27/4cddd086-9475-4edc-9ac6-c1a509143755.png",
            "hover_image_url": "//media.truex.com/image_assets/2017-06-27/2a6e3f18-4bc4-4d74-9ea2-1dcac8f367ae.png"
        },
        "voiceover": "https://media.truex.com/audio_assets/2017-08-25/8b817c73-d352-4d5b-9bde-770fbcf9cc00.mp3",
        "timer_text": {
            "text": ""
        }
    }
}
```
