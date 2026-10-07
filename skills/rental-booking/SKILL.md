---
name: rental-booking
description: Find rental equipment through Concronex, check availability and prices, prepare exact rental offers, and manage eligible reservations and cancellations. Use when the user wants to hire equipment or view or cancel a booking made through their current Concronex connection.
---

# Rental booking with Concronex

Use the Concronex connector for supplier information and rental actions. Its returned capabilities, eligibility, terms and errors determine what is possible. Use only the tools needed for the user's request.

## Find and prepare

- Establish the equipment, quantity, location, pickup and return times, including timezone. Ask for missing details that affect the rental.
- Discover relevant offerings with `search_offerings`. Use `get_capabilities` or `get_supplier` when their information is needed. Catalogue results do not promise availability: check the exact dates with `check_availability` and use `get_quote` for preliminary pricing.
- Only when the user wants to proceed, collect the required renter details and call `prepare_booking`. Explain that preparation is not a reservation; it may create an unreserved supplier draft. A preliminary quote is not the exact booking offer.
- Present the returned item, quantity, dates/timezone, location, currency and total, taxes/fees, amount due now, amount due on collection, deposit, material supplier/cancellation terms, and offer expiry. Present terms accurately; clarify material ambiguity rather than dismissing it.

## Reserve after confirmation

- Wait for fresh, explicit approval of that exact offer. General intent, an earlier approval, silence or a tool-permission click is not approval of rental terms. Respect refusal and stop without calling `confirm_booking`.
- Use only the returned exact offer identifiers/hash when calling `confirm_booking` after approval. If terms change or the offer expires, present the new offer and obtain fresh approval. Never substitute your own terms or bypass a server refusal.
- If the result is uncertain, use available intent/booking readback before attempting another mutation; do not create a second reservation to resolve uncertainty.
- Report the actual final status, supplier booking reference, collection/handoff instructions and any outstanding action. Preserve the reference for the user. Use `get_booking` or `list_my_bookings` as needed to verify an owned reservation.

## Cancel after separate confirmation

- Identify the intended owned booking. Call `prepare_cancellation` to obtain the exact cancellation offer; preparation does not cancel it.
- Show the returned cancellation fee, refund consequence, terms and expiry. Require a separate fresh confirmation of that cancellation offer before calling `cancel_booking` with its exact identifiers/hash.
- Changed or expired cancellation terms require renewed approval. Read back the final booking state and report only the verified outcome.

## Connection and payment boundaries

Public discovery does not require a guest connection. Protected actions may ask the user to allow Concronex access. Renter name/email are booking data, never ownership or recovery credentials. Normal renewal retains ownership; disconnecting/reconnecting cannot reclaim earlier guest bookings. Retain supplier references and direct recovery questions to Concronex support.

Concronex does not execute payments or refunds through this plugin. Do not invent payment tools, collect payment credentials, or equate an unpaid reservation with payment. Follow any supplier handoff and report amounts and payment status exactly as returned.
