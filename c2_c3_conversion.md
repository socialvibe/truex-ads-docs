It is understood that the current and planned approach for mobile and desktop ads is to use the pure JS-code approach. In this case it is expected that no changes to the code need to be done, since the full UX is completely controlled by the JS-code only, and there are no elements described in the LayoutJSON section.

However, for those ads that make use the LayoutJSON elements, the following notes apply when migrating from C2 to C3 behavior:
* Text elements are now just `<div>` items, no longer are `<input>` items, so JS code needs to refer to `elmt.innerText` instead of `elmt.value`
* Video elements are now technically `<div>` items wrapping a `<video>` item, lazy loading the video only when it actually needs to play. While the wrapper div has mostly the same JS api as a video item (e.g. `play()` and `pause()` methods, event listener support for `play`, `pause`, `ended`, `timeupdate`), there can be subtle differences.
  * The ideal approach is to simply declare a video and let built-in container support for autoplay, replay, and rotating videos just do its work.
  * The most complex scripting needed should ideally be to just invoke `play()` as needed, perhaps in response to another button click.
  * NOTE: for existing JS-based ads that create their `<video>` elements directly, they should continue to work unchanged. If they have their own click-for-sound/replay buttons, they will continue to work.
* Remove autoplay detection and UX for “Video” elements declared via LayoutJSON, that will be handled by the container itself.
  * For ads that contain a play icon to support tap/click to play, that code should be removed, the `.play-icon` in the CSS, and any JS code references to the play icon DOM element.
  * NOTE: for existing JS-based ads that create their `<video>` elements directly, they should continue to implement their own click-for-sound/replay buttons.
* The video element now supports these properties in the LayoutJSON to help control this autoplay behaviour:
  * `allowClickToPlay` (default `false`). If `true` autoplay failures cause a click-to-play button to be shown to allow the user to manually invoke playback. If `false` then the video is instead muted and replayed, which should cause the click-for-sound icon to be shown.
  * `allowClickForSound` (default `true`). If `true` muted video playback causes a click-for-sound icon to be shown, to allow the user to unmute the video. If `false`, muted video playback will simply continue.
    * Muted videos typically are allowed to auto play even on pages that have had no user interaction.
  * To support the scenario of muted background videos, simply set both `allowClickToPlay` and `allowClickForSound` to false for the video in question.
* Videos should be either declared in the LayoutJSON or else created via JS code via `TXM.layout.createVideoElement(id, srcOrConfig)`
  * The use of raw HTML5 \<video> DOM instances is discouraged as this avoids autoplay and tracking support.
  * The `config` parameter should have the same fields as that of the "Video" elements in the LayoutJSON.
* Steps in the LayoutJSON are distinct in C3, to improve runtime loading and memory performance. However, for C2 ads steps are used more for grouping purposes, in particular for tracking, and are their DOM elements are assumed to all be loaded when the ad is first loaded. This will continue to be supported, but with some improvements.
  * Note in particular the implicit `alwaysLoaded` and `alwaysVisible` properties for steps in the LayoutJSON to control this. These will default to `true` for mobile and desktop ads.
  * This kind of ad code that tries to combine steps via JS should no longer be needed, steps for mobile and desktop ads will do the right thing be default. On the other hand, executing it will have no effect either, since the `offstage` css class will already not be present, as all steps will already be visible for mobile and desktop ads.
    ```javascript
    setTimeout(function() {
        $('#ad_stage .offstage').removeClass('offstage');
        for (var i = 0; i < 10; i++) {
            $('#ad_stage .step_'+ i).removeClass('step_'+ i);
        }
    }, 0);
    ```
* Bluescript ads are now (incidentally!) supported for mobile and desktop C3 ads, so that is an option if one wants to create ads similar to CTV ads.