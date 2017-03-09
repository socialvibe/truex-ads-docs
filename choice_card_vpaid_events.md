## Description of TrueX VPAID AdInteraction Events

The following lists and describes all AdInteraction events fired from the TrueX choice card ad flow.

| AdInteraction Event  | Description |
| ------------- | ------------- | ------------- |
| TrueXChoiceCardLoaded  | Dispatched when the TrueX choice card is shown to the user.  This only fires if an ad is available.  |
| TrueXSkipCardLoaded  | Dispatched when the TrueX skip card (aka 'Thank You' card) is shown to the user.  |
| TrueXNoAdLoaded  | Dispatched when the TrueX choice card would have been shown, but no ad was available so the choice card is skipped.  |
| TrueXUserOptIn  | Dispatched when the user chooses to interact with a TrueX engagement ad.  |
| TrueXUserOptOut  | Dispatched when the user chooses to watch normal commercial breaks instead of interacting with a TrueX engagement ad.  |
| TrueXUserTimeOut  | Dispatched if the user does not make an ad choice in 15 seconds.  The choice card auto-advances to normal commercial breaks.  |
| TrueXClosedAfterCredit  | Dispatched when the user closes the TrueX engagement ad after they have been credited.  Crediting occurs when the user spends the required amount of time (typically 30 or 60 seconds) and interacts at least once with the ad.  |
| TrueXClosedBeforeCredit  | Dispatched when the user closes the TrueX engagement ad before being credited.  |
| TrueXCredit  | Dispatched when the user has satisfied the TrueX engagement ad requirement--time spent (typically 30 or 60 seconds) and one interaction with the engagement ad.  This event signifies that the user has successfully earned their user benefit.  For example, removal of ads from the rest of the ad break or entire stream.  |
