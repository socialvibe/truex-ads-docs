## Overlay Configuration Parameters

This page lists and describes all the possible parameters for configuring an overlay 'card'.

| Parameter | Type | Description |
| ------------- | ------------- | ------------- |
| name  | `string`  | Tracking label used in all tracking events for the specific card configuration.  Useful for a/b testing.  Note: not used in Roku tracking.  |
| width  | `integer`  | Width in pixels of overlay, based on 1920x1080 screensize.  |
| height  | `integer`  | Height in pixels of overlay, based on 1920x1080 screensize.  |
| background_color  | `hex`  | Hex value used as the base background color.  Example: "#000000".  |
| background_image  | `image`  | Object to configure background image. See below for `image` properties.  |
| brandable_image  | `image`  | Object to configure the brandable image.  See below for `image` properties.  |
| interact_button  | `button`  | Object to configure the interact with engagement button. See below for `button` properties.  |
| timer_text  | `text`  | Object to configure the countdown timer.  See below for `text` properties.  |
| timer_seconds  | `integer`  | Number of seconds to count down from for the auto advance timer.  |
| start_animation  | `animation`  | Object to configure the intro animation.  See below for `animation` properties.  |
| end_animation  | `animation`  | Object to configure the outro animation.  See below for `animation` properties.  |

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

##### `animation` object
```javascript
{
    "start_x": 1920,
    "start_y": 30,
    "end_x": 1810,
    "end_y": 30,
    "duration": 2
}
```
---
##### Example
```javascript
{
    "name": "fox_sports_overlay",
    "background_color": "#000000",
    "background_image": {
        "image_url": "//media.truex.com/image_assets/2017-07-31/a14c192a-0959-45d8-b9e4-e695b75f6ed3.jpg"
    },
    "interact_button": {
        "width": 1236,
        "height": 537,
        "x": 600,
        "y": 177,
        "image_url": "//media.truex.com/image_assets/2017-06-27/7e0018e2-9ca5-4b8e-89b5-27fde77bee9f.png",
        "hover_image_url": "//media.truex.com/image_assets/2017-06-27/90395f4d-d246-4854-8721-7e478f93ba78.png"
    },
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
    "timer_seconds": 10,
    "start_animation": {
        "start_x": 1920,
        "start_y": 30,
        "end_x": 1810,
        "end_y": 30,
        "duration": 2
    },
    "end_animation": {
        "start_x": 1810,
        "start_y": 30,
        "end_x": 1920,
        "end_y": 30,
        "duration": 1
    }
}
```
