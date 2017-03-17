## Connected Device Choice Card VAST Wrapper Query Parameters

The following lists and describes each query parameter that is passed to `VASTAdTagURI` in the TrueX connected device wrapper tag.

---

| Query Arg | Previous Extension Name | Description |
| ------------- | ------------- | ------------- |
| resume | n/a | When `true` resume stream without showing a choice card and all ad breaks replaced with the skip card. |
| s | TrueXServiceURL | The TrueX ad server URL.  Used for making tracking analytics calls. |
| p | TrueXServiceParams | A query args string to be appended to the ServiceURL when making TrueX analytics calls. |
| sk | SkipCardImageURL | URL of skip card image. |
| sec | NativeHeaderCountdownSeconds | The number of seconds that the native interactive ad header starts at.  Typically, 30 seconds. |
| bg | CCNativeBgURL | URL for choice card background image. |
| ib | CCNativeSpriteLeftButtonURL | URL to sprite image for 'interactive ad' button. |
| wb | CCNativeSpriteRightButtonURL | URL to sprite image for 'watch video ads' button. |
| n | n/a | Show name to be displayed in footer. |
| f | n/a | URL to a show thumbnail image to be displaced in footer. |
| pos | n/a | Value will be `preroll` or `midroll`--indicates sponsored stream or sponsored ad break. |

---
#### Example VASTAdTagURI

```xml
<VASTAdTagURI><![CDATA[
    http://rtr.innovid.com/r1.5851e9789b9101.35517103;cb=2342342342?resume=false&s=http%3A%2F%2Fserve.truex.com&p=campaign_id%3D8590%26creative_id%3D10740&sk=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Fbrightline%2Ffxnow_roku_skip_card.png&sec=30&bg=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Fbrightline%2Ffxnow%2Fnative_bg.png&ib=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Fbrightline%2Ffxnow%2Fnative_left_btn.png&wb=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Fbrightline%2Ffxnow%2Fnative_right_btn.png&n=Rosewood&f=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-03-16%2Fa2779f90-b965-42c5-966d-8b9dfdc8b2a4.png&pos=preroll
]]></VASTAdTagURI>
```
---
#### Example Tag
[http://stash.truex.com/tests/roku/connected_device.xml](http://stash.truex.com/tests/roku/connected_device.xml)
