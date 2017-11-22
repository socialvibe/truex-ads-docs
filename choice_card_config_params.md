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
