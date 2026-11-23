# Concronex for Claude

Find rental equipment from participating suppliers, check exact-date availability and preliminary pricing, and manage eligible reservations through Claude.

The plugin includes the `rental-booking` and `travel-booking` skills and the Concronex remote connector at `https://claude.concronex.com/mcp`. Supplier coverage and bookable options depend on the current catalogue, availability and supplier terms. Hotels and flights work only where your Concronex connection offers the travel tools; where it does not, Claude says so and rental works as before.

## Use Concronex

Install the plugin in Claude, then add its Concronex connector from the plugin's **Connectors** tab. When Claude asks for authentication settings, choose **Sign in when needed** and **Use Claude's published identity**. Public discovery needs no login. When a protected action requires access, allow the guest connection; no Concronex password is required.

Try asking:

- Find compact excavators available to hire in Perth.
- Check availability and a preliminary price for my selected rental dates.
- Prepare an exact rental offer and show the full terms before I decide.
- Show bookings made through my current Concronex connection.
- Show my booking's cancellation fee and consequences without cancelling it yet.

Preparation is not a reservation and may create an unreserved supplier draft. Review the exact item, dates, location, price, deposit and cancellation terms before explicitly confirming. Cancellation also requires a separate confirmation after its terms are shown. Concronex does not execute payments or refunds through Claude.

## Hotels and flights

Hotel and flight inventory is supplied by Nuitée Connect. Try asking:

- Find a refundable hotel in Melbourne for two adults from 12 to 15 March.
- Show the rooms, board and cancellation terms for that hotel.
- Find a return economy flight from Sydney to Auckland for two adults.
- Prepare that fare so I can review the final price and pay.
- Show my hotel and flight bookings, or the cancellation terms for one of them.

Claude can search and prepare an offer, but it cannot book or pay. Preparing re-verifies the price and conditions and books nothing. You enter traveller details, approve the exact price and pay on a Concronex checkout page through Nuitée's payment form; Nuitée is the merchant of record. Do not share card or passport details in chat. A prepared offer, a confirmed reservation and an issued airline ticket are separate states, and Claude reports which one your booking is in. Hotels and flights are always separate bookings, never a package. Cancelling needs a separate confirmation in chat after the exact cancellation terms are shown. Test (sandbox) results are labelled as test inventory and are not real offers.

## Your connection and information

Requested rental details and necessary renter contact information are sent to Concronex and the selected supplier or booking provider to fulfil the request. For hotels and flights, search details go to Concronex and Nuitée, and traveller details entered on the checkout page go to Concronex, Nuitée and the hotel or airline. Renter name/email are booking data, not account credentials. Access to a booking belongs to the guest connection that created it. Normal token renewal preserves access, but reconnecting does not recover earlier bookings. Keep your supplier reference for support.

[Website](https://concronex.com) · [Privacy](https://concronex.com/privacy) · [Terms](https://concronex.com/terms) · [Support](https://concronex.com/support)

## License

The manifest, connector configuration, rental-booking and travel-booking skills and documentation are licensed under MIT. This license excludes brand assets and the hosted Concronex service and backend. Concronex names and marks remain the property of their owners.
