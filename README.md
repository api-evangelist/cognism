# Cognism

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Cognism is a B2B sales intelligence platform selling verified emails, mobile numbers, direct dials, firmographic and technographic data, buying-intent signals and contact enrichment to sales, marketing and RevOps teams, with a compliance-first posture built around GDPR and CCPA. Its API is a three-service REST contract published as a Postman collection at developers.cognism.com — a Search API for finding contacts and companies by filter, an Enrich API for matching records you already hold, and a Redeem API for exchanging the resulting redeem ID for the full profile. Search and Enrich return preview records and cost nothing; credits are consumed only when a contact is redeemed for the first time. A Compliance API exposes the opt-out suppression list, and an Entitlement API reports which fields the account is licensed to receive. API access is sales-gated: it must be enabled on the subscription, field-level entitlements provisioned by Cognism, and a six-month bearer token generated in the application.

**Areas:** Sales Intelligence · B2B · Data Enrichment · Contact Data · GDPR · Intent Data · Lead Generation · Firmographics · Technographics · Company Data · Prospecting · Data as a Service

## Cognism on APIs.io

Everything profiled for Cognism is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/cognism/](https://apis.io/providers/cognism/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [Cognism Compliance API](https://apis.io/apis/cognism/cognism-compliance-api/) — Opt-out list lookups for GDPR/CCPA suppression.
- [Cognism Enrich API](https://apis.io/apis/cognism/cognism-enrich-api/) — Match a record you already hold to a Cognism record.
- [Cognism Entitlement API](https://apis.io/apis/cognism/cognism-entitlement-api/) — Read which fields your organisation is licensed for.
- [Cognism Filter API](https://apis.io/apis/cognism/cognism-filter-api/) — Reference lists of values the search filters accept.
- [Cognism Redeem API](https://apis.io/apis/cognism/cognism-redeem-api/) — Exchange a redeem ID for the full record.
- [Cognism Search API](https://apis.io/apis/cognism/cognism-search-api/) — Find contacts and companies matching a filter set.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [cognism](https://apis.io/servers/cognism/cognism/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Cognism Agentic Access](https://apis.io/agentic-access/cognism/cognism-agentic-access/)

#### Examples (34)

Example requests and responses.

- [Cognism Account Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-account-advanced-entitlement-response/)
- [Cognism Contact Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-contact-advanced-entitlement-response/)
- [Cognism Enrich Account By Domain Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-domain-advanced-entitlement-response/)
- [Cognism Enrich Account By Domain Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-domain-standard-entitlement-response/)
- [Cognism Enrich Account By Linkedinurl Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-linkedinurl-advanced-entitlement-response/)
- [Cognism Enrich Account By Linkedinurl Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-linkedinurl-standard-entitlement-response/)
- [Cognism Enrich Account By Various Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-various-standard-entitlement-response/)
- [Cognism Enrich Account By Website Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-website-advanced-entitlement-response/)
- [Cognism Enrich Account By Website Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-account-by-website-standard-entitlement-response/)
- [Cognism Enrich Account Request](https://apis.io/examples/cognism/cognism-enrich-account-request/)
- [Cognism Enrich Accountby Various Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-accountby-various-advanced-entitlement-response/)
- [Cognism Enrich Contact By Email Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-email-advanced-entitlement-response/)
- [Cognism Enrich Contact By Email Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-email-standard-entitlement-response/)
- [Cognism Enrich Contact By Firstname Lastname And Website Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-firstname-lastname-and-website-advanced-entitlement-response/)
- [Cognism Enrich Contact By Firstname Lastname And Website Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-firstname-lastname-and-website-standard-entitlement-response/)
- [Cognism Enrich Contact By Linkedinurl Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-linkedinurl-advanced-entitlement-response/)
- [Cognism Enrich Contact By Linkedinurl Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-linkedinurl-standard-entitlement-response/)
- [Cognism Enrich Contact By Phonenumber And Firstname Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-phonenumber-and-firstname-advanced-entitlement-response/)
- [Cognism Enrich Contact By Phonenumber And Firstname Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-phonenumber-and-firstname-standard-entitlement-response/)
- [Cognism Enrich Contact By Various Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-various-advanced-entitlement-response/)
- [Cognism Enrich Contact By Various Standard Entitlement Response](https://apis.io/examples/cognism/cognism-enrich-contact-by-various-standard-entitlement-response/)
- [Cognism Enrich Contact Request](https://apis.io/examples/cognism/cognism-enrich-contact-request/)
- [Cognism Redeem Accounts By Ids Request](https://apis.io/examples/cognism/cognism-redeem-accounts-by-ids-request/)
- [Cognism Redeem Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-redeem-advanced-entitlement-response/)
- [Cognism Redeem Contact Advanced Entitlement Response](https://apis.io/examples/cognism/cognism-redeem-contact-advanced-entitlement-response/)
- …and 9 more published on APIs.io

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Cognism Api Overlay](https://apis.io/overlays/cognism/cognism-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Cognism Authentication](https://apis.io/security/cognism/cognism-authentication/)
- [Cognism Domain Security](https://apis.io/security/cognism/cognism-domain-security/)
- [Cognism Trust Center](https://apis.io/security/cognism/cognism-trust-center/)
- [Cognism Vulnerability Disclosure](https://apis.io/security/cognism/cognism-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Cognism Plans Pricing](https://apis.io/plans/cognism/cognism-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Cognism Rate Limits](https://apis.io/rate-limits/cognism/cognism-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Cognism Finops](https://apis.io/finops/cognism/cognism-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.cognism.com/)
- [Pricing](https://www.cognism.com/pricing)
- [Sign up](https://www.cognism.com/demo)
- [Support](https://help.cognism.com/hc/en-gb)
- [Blog](https://www.cognism.com/blog/rss.xml)
- [Security](https://www.cognism.com/security)
- [Compliance](https://www.cognism.com/compliance)
- [Terms of service](https://www.cognism.com/terms-of-website-use)
- [Privacy policy](https://www.cognism.com/privacy-policy)
- [GitHub](https://github.com/Cognism)

## API Evangelist rating

**Kin Score 52.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 17.4 |
| Contract governance | 4.5 |
| Operational transparency | 44.7 |
| Developer ergonomics | 63.7 |
| Access clarity | 92.1 |

**Agent readiness 36.9** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | yes |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/cognism/](https://apis.io/providers/cognism/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Cognism: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/cognism/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
