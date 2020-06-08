# true[X] Web Service API

In addition to the [JS API](https://github.com/socialvibe/truex-ads-docs/blob/master/js_ad_api.md), a web service API is available.  The use case for this API is primarily for server-side implementations.

## Endpoint

Production:
```
https://get.truex.com/v2
```
QA:
```
https://qa-get.truex.com/v2
```

## `GET` Parameters
| Parameter | Required | Type | Description |
| ------------- | ------------- | ------------- | ------------- |
| `user.id` | ✓ | string | A partner-provided alphanumeric string uniquely identifying the current user. (If supported, Advertising ID is recommended). |
| `placement.key` | ✓ | string | The placement-specific identifier provided by true[X]. |
| `ip` | ✓ | string | The IP address of the user; used for geo-targeting. Required for server-side calls. |
| `user_agent` | ✓ | string | Device User Agent string. Required for server-side calls. This value should be URI encoded. |
| `referrer` |  | string | The publisher URL that the user is currently visiting when the true[X] call to action is shown. This value should be URI encoded. |
| `age` |  | int | The age of the current user.  Used for targeting. |
| `yob` |  | int | Year of birth of user as 4 digit integer.  Can be provided instead of `age`. |
| `gender` |  | string | The user's gender, represented as 'm' for male, 'f' for female, or 'x' for non-binary.  Used for targeting. |
| `dimension_1` |  | string | Slot 1 for targetable metadata. |
| `dimension_2` |  | string | Slot 2 for targetable metadata. |
| `dimension_3` |  | string | Slot 3 for targetable metadata. |
| `dimension_4` |  | string | Slot 4 for targetable metadata. |
| `dimension_5` |  | string | Slot 5 for targetable metadata. |
| `coppa` |  | boolean | '1' or '0'.  Pass '1' if the user is under 13.  Prevents data storing. |
| `mraid` |  | boolean | (mobile only) '1' or '0'. When true, adds mraid.js to the ad markup. |

#### Sample Request
```
https://get.truex.com/v2?placement.key=c0b37fa4bd62dd79046e392aa4f6f005aae4f2e5&user.id=tester123
```

## Response
A `activity` JSON object.  Or `{"error": "No bid found"}` if no ads are available.
#### Format: `JSON`
| Field | Description |
| ------------- | ------------- |
| `id` | The unique identifier of the ad creative, defined by true[X]. |
| `campaign_id` | The unique identifier of the ad campaign, defined by true[X]. |
| `name` | The name of the ad creative, defined by true[X]. |
| `window_url` | The URL that renders the ad. |
| `window_width` | The pixel width of the ad.  Any container holding the ad should be sized to this. |
| `window_height` | The pixel height of the ad.  Any container holding the ad should be sized to this. |
| `currency_amount` | The amount of publisher currency that the user will earn for completing this ad.  Example: 2 Coins. |
| `revenue_amount` | The revenue to be earned by the partner for this ad. |
| `session_id` | A unique identifier for the current session, assigned by true[X]. |
#### Sample Response
```json
{
	"id": 7633,
	"campaign_id": 3802,
	"name": "Kung Fu Panda",
	"window_url": "https://qa-media.truex.com/...",
	"window_width": 960,
	"window_height": 500,
	"currency_amount": 2,
	"revenue_amount": "0.1",
	"session_id": "i_wktnzfS1G6yqdp57lSIw"
}
```



