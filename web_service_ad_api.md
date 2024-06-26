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

## Using a WSAPI response with the Javascript Client
Should you want to get the benefits of client-side callbacks and event handling while checking for available activities using the Web Service API, you can do so.

Include the javascript client on your page, and use the `truex.client.prepareActivity` like so:

```html
<div id='tx_container'></div>

<script type='text/javascript' src='https://static.truex.com/js/client.js'></script>

<script>
	// activity is JSON returned from WSAPI
	var openActivityFromWSAPI = function(activity) { 
		truex.client(options, function(client) {

			// prepareActivity adds the event handler hooks
			client.prepareActivity(activity);

			// you can now listen for these events
			activity.onStart(function(activity){
				alert(activity.id + ' started!')
			});
			
			activity.onClose(function(activity){
				alert(activity.id + ' closed!')
			});

			activity.onCredit(function(engagement){
				alert(engagement.signature + ' credit!')
			});

			activity.onFinish(function(activity){
				alert(activity.id + ' onFinish!')
			});

			// use the loadActivityIntoContainer to load the ad unit in a div on your page
			client.loadActivityIntoContainer(activity, document.getElementById(‘tx_container’));
		});
	};
</script>
```

## Server-side Callback Specification (optional)
Most partners use client-side callbacks to know when a user has completed an engagement.  This is the recommended callback strategy, and should be sufficient for most use cases.  However, in some configurations, a client-side callback is not possible.  In such configurations, a server-side callback is useful.

In order to leverage server-side callbacks, the partner supplies a callback URL to true[X] during the integration phase.  Upon a user successfully completing an engagement, true[X] will make a request to the partner's callback URL with information about the engagement.  This callback generally happens in real time, but may be delayed depending on current load.  The callback will include the following parameters:
| Parameter | Type | Description |
| ------------- | ------------- | ------------- |
| `application_key` | string | The partner-specific application key provided by true[X]. |
| `network_user_id` | string | The unique, partner-provided user identifier for the user who completed the ad. |
| `currency_amount` | int | The amount of currency earned by completing the ad. |
| `currency_label` | string | The label of the currency used (e.g. "coins"). |
| `revenue` | decimal | The amount of revenue earned by the partner for this engagement. Values can have up to 8 decimal places. |
| `placement_hash` | string | The identifier hash of the placement from which this engagement originated. |
| `campaign_name` | string | The name of the campaign completed in this engagement. |
| `campaign_id` | string | The unique identifier of the campaign completed in this engagement. |
| `creative_name` | string | The name of the creative completed in this engagement. |
| `creative_id` | string | The unique identifier of the creative completed in this engagement. |
| `engagement_id` | string | The unique, true[X]-generated identifier for this engagement.  If a non-unique engagement_id is passed to the partner, the request should be ignored and return a failure code (see below) to avoid over-crediting a user. |
| `sig` | string | The signature of the signed request.  See the [Callback Signing](#callback-signing) section below for details. |

The partner should ensure that the callback URL provides the following response codes:
- 0 – Recoverable failure (request will be retried)
- 1 – Callback successfully processed
- 2 – Invalid signature (this will notify true[X] for investigation)
- 3 – Invalid user or duplicate engagement_id (request will not be retried)

### Callback Signing
In order to protect both true[X] and its publishers from request forgery, all engagement callbacks will be signed using the following algorithm:
1. Put parameters in an array of `key=value` pairs.
2. Order array of parameter pairs in alphabetical order by parameter key.
3. Concatenate parameter pairs into a string.
4. Append the partner-specific `application_secret` provided by true[X] to the string.
5. Calculate a keyed-hash message authentication code (HMAC-SHA1) signature using the partner-specific `application_secret`.
6. URL escape the signature before using it as a query argument.

Sample signature generation using PHP:
```php
<?php
$application_secret = 'XXXXXXXXXXXX';
$args = array(
  'application_key' => 'XXXXXXXXXXXXXXXX',
  'network_user_id' => 'XXXXXXXXXX',
  'currency_amount' => 'XXX',
  'currency_label' => 'XXXX',
  'revenue' => 'XXX.XXXXXXXX',
  'placement_hash' => 'XXXXXXXXXX',
  'creative_name' => 'XXXXXXXXX',
  'creative_id' => 'XXXXXXXXX',
  'engagement_id' => 'XXX'); // Arguments used in the request
ksort($args); // Sort arguments alphabetically

$base_str = '';
foreach ($args as $key => $value) {
  $base_str .= $key . '=' . $value; // Note: there is no separator
}
$base_str .= $secret;
$sig = base64_encode(hash_hmac('sha1', $base_str, $secret, true));
?>
```