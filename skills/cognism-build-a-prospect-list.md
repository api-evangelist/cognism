---
name: Build a prospect list with Cognism
description: Search the Cognism database for contacts matching a target persona, page through the results with the forward-only cursor, and redeem only the records that carry the fields you actually need.
api: openapi/cognism-api-openapi.yml
operations: [listIndustries, listJobFunctions, listSeniorityLevels, listManagementLevels, listCountries, listTechnologies, searchContacts, redeemContacts]
generated: '2026-08-13'
method: generated
source: openapi/cognism-api-openapi.yml + conventions/cognism-conventions.yml
---

# Build a prospect list with Cognism

Prospecting flow: define the persona against Cognism's controlled vocabularies, search, page, then
redeem selectively.

## Step 1 — resolve the vocabularies first

Cognism's filters take **exact strings from its own controlled lists**. Do not hard-code them and do
not guess — fetch them. All of these are free `GET`s and all are cacheable:

| Filter you want to use | Operation | Path |
|---|---|---|
| `account.industries` | `listIndustries` | `/api/search/filter/industries` |
| `jobFunctions` | `listJobFunctions` | `/api/search/filter/jobFunctions` |
| `seniority` | `listSeniorityLevels` | `/api/search/filter/seniority` |
| `managementLevel` | `listManagementLevels` | `/api/search/filter/managementLevels` |
| `countries` / `states` / `regions` | `listCountries` / `listStates` / `listRegions` | `/api/search/filter/...` |
| `account.technologies` | `listTechnologies` | `/api/search/filter/technologiesSearch?search=<term>` |
| `account.sic` / `isic` / `naics` | `listSicCodes` / `listIsicCodes` / `listNaicsCodes` | `/api/search/filter/...` |
| `skills` | `listSkills` | `/api/search/filter/skills` |
| `account.types` | `listCompanyTypes` | `/api/search/filter/companyTypes` |

`listTechnologies` is the only one that takes a `search` term and pages (`indexSize`,
`lastReturnedKey`) — the technology list is long.

Some vocabularies are also documented inline: `seniority` accepts Manager, Director, Partner, CXO,
Owner, VP; `managementLevel` accepts Entry-Level, Team-Lead, Experienced Staff, Executive-Level,
Senior Leadership, Middle-Management, CxO.

## Step 2 — search

`searchContacts` → `POST /api/search/contact/search?indexSize=100`

```json
{
  "jobTitles": ["Head of Revenue Operations"],
  "seniority": ["Director", "VP"],
  "regions": ["EMEA"],
  "emailQuality": { "highPlus": true },
  "mobilePhoneNumbers": { "highPlus": true },
  "account": {
    "industries": ["Computer Software"],
    "headcount": { "from": 200, "to": 2000 },
    "technologies": ["Salesforce"]
  },
  "searchOptions": {
    "match_exact_job_title": false,
    "ai_job_title": true,
    "sort_fields": ["ProfileScoreDESC"]
  }
}
```

Things worth knowing about the filter set:

- `indexSize` must be **20-100**. The default in Cognism's own example is 25.
- Quality filters (`emailQuality`, `mobilePhoneNumbers`, `directPhoneNumbers`,
  `account.hqPhoneNumbers`, `account.officePhoneNumbers`) are `{medium|high|highPlus: true}` objects,
  not strings. Filtering to `highPlus` up front is the cheapest way to avoid redeeming weak data.
- `searchOptions.match_exact_job_title: false` (the default) tokenises the title — "Partnerships
  Manager" will also return "Senior Manager, Integrated Marketing Partnerships". Set it `true` for a
  word-for-word match, and combine with `ai_job_title: true` for a broad-but-targeted search.
- Every positive filter has an `exclude*` twin (`excludeJobTitles`, `excludeCountries`,
  `excludeIndustries`, `account.excludedTechnologies`, …). Use them; they are cheaper than filtering
  client-side.
- Signal filters take timestamp ranges in **milliseconds**: `account.hiringEvent`,
  `account.fundingEvent` (with `fundingType` and `series`), `account.ipoEvent`,
  `account.acquisitionEvent`, plus contact-side `jobJoinEvent`, `jobLeaveEvent`, `locationMoveEvent`.
  `account.accountSearchOptions.events_operator` switches AND/OR across them (default OR).

Searching is **free**.

## Step 3 — page forward, and only forward

The response is `{ "lastReturnedKey": "...", "totalResults": n, "results": [...] }`.

Pass `lastReturnedKey` back as a query parameter to get the next page. **Paging is forward-only** —
there is no offset, no page number, and no way to jump back. If you need the whole list, walk it in
one pass and persist as you go. There is no longer a 10,000-result display cap.

## Step 4 — filter the previews before you spend

Each result is a preview with `has*` booleans and a `redeemId`. Filter here, in memory, for free:

```
keep = [r for r in results if r["hasEmail"] and r["hasMobilePhoneNumbers"]]
```

Redeeming a preview whose `hasMobilePhoneNumbers` is `false` spends a credit to learn something the
preview already told you.

## Step 5 — redeem in batches of 20

`redeemContacts` → `POST /api/search/contact/redeem`

```json
{ "redeemIds": ["<id1>", "<id2>", "… up to 20"] }
```

- Hard ceiling: **20 redeem IDs per request**, **1,000 records per minute** across requests.
- 1 credit per contact, first redemption only.
- No idempotency key — deduplicate IDs before sending, and persist results as each batch returns so
  a crash does not force a re-redeem.
- On 429, back off. No `Retry-After` header is returned.

## Companies instead of people

The same shape with `searchAccounts` (`POST /api/search/account/search`) and `redeemAccounts`
(`POST /api/search/account/redeem`). Account redemption is **free**, so for firmographic-only work
you can redeem freely.
