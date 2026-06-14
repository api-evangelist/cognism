# Cognism GraphQL Schema

## Overview

This conceptual GraphQL schema models the Cognism B2B sales intelligence platform, covering contact enrichment, company data, firmographics, phone and email verification, list management, CRM integrations, webhooks, and authentication. Cognism's core REST API is an enterprise-gated enrichment and data platform. This schema represents those capabilities as a GraphQL surface.

## Provider

- **Name**: Cognism
- **Category**: B2B Sales Intelligence
- **Base URL**: https://api.cognism.com
- **Developer Portal**: https://www.cognism.com/api
- **Documentation**: https://docs.cognism.com/

## Schema Source

Conceptual — derived from Cognism's public documentation, developer portal, and published API capabilities covering contact enrichment, company enrichment, verified phone (Diamond Data), email verification, intent signals, list management, CRM integrations (Salesforce, HubSpot, Outreach, Salesloft), webhook events, and authentication.

## Types

### Contact Types

- **Contact** — Core contact record with name, title, seniority, department, and linked company.
- **ContactDetails** — Extended contact data including social profiles, location, and verification status.
- **ContactEmail** — Email address with type (work/personal), validity status, and verification metadata.
- **ContactPhone** — Phone number with type (mobile/direct/company), Diamond Data flag, and verification status.
- **ContactVerification** — Aggregate verification record indicating overall contact data confidence.
- **ContactTitle** — Job title string with normalized role classification.
- **ContactSeniority** — Seniority level enum (C-Suite, VP, Director, Manager, Individual Contributor).
- **ContactDepartment** — Functional department classification (Sales, Marketing, Engineering, Finance, HR, etc.).

### Company Types

- **Company** — Core company entity with name, domain, industry, and firmographic summary.
- **CompanyDetails** — Extended company data including description, founded year, social profiles, and HQ location.
- **CompanySize** — Employee count range and headcount growth signals.
- **CompanyRevenue** — Annual revenue range and currency.
- **CompanyIndustry** — Primary and secondary industry classifications with NAICS/SIC codes.
- **CompanyType** — Organization type (Private, Public, Non-Profit, Government, Education).
- **CompanyKeyword** — Keyword or tag associated with a company for search and segmentation.
- **CompanyTechnology** — Technology product detected in use at the company.
- **TechCategory** — Technology category grouping (CRM, Marketing Automation, ERP, Cloud, Security, etc.).
- **Tech** — Individual technology product with name, vendor, and category.
- **CompanyLocation** — Office location record linked to a company.
- **Address** — Structured postal address with street, city, state, postal code, and country.

### Funding Types

- **FundingDetails** — Aggregate funding summary including total raised and last round.
- **Funding** — Funding event record with round type, amount, date, and investors.
- **FundingRound** — Round classification (Seed, Series A–F, Growth, IPO, Acquisition, Debt Financing).
- **Investor** — Investor entity with name, type (VC, PE, Angel, Corporate), and portfolio links.

### Search and Query Types

- **SearchFilter** — Filter criteria for contact or company search (industry, seniority, company size, geography, technology, keywords).
- **SearchResults** — Paginated search response containing matched contacts or companies and metadata.
- **SearchQuery** — Saved or submitted search query with filter snapshot and result count.

### Enrichment Types

- **Enrichment** — Top-level enrichment request linking input identifiers to enriched output.
- **EnrichmentDetails** — Metadata about an enrichment operation including match confidence and fields populated.
- **ContactEnrichment** — Enrichment result for a single contact including all populated fields and match score.
- **CompanyEnrichment** — Enrichment result for a single company including firmographics and technology stack.
- **PhoneVerification** — Phone number verification result with line type, carrier, and Diamond Data status.
- **EmailVerification** — Email address verification result with syntax, MX, and deliverability checks.
- **BulkEnrichment** — Batch enrichment job submission containing multiple contact or company records.
- **BulkResults** — Batch enrichment job results with per-record status and enriched data.

### List and Segment Types

- **List** — Named list of contacts or companies for prospecting or export.
- **ListDetails** — Metadata about a list including owner, created date, and record count.
- **ListContact** — Junction type linking a contact record to a list with date added.
- **SegmentDetails** — Metadata about a saved audience segment including filter criteria snapshot.
- **Segment** — Dynamic or static segment of contacts or companies based on defined criteria.

### CRM Integration Types

- **CRMIntegration** — Base integration record with connected CRM name, status, and sync settings.
- **SalesforceIntegration** — Salesforce-specific integration settings including instance URL, field mappings, and sync frequency.
- **HubSpotIntegration** — HubSpot-specific integration settings including portal ID and property mappings.
- **OutreachIntegration** — Outreach.io integration settings including sequence push rules and field mappings.
- **SalesloftIntegration** — Salesloft integration settings including cadence assignment rules and field mappings.

### Webhook Types

- **Webhook** — Registered webhook endpoint with URL, active status, and subscribed event types.
- **WebhookEvent** — Delivered webhook event payload with event type, timestamp, and data object.

### Authentication Types

- **APIKey** — API key credential with name, scopes, created date, and expiry.
- **Token** — Bearer token issued for API authentication with expiry and associated scopes.

## Queries

- `contact(id: ID!)` — Fetch a single contact by ID.
- `contacts(filter: SearchFilter, page: Int, limit: Int)` — Search contacts with filters.
- `company(id: ID!)` — Fetch a single company by ID or domain.
- `companies(filter: SearchFilter, page: Int, limit: Int)` — Search companies with filters.
- `enrichContact(input: ContactEnrichmentInput!)` — Enrich a contact by email, LinkedIn URL, or name+company.
- `enrichCompany(input: CompanyEnrichmentInput!)` — Enrich a company by domain or name.
- `bulkEnrichStatus(jobId: ID!)` — Poll the status of a bulk enrichment job.
- `list(id: ID!)` — Fetch a named prospect list.
- `lists` — List all saved prospect lists.
- `segment(id: ID!)` — Fetch a saved segment.
- `webhooks` — List registered webhook endpoints.
- `apiKeys` — List API keys for the authenticated account.

## Mutations

- `submitBulkEnrichment(input: BulkEnrichmentInput!)` — Submit a batch enrichment job.
- `createList(input: ListInput!)` — Create a new prospect list.
- `addContactToList(listId: ID!, contactId: ID!)` — Add a contact to a list.
- `removeContactFromList(listId: ID!, contactId: ID!)` — Remove a contact from a list.
- `createSegment(input: SegmentInput!)` — Create a new audience segment.
- `registerWebhook(input: WebhookInput!)` — Register a new webhook endpoint.
- `deleteWebhook(id: ID!)` — Delete a registered webhook.
- `createAPIKey(input: APIKeyInput!)` — Generate a new API key.
- `revokeAPIKey(id: ID!)` — Revoke an existing API key.

## Notes

- Cognism's production API requires enterprise access. This schema is a conceptual representation of the platform's capabilities.
- Diamond Data refers to Cognism's mobile-verified phone numbers with highest confidence scores.
- GDPR compliance signals (lawful basis, opt-out status) are embedded on ContactEmail and ContactPhone types.
- Intent data signals (Bombora-powered) are available on the Company type for enterprise tiers.
- CRM integrations sync bidirectionally via Cognism's native connectors.
