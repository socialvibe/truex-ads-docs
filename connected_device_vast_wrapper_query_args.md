## Connected Device Choice Card VAST Wrapper Query Parameters

The following lists and describes each query parameter that is passed to `VASTAdTagURI` in the TrueX connected device wrapper tag.  **All pixels calculations are based on 720p.**

---

| Query Arg | Previous Extension Name | Description |
| ------------- | ------------- | ------------- |
| resume | n/a | When `true` resume stream without showing a choice card and all ad breaks replaced with the skip card. |
| s | TrueXServiceURL | The TrueX ad server URL.  Used for making tracking analytics calls. |
| p | TrueXServiceParams | A query args string to be appended to the ServiceURL when making TrueX analytics calls. |
| sk | SkipCardImageURL | URL of skip card image or mp4. |
| sec | NativeHeaderCountdownSeconds | The number of seconds that the native interactive ad header starts at.  Typically, 30 seconds. |
| bg | CCNativeBgURL | URL for choice card background image. |
| bgv | n/a | URL for choice card background video. |
| ib | CCNativeSpriteLeftButtonURL | URL to sprite image for 'interactive ad' button. |
| ix | n/a | x-coordinate of interact button. |
| iy | n/a | y-coordinate of interact button. |
| wb | CCNativeSpriteRightButtonURL | URL to sprite image for 'watch video ads' button. |
| wx | n/a | x-coordinate of watch button. |
| wy | n/a | y-coordinate of watch button. |
| bd | n/a | a value that specifies the number of seconds to delay when all buttons should be shown.  If not specified, there should be no delay. |
| cx | n/a | x-coordinate of countdown timer.  Based on 720p. |
| cy | n/a | y-coordinate of countdown timer.  Based on 720p. |
| cw | n/a | width of countdown timer.  Based on 720p. |
| ch | n/a | height of countdown timer.  Based on 720p. |
| cfs | n/a | font size of countdown timer. |
| n | n/a | Show name to be displayed in footer. |
| f | n/a | URL to a show thumbnail image to be displaced in footer. |
| pos | n/a | Value will be `preroll` or `midroll`--indicates sponsored stream or sponsored ad break. |
| aa | n/a | Number of seconds for auto advance timer.  Typically, 30 seconds. -1 for no auto advance.|
| vo | n/a | URL for choice card voice over audio file. |
| rdp | n/a | URL for StreamPass redeem pixel. |
| product | n/a | Value will be `sponsored_stream`, `sponsored_ad_break`, `continue`, or `stream_pass`--indicates the product type.  If not specified, please default to `sponsored_stream` when pos=`preroll` and default to `sponsored_ad_break` when pos=`midroll`. |
| name | n/a | A descriptive name for the choice card config used for a/b testing.  The `name` gets appended to the tracking categroy. |
| survey_config_url | n/a | URL to JSON config object of survey parameters.  [Example](http://stash.truex.com/tests/innovid/survey_config.json)  |

---
#### Example Sponsored Stream VASTAdTagURI

```xml
<VASTAdTagURI><![CDATA[
     http://rtr.innovid.com/r1.5852a9001edca0.98642229;cb=1502389311?bg=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-07%2Fb88115f3-926d-4602-9420-8b194296d335.png&f=&ib=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-07%2Fc0d17205-17cd-4bdf-8eb6-672633fd8eef.png&n=&p=bid_info%3Dcikxt0o9ptky3tm9iq4eh4ajvr9sj1e1u8e5bv0qight1kgnv7mflb1mbozo15adx0h7aay7w0ut2vdewgciel95ij6bxowdz5xfa0cb9uodw90fwlevakf70oy22ng5afsogcy1g4q3vvwyppw18xrurywli5bapdnfylzztou3tyr6x5tnjfkxyf8ybbxdenk3r5l40mvn1nsskjwss99fksrhj1o793lcahn153myqab6lbhk3tzlujyt6xlz3stmqg8turnjfum7zmxr6wso1dsfp35xe10nzq68fwdg0jkcl4tuteu4xql6jam2en6zwezl7nvmcd7g2hohbtpl23umg5lcqebawwcl1cmn36mtqo2n7fitogrhvve5mou2o5jfpw1kdo2h1di8mccwekaz3f4roptzg4d31z0jl3kb72prm2vbe%26campaign_id%3D8905%26creative_id%3D10840%26currency_amount%3D1%26impression_signature%3Df9c30a4bfc5b5f52e6cfda6b50a25b15348906c9dc1ee6c9a483ada0de03f585%26impression_timestamp%3D1502389311.7728536%26internal_referring_source%3DGx2I8puTR-CoHXvUNwoKDQ%26ip%3D76.79.158.34%26network_user_id%3DQF32P4QbSLaeYN2a0yj4rw%26placement_hash%3D1e8b5e4fb1b62451b8ce2cdfd76b5c598566f215%26session_id%3DVafbe652QkyEeasoFYUyBA%26stream_id%3D123&pos=preroll&resume=false&s=http%3A%2F%2Fserve.truex.com&sec=30&showskipcard=false&sk=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-07%2Fbf25e3c1-2d7e-4cad-8f25-8f41ece89788.png&wb=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-07%2Fc0bcbcc3-4c3e-4b65-90b3-56b6f8b7afac.png
]]></VASTAdTagURI>
```

#### Example Sponsored Stream Tag
[http://stash.truex.com/tests/roku/sponsored_stream.xml](http://stash.truex.com/tests/roku/sponsored_stream.xml)


---
#### Example Sponsored Ad-Break VASTAdTagURI

```xml
<VASTAdTagURI><![CDATA[
     http://rtr.innovid.com/r1.59847aafacc329.60662603;cb=1502389526?bg=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-06-23%2Fc0f060e8-fbfc-4827-9d54-a8d90ba2633b.jpg&f=&ib=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Finnovid%2Froku2%2Fmr_interact_btn_sprite.png&n=&p=bid_info%3Dcikxt0o9ptky3tm9ih4w1ev4wa9jpqxc6u45mby5pybvjyp7hfwvyczy1ff2pjxeh4csiml850dwtpomp6oh1cw7jnhba53qyickoikav99kxrml9i3z2y9wq1eekuayz9qrmlbkjuex97c7zb0kou68ro44wnj0dccsbf0d1mony73qa3f9oek0n7uc0o7093gvmo194wt23imw5oi8zvpnlpse0aasd1ou3dbqw410mcd23r7i0bf012rnzhktxxq3tefqgsf8t8ovq1u1yrsdac10bc7kconr4fliebh16yoyn4gti1zz622nhcz2z8ndcjrkfcjfuy93xrkl782tjehrcvx5vicatree1v1gqz9wot7m6txyhbcusim3t9015bze59ime6jab10unz8rw2nbfffj4yno34t5cxzvpgxomcfpezsi2%26campaign_id%3D9687%26creative_id%3D11622%26currency_amount%3D1%26impression_signature%3Df465e7953cd61116e5e98bd01b18fa5c807b3e7cb4cf86c58829060a75fc4a23%26impression_timestamp%3D1502389526.6020741%26internal_referring_source%3DOFGBaBsiS8OVXDDD2QqWCA%26ip%3D76.79.158.34%26network_user_id%3DiqyC8vFGSJSsO3gDdJcSpw%26placement_hash%3D4203b25e9b01c1bbf8ce175880efcdae3d862a27%26session_id%3DdH16kdCyQ1qy8B28iuY9kw%26stream_id%3D123&pos=midroll&resume=false&s=http%3A%2F%2Fserve.truex.com&sec=30&showskipcard=false&sk=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Finnovid%2Froku2%2Ffxnow_roku_skip_card.png&wb=http%3A%2F%2Fmedia.truex.com%2Fm%2Fpartners%2Finnovid%2Froku2%2Fmr_watch_btn_sprite.png
]]></VASTAdTagURI>
```

#### Example Sponsored Ad-Break Tag
[http://stash.truex.com/tests/roku/sponsored_ad_break.xml](http://stash.truex.com/tests/roku/sponsored_ad_break.xml)

---

For 'Continue' Card product, please see [https://github.com/socialvibe/truex-ads-docs/blob/master/connected_device_continue_card_spec.md](https://github.com/socialvibe/truex-ads-docs/blob/master/connected_device_continue_card_spec.md)
