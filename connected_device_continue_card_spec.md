## Single-choice Continue Card Product/Tech Spec for Roku/Innovid

#### User Experience:
When a user starts an FX Originals stream on their connected device, they will be shown a preroll ad unit (we call the 'Continue Card') that presents a single option to interact with an engagement for 30-seconds in order to watch the stream.  The Continue Card will have an auto-play background video and a single button.  There is no auto-advance or option to exit/skip.  The user must click to enter the engagement ad experience in order to continue.  The engagement ad should have the same functionality and look/feel as our choice card product--requiring 1 interaction and a time requirement.

---

#### Difference between Continue Card vs Sponsored-Stream Choice Card:
* No 'watch' sprite for normal ads choice.
* No auto-advance (the Continue Card will display until the user selects 'Interact').
* Interact button is positioned at coordinates: 177 x 202 (use only highlighted state of sprite)
* Continue Card background is a video instead of an image.  When the video ends, remain at last frame of video.
* There is no need to skip ads.
* This product has no separate voice over.  Voice over will be encoded into the background video.

_(All other functionality should be the same as Sponsored-Stream Choice Card)_

---

#### 'Resume' functionality:

When `ivc_resume=true` (or `showskipcard=true` in legacy) is passed in.  The Continue Card should go directly to the stream content.

---

The following lists and describes each query parameter that is passed to `VASTAdTagURI` in the TrueX connected device wrapper tag.

| Query Arg | Description |
| ------------- | ------------- |
| resume | When `true` resume stream without showing a choice card and all ad breaks replaced with the skip card. |
| s | The TrueX ad server URL.  Used for making tracking analytics calls. |
| p | A query args string to be appended to the ServiceURL when making TrueX analytics calls. |
| bgv | URL for choice card background video. |
| ib | URL to sprite image for 'interactive ad' button.  The sprite format is the same as choice card with a normal and highlight state.  Display only the highlight state for this product. |
| sec | The number of seconds that the native interactive ad header starts at.  Typically, 30 seconds. |
| n | Show name to be displayed in footer. |
| f | URL to a show thumbnail image to be displaced in footer. |

---
#### Example 'Continue' Card VASTAdTagURI (Delta w/ ivc_ param)

```xml
<VASTAdTagURI><![CDATA[
http://rtr.innovid.com/r1.596553a726aa23.60310056;cb=1502493217?ivc_bgv=http%3A%2F%2Fmedia.truex.com%2Fvideo_assets%2F2017-07-11%2Fe85ecda8-58a3-4f2b-bb94-8ded3e2ab91a_large.mp4&ivc_f=&ivc_ib=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-09%2F9027f274-5808-49ef-97a8-c0bc098ac662.png&ivc_n=&ivc_p=bid_info%3Dcikxt0o9ptky3tm9sa7cjtbcd1pbdq1jzlkusqk4wb5edfs9bnwup1uszhhypjh5fpjjik9oziyq18la1mcquzpwupsk9iygm6s8bwj83z09slgok5kzk9ofow9hflzrc6ir7u551ozqryqrco4k2kxdf6x9shjxxgts8u4l3c52g3ljgx977v3sxmj1yf6uby2lp0bpqtpf54cv8jmwievepshyzez6dqoiy1f3wsup0p8eviofxp7w7dqzozvik0j2lmbgt06vjjwbprl83zcsg51s1ju85ndw7e3xrtpmkc8x31mnz9wd0zulosln9w4dtidh4ay19o2y8o8worniuc4cbmxt0urihw44gvq37kytd5cuu04l9zh7bm0gyi86jpss6cq7lmsjt3kq68kwvsdwz86r4phakn2nifkeq58jwusa7328a%26campaign_id%3D10050%26creative_id%3D11866%26currency_amount%3D1%26impression_signature%3D2a4e8d7ff975aa8e083f39ebc781d5c77239b12e9f38718121e390a95c3908d0%26impression_timestamp%3D1502493217.755281%26internal_referring_source%3DBmHW0vK-STWk5DfaI2H9Dw%26ip%3D76.79.158.34%26network_user_id%3DLz9E46QMTnmdYZhY-Cbeng%26placement_hash%3Daba51de8f42fca9e5624e611f6e8a0ea3685d25d%26session_id%3DYdx0qBZtSmSCBD0aDBGyng%26stream_id%3D123%26stream_position%3Dpreroll&ivc_resume=false&ivc_s=http%3A%2F%2Fserve.truex.com&ivc_sec=30&ivc_showskipcard=false
]]></VASTAdTagURI>
```

#### Example 'Continue' Card Tag (Delta w/ ivc_ param)
[http://stash.truex.com/tests/roku/continue_card.xml](http://stash.truex.com/tests/roku/continue_card.xml)

```
http://get.truex.com/aba51de8f42fca9e5624e611f6e8a0ea3685d25d/vast/connected_device?network_user_id=ROKU_ADS_TRACKING_ID&env[]=brightscript&stream_position=preroll&native_prefix=ivc_
```
---

#### Example 'Continue' Card VASTAdTagURI (Legacy)

```xml
<VASTAdTagURI><![CDATA[
http://rtr.innovid.com/r1.594a4bfe959b03.45226825;cb=1502493250?bgv=http%3A%2F%2Fmedia.truex.com%2Fvideo_assets%2F2017-07-11%2Fe85ecda8-58a3-4f2b-bb94-8ded3e2ab91a_large.mp4&f=&ib=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-09%2F9027f274-5808-49ef-97a8-c0bc098ac662.png&n=&p=bid_info%3Dcikxt0o9ptky3tm9iqt2nai4q940h767ikvzgx7htkd871vw3xj6cym3jsy4ahushgybk55m3gp5y31obc5v14kstbklxza203njy95f2rahsqp1b7y8itfyovtfqol9gk8a7q078nnlgke8v4e1z89mdygtxbrmm5xxryg6nppevukv8a2jwbq8dvg9mx5jc84qthx8tknknvfqphxvm3yeita6aqfwwsf6cmyouwlbo52xbzypz9fkpsvt1f1wpn2996b5aunvqo0nz8nmhiabl37vro8n0u9k0dcz530mbbd1x3prhui8whuw7a6ugikjwfnt1e83867ycqfgzd8o7i8963qq5u5eo6uolizoyb3ewt7sk75kyvajid16wg2opfisv88lvuvlz92w2jl0gv1i8ke55npxyv0k36lhhd7u3ukmd347e%26campaign_id%3D10050%26creative_id%3D11866%26currency_amount%3D1%26impression_signature%3D1996f6b2ca137bb7b6abf32326e55ed3d98d61eba31b086cfae14007fb614be9%26impression_timestamp%3D1502493250.8368917%26internal_referring_source%3DE26z5w9OT9avDpfSzY4C3w%26ip%3D76.79.158.34%26network_user_id%3D2F3QbrB1RMGMBY86KRlkpw%26placement_hash%3Daba51de8f42fca9e5624e611f6e8a0ea3685d25d%26session_id%3DiFmfAegpSnWfrwBeZZMxcA%26stream_id%3D123%26stream_position%3Dpreroll&resume=false&s=http%3A%2F%2Fserve.truex.com&sec=30&showskipcard=false
]]></VASTAdTagURI>
```

#### Example 'Continue' Card Tag (Legacy)
[http://stash.truex.com/tests/roku/continue_card_legacy.xml](http://stash.truex.com/tests/roku/continue_card_legacy.xml)
```
http://get.truex.com/aba51de8f42fca9e5624e611f6e8a0ea3685d25d/vast/connected_device?network_user_id=ROKU_ADS_TRACKING_ID&env[]=brightscript&stream_position=preroll&stream_id=123&dimension_5=legacy_testing
```
