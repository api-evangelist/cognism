---
name: Honour Cognism opt-outs and entitlements
description: Sync the Cognism opt-out suppression list into your own systems, check individual records before outreach, and read the entitlement map so you interpret missing fields correctly.
api: openapi/cognism-api-openapi.yml
operations: [listOptOutContacts, getOptOutByEmail, getOptOutById, getContactEntitlement, getAccountEntitlement]
generated: '2026-08-13'
method: generated
source: openapi/cognism-api-openapi.yml + conventions/cognism-conventions.yml + https://www.cognism.com/compliance
---

# Honour Cognism opt-outs and entitlements

Cognism sells B2B personal data under GDPR and CCPA, and it exposes the compliance machinery as API
surface rather than leaving it to a PDF. Two things you are expected to consume: the **opt-out
suppression list** and the **entitlement map**. Both are free.

## Part 1 — the opt-out suppression list

### Sync the whole list

`listOptOutContacts` → `GET /api/search/contact/optOut?pageSize=100`

Note the parameter names: this endpoint uses `pageSize` and `pageKey`, **not** the `indexSize` and
`lastReturnedKey` the search endpoints use. `pageKey` comes back with every response; pass it to
request the next page.

```
GET /api/search/contact/optOut?pageSize=100
GET /api/search/contact/optOut?pageSize=100&pageKey=<pageKey from previous response>
```

Persist the result into your own suppression table and re-sync on a schedule. Opt-outs accrue after
you have already ingested a contact, so a one-time sync is not enough — a record you redeemed last
quarter can be suppressed today.

### Check one record before outreach

`getOptOutByEmail` → `GET /api/search/contact/optOut/email/{email}`

`getOptOutById` → `GET /api/search/contact/optOut/id/{redeemId}`

Use the email form when you are checking data that came from your own CRM, and the redeem-ID form
when you are checking a record that came out of Search or Enrich.

Run this **before** redeeming, not after. A suppressed contact is a contact you must not use, so
redeeming one spends a credit on data you cannot act on.

### Per-number Do Not Call

Redeemed phone numbers carry a `dnc` boolean:

```json
{ "number": "+442038580822", "numberType": "FIXED_LINE", "label": "COMPANY_SWITCHBOARD", "score": 20, "dnc": false }
```

`dnc: true` means do not dial that number, independently of whether the contact is on the opt-out
list. Treat suppression and DNC as two separate gates that both have to pass.

## Part 2 — the entitlement map

`getContactEntitlement` → `GET /api/search/entitlement/contactEntitlementSubscription`

`getAccountEntitlement` → `GET /api/search/entitlement/accountEntitlementSubscription`

Both return a flat map of field name to boolean:

```json
{ "contact Entitlement": { "email": true, "mobilePhoneNumbers": false, "directPhoneNumbers": false, "skills": true, "education": true } }
```

Note the space in the key `"contact Entitlement"` / `"account Entitlement"` — parse it literally.

### Why this matters

The Redeem API returns **only** the fields inside your organisation's entitlement, and it omits the
rest silently. There is no error, no null, no flag. So:

- `mobilePhoneNumbers` absent + `mobilePhoneNumbers: false` in the map = **you did not buy it**.
- `mobilePhoneNumbers` absent + `mobilePhoneNumbers: true` in the map = **Cognism has no number for
  this person**.

Those are completely different facts and only the entitlement map can tell them apart. Fetch it once
at startup, cache it, and use it to label every gap in a redeemed record. Reporting "Cognism has poor
mobile coverage" when the real answer is "our contract excludes mobiles" is the single most common
misreading of this API.

Entitlements are configured by the Cognism Provisioning team after Account Manager approval — they
cannot be changed through the API. If a field you need is `false`, that is a commercial conversation.

## Part 3 — where the boundaries are

- Cognism's compliance posture is documented at https://www.cognism.com/compliance ; North America
  specific disclosures at https://www.cognism.com/customer-terms/north-america-specific-disclosures
- Cognism publishes **no webhooks and no event stream**, so there is no push notification when a
  contact opts out. Polling `listOptOutContacts` is the only mechanism. Schedule it.
- All calls in this skill are free — no credits are consumed by opt-out lookups or entitlement reads.
