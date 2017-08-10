## Connected Device Choice Card VAST Wrapper Query Parameters

The following lists and describes each query parameter that is passed to `VASTAdTagURI` in the TrueX connected device wrapper tag.

---

| Query Arg | Previous Extension Name | Description |
| ------------- | ------------- | ------------- |
| resume | n/a | When `true` resume stream without showing a choice card and all ad breaks replaced with the skip card. |
| s | TrueXServiceURL | The TrueX ad server URL.  Used for making tracking analytics calls. |
| p | TrueXServiceParams | A query args string to be appended to the ServiceURL when making TrueX analytics calls. |
| sk | SkipCardImageURL | URL of skip card image. |
| skv | n/a | URL of skip card video. |
| sec | NativeHeaderCountdownSeconds | The number of seconds that the native interactive ad header starts at.  Typically, 30 seconds. |
| bg | CCNativeBgURL | URL for choice card background image. |
| bgv | n/a | URL for choice card background video. |
| ib | CCNativeSpriteLeftButtonURL | URL to sprite image for 'interactive ad' button. |
| wb | CCNativeSpriteRightButtonURL | URL to sprite image for 'watch video ads' button. |
| n | n/a | Show name to be displayed in footer. |
| f | n/a | URL to a show thumbnail image to be displaced in footer. |
| pos | n/a | Value will be `preroll` or `midroll`--indicates sponsored stream or sponsored ad break. |

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
#### Example 'Continue' Card VASTAdTagURI

```xml
<VASTAdTagURI><![CDATA[
http://rtr.innovid.com/r1.594a4bfe959b03.45226825;cb=1502389104?ivc_bgv=http%3A%2F%2Fmedia.truex.com%2Fvideo_assets%2F2017-07-11%2Fe85ecda8-58a3-4f2b-bb94-8ded3e2ab91a_large.mp4&ivc_f=&ivc_ib=http%3A%2F%2Fmedia.truex.com%2Fimage_assets%2F2017-08-09%2F9027f274-5808-49ef-97a8-c0bc098ac662.png&ivc_n=&ivc_p=bid_info%3Dcikxt0o9ptky3tm9r9whwjjydpyu7aptzd90td4iga32p8qp76xgzwfyxvvk6t9f5j76tvtsnn4isxu4w4pqqdmdxnphzqfjoebq1cm68t7jrbx0y3338v1z252dwbcrnjhudguo7c93xj6nmapoed01ptn1bmivymqcc7jbky468l6dm920ukjjoe31a7h7r87vpgzesdi4i49bw03jd0svht6srl35lcye7pv6fqo8db4lt1l5mav8i259b9gohnbo271ecr563ziedqeldw9495v4bffioduhmf5jc63ra6l9i6t0yhlgrx3elbccd8g8ejb8gpsf1kiz7sj6gqtrsmjqn8k7oj98c6u1zoykmqkg7mfgujkq2tpue4y571ma37r9ycj9hbew9h19wo8e1l30u2g6kddrj6ef9p9dhum20ijb6aux6%26campaign_id%3D10050%26creative_id%3D11866%26currency_amount%3D1%26impression_signature%3D78204b72c94a5b69cca48d511e314a6fb21f1396f16514cbcad75bedfb0b011d%26impression_timestamp%3D1502389104.202253%26internal_referring_source%3D1AKY_dDRRUKSbbIKvqdV0Q%26ip%3D76.79.158.34%26network_user_id%3DxnUIMmLpQYGqg1RwPLuB-A%26placement_hash%3Daba51de8f42fca9e5624e611f6e8a0ea3685d25d%26session_id%3Dgn3StSoqQs67QDb96GX1sQ%26stream_id%3D123&ivc_pos=preroll&ivc_resume=false&ivc_s=http%3A%2F%2Fserve.truex.com&ivc_sec=30&ivc_showskipcard=false&ivc_skv=http%3A%2F%2Fmedia.truex.com%2Fvideo_assets%2F2017-08-04%2F303e7139-c3c5-4772-9082-1198196b942f_large.mp4&ivc_wb=
]]></VASTAdTagURI>
```

#### Example 'Continue' Card Tag
[http://stash.truex.com/tests/roku/continue_card.xml](http://stash.truex.com/tests/roku/continue_card.xml)
