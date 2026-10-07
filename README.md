# Concronex for Claude

Find rental equipment from participating suppliers, check exact-date availability and preliminary pricing, and manage eligible reservations through Claude.

The plugin includes the `rental-booking` skill and the Concronex remote connector at `https://claude.concronex.com/mcp`. Supplier coverage and bookable options depend on the current catalogue, availability and supplier terms.

## Use Concronex

Install the plugin in Claude, then add its Concronex connector from the plugin's **Connectors** tab. When Claude asks for authentication settings, choose **Sign in when needed** and **Use Claude's published identity**. Public discovery needs no login. When a protected action requires access, allow the guest connection; no Concronex password is required.

Try asking:

- Find compact excavators available to hire in Perth.
- Check availability and a preliminary price for my selected rental dates.
- Prepare an exact rental offer and show the full terms before I decide.
- Show bookings made through my current Concronex connection.
- Show my booking's cancellation fee and consequences without cancelling it yet.

Preparation is not a reservation and may create an unreserved supplier draft. Review the exact item, dates, location, price, deposit and cancellation terms before explicitly confirming. Cancellation also requires a separate confirmation after its terms are shown. Concronex does not execute payments or refunds through Claude.

## Your connection and information

Requested rental details and necessary renter contact information are sent to Concronex and the selected supplier or booking provider to fulfil the request. Renter name/email are booking data, not account credentials. Access to a booking belongs to the guest connection that created it. Normal token renewal preserves access, but reconnecting does not recover earlier bookings. Keep your supplier reference for support.

[Website](https://concronex.com) · [Privacy](https://concronex.com/privacy) · [Terms](https://concronex.com/terms) · [Support](https://concronex.com/support)

## License

The manifest, connector configuration, rental-booking skill and documentation are licensed under MIT. This license excludes brand assets and the hosted Concronex service and backend. Concronex names and marks remain the property of their owners.
