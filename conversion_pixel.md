TrueX Conversion Pixel
==================================================
The TrueX conversion pixel is a URL we can provide to clients to track actions the user takes on the client's website.  For example, if a client wants to track how many users who saw their TrueX engagement ad and then made a purchase, they could place the TrueX conversion pixel on their shopping cart confirmation page.  Please note the conversion pixel usages browser cookies to track and therefore is only supported for desktop campaigns.

The Pixel:
-----------
```
https://engage.truex.com/c.gif?value=LABEL
```

Details:
-----------
- The pixel above should be provided to clients, replacing `LABEL` with something that is descriptive for the campaign and action being taken.  For example: https://engage.truex.com/c.gif?value=BLUE_BUFFALO_CAT_FOOD_PURCHASE
- The pixel works by dropping a cookie when a user completes an engagement.  If the user then goes off to a website where the conversion pixel is fired, we will record a tracking event.
- Conversion events are recorded in the `tracking_events` table.  Daniel Chon has created a Tableau dashboard for these events.

Technical Notes:
-----------
- Safari blocks 3rd-party cross-site cookies and therefore our conversion pixel does not work on Safari.  Usage of Safari is low on desktop but is high in mobile.  Therefore, the conversion pixel should not be used on mobile campaigns.
