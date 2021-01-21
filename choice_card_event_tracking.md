## Choice Card Event Tracking

#### Overview
This document describes our naming convention when tracking choice card events.  These events are fired from our FEP trigger point, which we call the Choice Card.  The main objective of these events is to diagnose issues and optimize performance of our choice cards.  Like all tracking at true[X], the tracking event is composed of 3 main fields: `category`, `name`, `value`.

---

#### `category` naming convention
The `category` for choice card tracking events is constructed by concatenating 3 naming elements.  All choice card events start with our base category name `fep`.  The second part represents the platform.  For example, on mobile we append `_mobile`.  Lastly, the 3rd and final component of the `category` is the choice card config name, which is obtained from the choice_card_config.  A full example is: `fep_roku_a_and_e_lifetime_ss`. 

---

#### Required Events
These are required by all implementations of our choice card.  They are critical for tracking basic performance.

| Name | Value | Notes |
| ------------- | ------------- | ------------- |
|`player_load` | n/a | Fired when the choice card is first loaded and an engagement ad is available, meaning the user is being shown the choice card.  |
|`player_no_load` | n/a | Fired when the choice card is first loaded and NO engagement ad is available so the user was NOT shown a choice card.  Note: this is only tracked with 2-stage tag implementations. |
|`player_ad_free` | n/a | Fired when the choice card is first loaded and the user is in an ad-free state (i.e. sponsored stream flow) so a Skip Card is shown. |
|`choice_card_select_unit` | n/a | Fired the first time a user chooses to interact for 30 seconds.  This event indicates the engagement ad is about to load. |
|`choice_card_select_watch` | n/a | Fired when a user chooses to watch the normal commercial break. |
|`choice_card_auto_advance` | n/a | Fired when the auto-advance timer expires.  This means the user did not make a choice. |
|`choice_card_re-select_unit` | n/a | Fired on subsequent clicks to interact with the ad.  This scenario happens if the user first views the engagement but closes it before True Attention, then tries to interact again. |

---

#### Optional Events
These events are optionals and are mainly used for debugging and error tracking purposes.

| Name | Value | Notes |
| ------------- | ------------- | ------------- |
|`choice_card_debug` | [debug info] | Fired to send debugging info for analysis.  |
|`choice_card_error` | [error message] | Fired to send error messages. |
|`unit_closed_by_x_after_credit` | n/a | Fired when the user closes the engagement AFTER True Attention. |
|`unit_closed_by_x_before_credit` | n/a | Fired when the user closes the engagement BEFORE True Attention. |
|`unit_load` | n/a | Fired when the engagement ad first loads. |
|`unit_credit` | n/a | Fired when the user earns their credit for True Attention. |
|`unit_first_interaction` | n/a | Fired upon the first interaction with the engagement ad. |
|`unit_time_met` | n/a | Fired upon reaching 30 seconds with the engagement ad. |


