## Engagement Interaction Tracking

#### Overview
Engagement interations are used to track how users interact and progress through the engagement ad creative.  It provides valuable insight to clients who often use interactions as KPIs.

---

#### Interaction Tracking Fields

| Name | Description |
| ------------- | ------------- |
| `category` | `category` is the top level descriptor for the interaction.  All interactions should use the 9 pre-defined category names listed in the section below.  It's important to use these pre-defined categories because reporting is based on them. |
| `name` | `name` is the next level descriptor below category.  It should describe the action being taken by the interaction.  Ex. video_started. |
| `value` | `value` is an optional descriptor.  Its used to provide additional context for the interaction, such as a video name.  Or its used to record milliseconds in a timing interaction.  |
| `step` | `step` indicates the step in which the interaction took place.  Starting at 1 for the initial step of the engagement.  Over the years we've emphasized the concept of 'step' less, so this field is less of a requirement these days. |
| `user_initiated` | This flag indicates whether or not the interaction was triggered from a user input (i.e. click, tap, remote control).  We use this to calculate how many times the engagement was interacted with. |

---

#### Interaction Tracking Categories

| Category | Description |
| ------------- | ------------- |
| `timing` | This is an internal tracking category used by the ad container/renderer for all timing events within the engagement.  For example, when the engagement starts, the 'initial' is a `timing` interaction. |
| `multimedia` | This category is used for all video or audio events.  Typically tracking the progress of the multimedia asset. |
| `navigation` | This category is used for progressing through steps in an engagement.  The most common use is for going to the next step in the engagement. |
| `external_page` | This category is used for external content.  The most common use is for clicking out to an external website.  Its also used for downloading, printing, add to calendar. |
| `click` | This category is used to track all user input within an engagement.  On desktop, every mouse click is tracked.  On mobile, every tap is tracked.  On CTV, every remote control press is tracked. |
| `other` | This is the catch-all category for all interactions that don't fall into any other category.  We generally use this for non-standard tracking such as creative-specific interactions. |
| `share` | This category is for social media shares (i.e. Facebook, Twitter, and Pinterest).  This category has become less important today. |
| `data_entry` | This category is for tracking data entry such as user submitted their email, name, or phone number.  This category is also seldom used today in the age of privacy. |
| `aggregate` | This category is used for aggregating multiple interactions into one tracking event.  The `value` field is used to provide a count of the interaction.  This is seldom used. |

---

#### Standard Interactions Handled by the Ad Container/Renderers
These engagement interactions are automatically tracked by the ad container or CTV ad renderer.  They are always tracked in a consistent matter regardless of the ad creative.

| Category | Name | Value | Notes |
| ------------- | ------------- | ------------- | ------------- |
| `timing` | `initial` | n/a | blurb |
| `timing` | `total_time_spent` | milliseconds since initial | Fired when the user ends engagement with credit for any reason (eg. exit error after credit).
| `timing` | `total time spent` | milliseconds since initial | Fired when the user ends engagement without credit for any reason.
| `timing` | `true_attention_time_met` | interacted or not_interacted | Fired once 30 seconds has passed.  The ‘value’ field specifies whether or not the user has interacted with the engagement yet. |
| `timing` | `true_attention_interaction_met` | milliseconds since initial | Fired upon first interaction with the engagement. |
| `timing` | `continue_to_end` | milliseconds since initial | Fired when the user clicks "I'm Done" to end the engagement. |
| `timing` | `creative_completion` | milliseconds since initial | Fired upon getting to the last step of an engagement before `true_attention_time_met`.  We do not want users to be 'stuck' so we automatically award the completion to the user. |
| `click` | `interaction` | x,y or button name | Fire on any user input action.  Record x,y coords when possible or remote control button name for CTV. |

---

#### Standard Video Interactions
These video interactions should always be used for video tracking.

| Category | Name | Value | Notes |
| ------------- | ------------- | ------------- | ------------- |
| `multimedia` | `video_started` | video name or URL | Fired when video starts. |
| `multimedia` | `video_completed` | video name or URL | Fired when video ends. |
| `multimedia` | `video_first_quartile` | video name or URL | Fired at 25% video progress. |
| `multimedia` | `video_second_quartile` | video name or URL | Fired at 50% video progress. |
| `multimedia` | `video_third_quartile` | video name or URL | Fired at 75% video progress. |
| `multimedia` | `video_replay` | video name or URL | Fired when video starts again. |



