## Roku TruexAdRenderer (TAR) Choice Card Parameters

The following lists and describes each supported parameter for TruexAdRenderer (TAR) based Choice Cards on the Roku platform.  **All pixel calculations are based on 720p (see notes below).**

---

| Parameter | Default Value | Description |
| ------------- | ------------- | ------------- |
| ix | n/a | x-coordinate of the interact button. |
| iy | n/a | y-coordinate of the interact button. |
| iw | n/a | Width of the interact button. |
| ih | n/a | Height of the interact button. |
| ib | n/a | URL to sprite image for 'interactive ad' button. |
| wx | n/a | x-coordinate of the watch button. |
| wy | n/a | y-coordinate of the watch button. |
| ww | n/a | Width of the watch button. |
| wh | n/a | Height of the watch button. |
| wb | n/a | URL to sprite image for 'watch video ads' button. |
| bd | 3 | a value that specifies the number of seconds to delay when all buttons should be shown.  Only supported for video choice cards. |
| bg | n/a | URL for choice card background image. Used whem `bgv` is not present and turns on static choice card mode. |
| bgv | n/a | URL for choice card background video. When present the choice card becomes a video choice card. |
| bgv_fmp4 | n/a | URL for choice card background video in fragmented MP4 format.  This is currently needed for Hulu's Neutron renderer. |
| vo | n/a | mp3 URL for choice card voice over audio file. Only supported for static choice cards. |
| sk | n/a | URL of skip card image or mp4. Can be overridden at runtime by any branded cards sent down by the ad server. |
| aa | 5 (skip cards), 30 (choice cards) | Number of seconds for auto advance timer. Typically, 30 seconds for Choice Cards, 5 seconds for Skip Cards. |
| pos | n/a | Value will be `preroll` or `midroll`--indicates sponsored stream or sponsored ad break. |
| product | n/a | Value will be `sponsored_stream`, `sponsored_ad_break`, `continue`, or `stream_pass`--indicates the product type.  If not specified, please default to `sponsored_stream` when pos=`preroll` and default to `sponsored_ad_break` when pos=`midroll`. |
| ct | [countdown] | format of string used to display remaining countdown time, [countdown] will be replaced with number of seconds remaining. |
| ctx | 0 | x-coordinate of the countdown timer label. |
| cty | 0 | y-coordinate of the countdown timer label. |
| ctw | 0 | width of the countdown timer label. Setting to 0 (or not setting) automatically sizes the label to fit the text |
| cth | 0 | height of the countdown timer label. |
| cts | 24 | font size for text in the timer label. |
| ctf | system default | font name for text in the timer label. |
| ctc | 0xddddddff | color for text in the timer label. |
| ctvx | 0 | x-coordinate of the visual countdown timer bar. |
| ctvy | 0 | y-coordinate of the visual countdown timer bar. |
| ctvw | 0 | width of the visual countdown timer bar. |
| ctvh | 0 | height of the visual countdown timer bar. |
| ctvc | 0x555555ff | color for visual countdown timer bar. |
| survey_override | n/a | object that represent a survey card. When TAR receive a survey, it will try to override the current choice card with this object. The format of this card is same as a normal choice card. |
| xtended_view | n/a | object that represent xtended_view choice card overrides, The format of this card is same as a normal choice card. |

---

## Notes

### Layout

Choice card layout and sprite images assume a 720p canvas. TruexAdRenderer has layout logic which will automatically scale the layout specified in this configuration based on the combination of the channel manifest resolution and the device resolution. In order for this to work take care to design your layout assuming 720p. This includes using image assets designed for 720p native resolution.

### Animation

TruexAdRenderer (TAR) has hard coded button sprite animations which grow the interact or watch button by 10% when selected. This behavior applies to both static and video choice cards, and currently cannot be customized or disabled.

### Countdown Timer

The Choice Card is only shown to the user for a limited time before automatically advancing to standard (non-interactive) video ads. By default a text label is displayed at the upper-left corner that shows the remaining number of seconds until auto-advance. The fields above (`ct*`) can be used to customize the text formatting, including what string to display, its size and color, positioning, and dimensions.

Along with the text countdown, a visual bar can be enabled which animates to indicate the remaining countdown time. It is hidden by default, to enable it simply specify non-zero width and height values for `ctvw/ctvh` fields.

### Choice Card Sprite Images

Choice Cards for TruexAdRenderer (TAR) Roku placements require width and height of the button sprite images to be defined using the `ww`, `wh`, `iw` and `ih` parameters. This is to address Roku issues with accurately returning image width and height values in low end device or memory constaint scenarios. More details can be found in the following issue [CTV-2041](https://truextech.atlassian.net/browse/CTV-2041).

Sprite images include selected and unselected button states in the same image. The sprite is split down the middle vertically with the unselected state at the top, and selected state below. Here's an example of what this might look:

![example choice card sprite image](http://ctv.truex.com/docs/example_choice_card_sprite.png)

### Branded Skip Card Images

TruexAdRenderer (TAR) supports branded card images for skip cards which can be specified in the usual manner. These are returned as part of the ad response and not mastered in the present choice card JSON structure. When an override is present, it takes precedence over the skip card image (`sk`) specified here in the VAST configuration.
