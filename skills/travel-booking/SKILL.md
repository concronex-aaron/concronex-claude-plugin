---
name: travel-booking
description: Search hotels and flights through Concronex, prepare an exact re-verified offer, hand the user a Concronex checkout link to approve and pay, and check or cancel their hotel and flight bookings. Use when the user wants a hotel stay or a flight, or asks about a travel booking made through their current Concronex connection. Not for equipment rental.
---

# Hotels and flights with Concronex

Concronex travel tools search hotel and flight inventory supplied by Nuitée Connect, prepare an exact offer, and return a checkout link. The connector's returned data, `next` hints, states and errors decide what is possible. Use only the tools the request needs.

## Before you start

- The travel tools are `search_hotels`, `get_hotel_offers`, `search_flights`, `prepare_travel_booking`, `get_travel_booking`, `list_travel_bookings`, `prepare_travel_cancellation` and `cancel_travel_booking`. If they are not available on this connection, say that hotels and flights are not available through this Concronex connection yet. Never use the rental tools (`search_offerings`, `prepare_booking`, `confirm_booking`, `cancel_booking`, `get_booking`, `list_my_bookings`) for travel, and never pass a travel `bookingRef` to them.
- Searching (`search_hotels`, `search_flights`) and reading offers (`get_hotel_offers`) need no Concronex connection, so search first and do not ask the user to connect before showing prices. Preparing, viewing and cancelling bookings need the user's Concronex connection. If a tool returns `UNAUTHORIZED` or `INSUFFICIENT_SCOPE`, ask the user to allow or reconnect Concronex access, then retry. An `offerRef` found before connecting still works after connecting.
- `TRAVEL_CHANNEL_DISABLED`, `TRAVEL_BOOKINGS_DISABLED`, `TRAVEL_HOTELS_DISABLED` or `TRAVEL_FLIGHTS_DISABLED` means that part of travel is switched off. Say so plainly. Search may still work when booking is off; do not offer a booking path that was refused.

## You cannot book or pay

- You search and prepare. The user enters traveller details, explicitly approves the exact price, and pays on the Concronex checkout page through Nuitée's payment form. Nuitée is the merchant of record.
- Do not ask for or accept card details, passport or ID numbers, dates of birth or other traveller details in chat. They are entered only on the checkout page. If the user offers them, tell them to enter them on the checkout page instead.
- Never say a hotel or flight is booked, reserved, paid or ticketed until `get_travel_booking` returns that state.

## Test inventory

Every travel result carries `environment`. When it is `sandbox` (`testInventory: true`), results are simulated test inventory: prices and availability are not real offers, and no real reservation, ticket or charge results. Say this each time you present sandbox results, offers or bookings, and never describe them as live. Use the returned `notice` wording where it helps.

## Hotels

1. **Gather the stay.** Destination (prefer city plus ISO country code, otherwise a landmark or district, or coordinates), check-in and check-out dates, and the rooms with the adults and children's ages in each room. Ask for the currency and the lead guest's nationality: nationality changes the price and must be truthful, so ask rather than assume. Optional filters are refundable only, minimum guest rating and star rating.
2. **Search.** Call `search_hotels`. Present each hotel's name, area, stars and guest rating, and its cheapest rate with the total for the stay, per-night price, board, refundability and anything in `payableAtProperty`. Search prices are not final and may change at preparation.
3. **Compare rooms.** When the user is interested in a hotel, call `get_hotel_offers` with its `hotelRef`. Present the rooms, board, total, taxes and fees, amounts payable at the property, refundability and the cancellation penalty windows with their dates, plus check-in and check-out times and important policies.
4. **Prepare.** Only when the user picks a specific rate and wants to proceed, call `prepare_travel_booking` with that `offerRef` (see "Request keys"). Preparation re-verifies the price with the hotel. It books, holds and charges nothing.
5. **Present the exact offer** before the link: hotel, room, board, dates and nights, rooms and guests, the final total and currency, taxes and fees, anything payable at the property, refundability and cancellation penalties, and the key terms and conditions. If `priceChanged` is true, show the new price against `searchedPrice`. If `cancellationChanged` or `boardChanged` is true, say what changed. Present terms accurately and point out material conditions, such as non-refundable rates.
6. **Hand over the checkout.** Give the user the `checkoutUrl` and say when it expires (`checkoutExpiresAt`). Explain that they enter the guest details, approve the exact price and pay on that page, and that nothing is booked until payment succeeds and the booking is confirmed.

## Flights

1. **Gather the journey.** Origin and destination as 3-letter IATA airport or city codes (confirm the airport when a city has several), departure date, return date or one-way, adults, children's ages (2 to 11), infants' ages (0 to 1, at most one infant per adult), and the currency. Ask about cabin class and maximum stops if they matter.
2. **Search.** Call `search_flights`. Present each fare's airline and flight numbers, departure and arrival times, stops and layovers, total duration, cabin and fare family, total and per-passenger price with the base, taxes and fees, checked and cabin baggage, whether it is changeable or refundable with any fees, and the offer expiry. Fares expire quickly and seats are not held.
3. **Prepare.** Only when the user picks a fare and wants to proceed, call `prepare_travel_booking` with that `offerRef` (see "Request keys"). Preparation re-verifies the fare with the airline. It does not book, hold a seat or charge.
4. **Present the exact offer** before the link: the verified itinerary, the final total and currency, the breakdown, baggage, change and refund conditions with fees, and `offerExpiresAt`. If `priceChanged`, `fareChanged` or `cabinChanged` is true, or there are `changeMessages`, show what changed and the price against `searchedPrice`.
5. **Hand over the checkout.** Give the user the `checkoutUrl` and its expiry. Explain that they enter each passenger's details exactly as on their travel document, plus contact details, on that page. The seat is held only once those details are submitted there; if the airline's price moves at that point, the page asks them to approve the new price before payment. They approve and pay on the page, and nothing is booked or ticketed until the booking shows that.

## Hotels and flights are separate

Hotels and flights are separate bookings with separate prices, checkouts, terms and cancellations. Never present them as a package, a bundle or a combined price, and never imply that one depends on the other. If the user wants both, prepare each one separately, give each its own checkout link, and make clear that cancelling or failing one does not change the other.

## Request keys

`prepare_travel_booking` and `cancel_travel_booking` take a `requestKey` of 8 to 80 letters, digits, `-` or `_`. Create a new random key for each new offer or cancellation, and reuse the same key only when retrying that same call after an error or timeout. A reused key returns the earlier preparation without a new checkout link; for a fresh link, prepare again with a new key. `IDEMPOTENCY_CONFLICT` means the key was used for a different offer, so use a new one.

## Booking states

`get_travel_booking` returns a `stage`, a more detailed `status` and a `next` hint. Report them in plain words. These are different states; never merge them:

- `prepared_offer`: the price is re-verified and nothing is booked or charged. The user still has to complete checkout.
- `seat_held_awaiting_payment` (flights): passenger details were submitted and the seat is held, but nothing is booked or paid yet.
- Status `price_change_review`: the price changed when the seat was held; the user must approve the new price on the checkout page.
- Status `awaiting_payment` or `payment_failed`: waiting for payment on the checkout page. A failed payment can be retried on the same page.
- `booking_in_progress` (also shown as status `booking_unknown`): the booking is being confirmed with the provider. Do not prepare again; check back with `get_travel_booking`.
- `reservation_pending`: the provider has not confirmed yet.
- `reservation_confirmed` (hotels): the stay is booked. The hotel's own confirmation code can arrive later; say so if it is still pending.
- `reservation_confirmed_ticket_pending` (flights): the reservation is confirmed, but the airline ticket is not issued yet. Do not call it a ticket or say the user is ticketed.
- `ticket_issued` (flights): the airline ticket is issued. Give the airline confirmation (PNR) and booking reference.
- `expired`: the checkout expired. Search again for a current offer.
- `booking_failed`: the booking was not completed. Any payment hold is released by the payment provider. Offer to search again.
- `cancellation_pending`, `cancelled`, `cancelled_with_charges`: see "Cancel".

After the user says they have paid, call `get_travel_booking` with the `bookingRef` and report the actual state, references and any outstanding step. Keep the `bookingRef` and provider references for the user. Use `list_travel_bookings` to find this connection's hotel and flight bookings. Bookings belong to the connection that prepared them; reconnecting does not recover earlier ones, so send recovery questions to Concronex support.

If a result is uncertain or a call times out, read the booking state with `get_travel_booking` before doing anything else. Never prepare or pay again to resolve uncertainty.

## Cancel

1. Identify the user's booking and call `prepare_travel_cancellation`. It does not cancel. Only confirmed or ticketed bookings can be cancelled.
2. Show the returned terms exactly: refundability, the expected penalty and refund, the policy, the `basis`, and when these terms expire. For flights the refund is the airline's maximum and is not guaranteed until the airline confirms; say so.
3. Cancellation cannot be undone. Wait for the user's fresh, explicit confirmation of those exact terms in this chat. General intent, an earlier approval, silence or a tool-permission click is not confirmation. If they decline, stop.
4. Then call `cancel_travel_booking` with the `bookingRef`, the `cancellationHash`, `confirmation: "confirmed_in_chat"` and a new `requestKey`.
5. Report only the verified outcome. `cancellation_pending` means the request was accepted but is not complete; flight cancellations often start this way, so check later with `get_travel_booking`. `cancelled_with_charges` means a penalty applied. `CANCELLATION_REFUSED` means nothing changed.
6. `CANCELLATION_TERMS_CHANGED` or `CANCELLATION_TERMS_EXPIRED` means prepare the cancellation again, show the new terms and get fresh confirmation.

## Errors

- `OFFER_EXPIRED`, `OFFER_UNAVAILABLE` or `NOT_FOUND` for an offer: the fare or rate is gone or cannot be offered. Search again; never substitute a different offer without the user choosing it.
- `INVALID_INPUT`: fix the inputs (dates, codes, passenger ages) and retry.
- A rate limit or provider error: tell the user and try again later rather than repeating searches.
