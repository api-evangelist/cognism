---
name: Enrich a CRM record with Cognism
description: Take a contact you already hold — an email, a LinkedIn URL, or a name plus a company — match it to the best Cognism record, check what data is available, and redeem the full profile only when it is worth a credit.
api: openapi/cognism-api-openapi.yml
operations: [enrichContact, redeemContacts, getContactEntitlement, getOptOutByEmail]
generated: '2026-08-13'
method: generated
source: openapi/cognism-api-openapi.yml + conventions/cognism-conventions.yml + errors/cognism-problem-types.yml
---

# Enrich a CRM record with Cognism

The core enrichment flow. Two calls minimum, and the second one costs money — so the whole skill is
about not making the second call blindly.

## Before you start

- Base URL: `https://app.cognism.com`
- Auth: `Authorization: Bearer <API_TOKEN>` on every request. Tokens are minted in the Cognism app
  under Settings > Tokens and API and expire after 6 months.
- API access is sales-gated. If a fresh token still returns 401, the organisation's Entitlements are
  not provisioned — that is an Account Manager problem, not a code problem.

## Step 0 — read the entitlement map once, cache it

`getContactEntitlement` → `GET /api/search/entitlement/contactEntitlementSubscription`

Returns a map of contact field name to boolean. Fields your organisation is not licensed for will be
**silently absent** from every redeem response. Cache this and use it to interpret every later
result, otherwise you will report "Cognism has no mobile number for this person" when the truth is
"we did not buy mobile numbers".

## Step 1 — match the record

`enrichContact` → `POST /api/search/contact/enrich`

Send the strongest identifier you hold. In descending order of match quality:

| What you send | Approximate matchScore |
|---|---|
| `firstName` + `lastName` + `email` + `phoneNumber` | 87-94 |
| `firstName` + `lastName` + `email` | 66-72 |
| `firstName` + `lastName` + `phoneNumber` + `accountName` | 51-60 |
| `email` alone | 47-49 |
| `firstName` + `lastName` + `accountName` | 33-50 |
| `firstName` + `lastName` alone | error — not enough input |

```json
{ "email": "person@example.com" }
```

Other accepted fields: `linkedinUrl`, `sha256` (hashed email, if you cannot transmit the address),
`jobTitle`, `accountName`, `accountWebsite`, `phoneNumber`, `anchorFields[]` (fields that MUST match),
`minMatchScore`.

This call is **free**. It consumes no credits.

## Step 2 — decide, do not redeem reflexively

The response carries `matchScore` and a `results[]` array of previews.

- **Empty `results[]` with HTTP 200 is the normal miss.** It is not an error. Either nothing matched
  or the best match scored below `minMatchScore` (default 30). Check `results.length` before
  anything else.
- `matchScore` below 27 is a low-quality match even when it is returned. Decide your own floor; you
  can raise or lower it per request with `minMatchScore`.
- Each preview carries `has*` booleans — `hasEmail`, `hasMobilePhoneNumbers`, `hasDirectPhoneNumbers`,
  `hasLinkedinUrl`, `hasSkills`, `hasEducation`, `hasPreviousAccounts`. **This is what you are paying
  for.** If you need a mobile number and `hasMobilePhoneNumbers` is `false`, redeeming spends a credit
  for data you already know is not there. Stop here.
- Keep `results[0].redeemId`.

## Step 3 — check suppression

`getOptOutByEmail` → `GET /api/search/contact/optOut/email/{email}`

If the contact is on the opt-out list, do not redeem and do not contact them. Phone numbers in a
redeemed record also carry a `dnc` (Do Not Call) flag — respect it per number.

## Step 4 — redeem

`redeemContacts` → `POST /api/search/contact/redeem`

```json
{ "redeemIds": ["<redeemId from step 2>"] }
```

- 1 to 20 redeem IDs per request. Throughput ceiling is 1,000 records per minute.
- Optional `?mergePhonesAndLocations=true` consolidates phone and location arrays.
- **This consumes a credit** — one per contact, on first redemption only. Re-redeeming a contact your
  organisation has already redeemed is free; a credit is only spent again if the contact's key
  details change, such as a job move.
- There is **no idempotency key**. A retried redeem of a not-yet-redeemed contact is the one way to
  spend twice. Deduplicate `redeemIds` before sending and treat the call as at-least-once.

## Step 5 — handle what comes back

- Response shape is `{ "totalResults": n, "results": [ ... ] }`.
- A `redeemId` you stored earlier may be stale if the person changed job or employer. That is not an
  error: Cognism falls back to the current record and returns the **new** `redeemId` alongside the
  data. Write it back to your CRM.
- Company switchboard and HQ phone numbers are **not** in a contact redemption. If you need them,
  redeem the Account instead — `redeemAccounts` is free.

## Errors

Cognism returns a top-level JSON **array**, not an object:

```json
[{"key": "MissingCredentials", "code": 401, "msg": "Missing required credentials"}]
```

| Status | Meaning | Do this |
|---|---|---|
| 400 | Invalid body, wrong parameter name, bad type | Validate against the schema; watch the nested filter paths |
| 401 | Missing/invalid/expired token, **or** unset Entitlements | Rotate the token; if still failing, escalate to the Account Manager |
| 404 | Route does not exist | All endpoints live under `/api/search/` |
| 429 | Rate limit exceeded | Back off — no `Retry-After` or `RateLimit-*` header is returned, so backoff must be blind |
