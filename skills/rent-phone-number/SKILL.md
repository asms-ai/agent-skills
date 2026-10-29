---
name: rent-phone-number
description: Rent a phone number from asms.ai for a day to a year and read the SMS it receives. Use when the user needs a number of their own for longer than one code, such as testing their app's SMS login or giving a bot or app account its own number.
---

# Rent a phone number with asms

A rented asms number belongs to the user for the whole period and receives every SMS sent to it, from any sender. It is paid from the user's asms wallet.

## Before you start

- This needs the asms MCP server connected (`get_rental_offers`, `rent_number`, `read_rental_sms`). Setup for each client: https://asms.ai/agents
- Never create an account, an API key or a top-up for the user. If asms isn't connected, point the user to https://asms.ai/agents and stop.

## Steps

1. **Offers.** Call `get_rental_offers` with no arguments to list countries, then with `{ "country": "uk" }` for that country's periods and prices.
2. **Confirm.** Tell the user the country, period and price and get an explicit yes. Renting spends wallet money.
3. **Rent.** Call `rent_number` with `{ "country": "uk", "period": "oneMonth" }`. Periods: `oneDay`, `oneWeek`, `oneMonth`, `threeMonths`, `sixMonths`, `twelveMonths` (not every period is sold in every country). The answer has the `number` and `expiresAt`.
4. **Read SMS.** Call `read_rental_sms` with `{ "number": "<number>" }`; poll every few seconds while waiting for a message.
5. **Later.** `list_rentals` shows the user's numbers and end dates.

## Cancelling

`cancel_rental` refunds a rental only between 5 and 120 minutes after renting, and only if no SMS has arrived. After that the rental runs to its end date.

## Good to know

- Choose a period that covers how long the user needs the number. When a rental ends the number can go to someone else, so for an account that may need verifying again, rent for longer and turn on the account's own two-step protection.
