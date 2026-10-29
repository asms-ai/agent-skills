---
name: sms-verification
description: Get a phone number and the SMS verification code (OTP) for a service using asms.ai. Use when the user needs to receive a one-time SMS code on a number other than their own, for example to verify an account or test a sign-up flow.
---

# SMS verification codes with asms

asms.ai sells one-time phone numbers for receiving a verification code. The user pays from their asms wallet, only when a code arrives; an order with no code refunds itself within about 10 minutes.

## Before you start

- Prefer the asms MCP tools if they are connected (`get_pricing`, `create_otp_order`, `poll_sms`). Setup for each client: https://asms.ai/agents
- Otherwise use the REST API with the user's key from the `ASMS_API_KEY` environment variable. Never print the key, and never create an account, a key or a top-up for the user. If there is no key, ask the user to create one at https://asms.ai/dashboard and stop until they confirm.

## Steps

1. **Price and stock.** Ask which service (e.g. `telegram`, `whatsapp`, `google`) and country (e.g. `us`, `uk`) unless the user said.
   - MCP: `get_pricing` with `{ "country": "us", "service": "telegram" }`
   - REST: `GET https://asms.ai/api/v1/otp/services?country=us` with `Authorization: Bearer $ASMS_API_KEY`
2. **Confirm.** Tell the user the price and get an explicit yes before ordering. Ordering spends wallet money.
3. **Order.**
   - MCP: `create_otp_order` with `{ "country": "us", "service": "telegram" }`
   - REST: `POST https://asms.ai/api/v1/otp/order` with body `{"country":"us","service":"telegram"}`. The answer has `activationId` and `phoneNumber`.
4. **Give the user the number** and tell them to request the code on the service.
5. **Poll** every 3 to 5 seconds for up to about two minutes.
   - MCP: `poll_sms` with `{ "orderId": "<activationId>" }`
   - REST: `GET https://asms.ai/api/v1/otp/sms?id=<activationId>`; the code is in `sms.code` once it arrives.
6. **Report the code.**

## When no code arrives

- Don't cancel to retry. Order a fresh number (after confirming with the user again) and poll that one. The quiet order refunds itself, so the user only pays for codes that land.
- A stalled code usually means the service rejected that number, not a fault at asms. Trying another country or pool often works.
- More help: https://asms.ai/not-receiving-sms-code

## Need a number for longer?

For a number the user keeps and receives every SMS on (testing their own app, an account they'll verify again later), use the `rent-phone-number` skill.
