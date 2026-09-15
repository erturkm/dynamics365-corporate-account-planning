# Corporate Banking – Account Planning for Dynamics 365

![Status: demo](https://img.shields.io/badge/status-demo%20asset-orange)
![Not production hardened](https://img.shields.io/badge/production%20hardened-no-red)
![Support: none](https://img.shields.io/badge/support-none%20%7C%20no%20SLA-lightgrey)
![Licence: MIT](https://img.shields.io/badge/licence-MIT-blue)

A model-driven **Account Planning** solution for corporate / wholesale banking, built on Dataverse and Dynamics 365 Sales.

The Account Plan is a single record surfaced through a **19-tab form**, where most tabs are custom HTML web resources rather than standard Dataverse grids — giving a dashboard-style experience closer to a modern banking front-office application.

> ### ⚠️ Demonstration asset — not production software
>
> This repository is a **demo / proof-of-concept** published as a community sample. It is
> **not** a product, and it is **not production hardened**.
>
> - **No warranty of any kind.** Provided "as is" under the MIT Licence.
> - **No support, no SLA, no maintenance commitment.** Issues and pull requests may not be
>   answered. There is no support contract available for it.
> - **Not from Microsoft.** This is not an official Microsoft product and is not endorsed,
>   reviewed or supported by Microsoft or by any bank.
> - **Not security, privacy or compliance reviewed.** It has had no penetration testing,
>   threat modelling, accessibility audit or regulatory review. Field-level security, data
>   loss prevention, auditing and retention are **not** configured.
> - **Not performance tested.** Screens query Dataverse directly from the browser and are
>   built for demo-sized data, not production volumes.
> - **All data is fictional.** Company names, financials, ratings, people and email domains
>   are illustrative and invented. Nothing here is real customer data, and no figure should
>   be relied on. "ADNOC" is used as a recognisable placeholder name only, and nothing in
>   this repository originates from or represents that company.
>
> **Before any production use**, treat this as a starting point to be re-implemented: conduct
> your own security, privacy, accessibility and regulatory review, and test in a
> non-production environment first. You assume all risk for any use you make of it.

---

## What's inside

| Area | Tabs |
|---|---|
| **Client** | Account Plan Summary, Business Background, Group Hierarchy & UBO, Client Financials |
| **Relationship** | Products & Holdings, Share of Wallet, Limits & Utilisation, Relationship Economics, Pricing |
| **Risk** | Covenants & Credit, Risk Register, SWOT |
| **Execution** | Sales Pipeline, Stakeholder Management, Coverage Team Collaboration, Global Team Inputs, Service & Complaints, Timeline, Settings |

### Component summary

| Component type | Count |
|---|---|
| Tables (13 custom + 5 extended OOB) | 18 |
| Columns on OOB tables | 52 |
| Web resources (HTML + JS) | 21 |
| Model-driven app + site map | 2 |
| Business process flow + cloud flow | 2 |
| PCF control (`PipelineDealTracker`) | 1 |
| Global option set (`fsi_tenor`) | 1 |
| Environment variables | 4 |
| **Total** | **109** |

**Custom tables:** `fsi_accountplanning`, `fsi_borrowerentity`, `fsi_companyfinancial`, `fsi_coverageteammember`, `fsi_crosssellproduct`, `fsi_deposit`, `fsi_facility`, `fsi_informationrequest`, `fsi_informationrequesttemplate`, `fsi_limitsandutilization`, `fsi_relationshipeconomics`, `fsi_shareofwallet`, `fsi_stakeholdercoverage`, `cr640_covenant`

**Extended OOB tables:** `account` (28 columns), `contact` (6), `opportunity` (4), plus `incident` and `lead` referenced read-only.

---

## Screens

All screenshots are of the running solution using the bundled demo dataset.

### Account Plan Summary
Relationship direction, revenue and EBITDA, limit utilisation, wallet share and next actions.

![Account Plan Summary](docs/images/01-summary-v2.png)

### Group Hierarchy & UBO
Pan/zoom legal-entity tree driven from Dataverse — ownership percentages, ultimate beneficial owner, country, entity type, and FAB-client vs non-client status. Click any entity for a detail card.

![Group Hierarchy and UBO](docs/images/02-group-hierarchy-ubo-v2.png)

### Client Financials
Three-year financial view — revenue, EBITDA, margin, leverage and cash-flow trends with YoY movement.

![Client Financials](docs/images/03-client-financials-v2.png)

### Products & Holdings
Current product estate with the client: accounts, deposits, lending, trade finance and cash management, plus cross-sell whitespace.

![Products and Holdings](docs/images/04-products-holdings-v2.png)

### Covenants & Credit
Covenant register with headroom, test dates and breach status, alongside facility and collateral detail.

![Covenants and Credit](docs/images/05-covenants-credit-v2.png)

### Service & Complaints
Service quality and complaint history for the relationship.

![Service and Complaints](docs/images/06-service-complaints-v2.png)

### Relationship Economics
Revenue, cost, capital consumption and returns by product line.

![Relationship Economics](docs/images/07-relationship-economics-v2.png)

### Coverage Team Collaboration
Departmental readiness and structured information requests across the global coverage team.

![Coverage Team Collaboration](docs/images/08-coverage-team-collab-v2.png)

---

## Prerequisites

The target environment must have these installed **before** import:

| Requirement | Why |
|---|---|
| **Dynamics 365 Sales** | `opportunity`, sales dashboards, Kanban / Deal Manager PCF controls |
| **Dynamics 365 Customer Service** | `incident` — used by the Service & Complaints tab |
| **Lead Management** | `lead` reference in the app site map |

These are standard first-party solutions and Dataverse resolves them automatically during import.

### Optional — collateral data

The **Covenants & Credit** tab can display collateral records from `msfsi_collateral`, which ships with **Microsoft Cloud for Financial Services** (`FinancialServicesCommonAccelerator`).

That table is a **managed Microsoft component and cannot be redistributed inside this solution**, so it is deliberately excluded. If it is absent the tab degrades gracefully and shows *"No collateral records are held for this borrower."* — everything else on the tab works normally.

To enable it, install the Financial Services accelerator from Microsoft:
<https://learn.microsoft.com/en-us/dynamics365/industry/financial-services/>

---

## Installation

> **Verified:** the managed zip in this repository has been import-tested end to end into a clean Dynamics 365 trial environment — it completed with no errors, and all 14 tables, 7 tab web resources, the model-driven app and all 19 form tabs were confirmed present afterwards.

1. Download `solution/CorporateAccountPlanning_managed.zip` (recommended) or the unmanaged zip for further development.
2. In [Power Apps](https://make.powerapps.com) → **Solutions** → **Import solution**.
3. Select the zip and complete the wizard.
4. **Set the environment variables** (below) — required for AI features.
5. Publish all customisations.

### Environment variables

Values are intentionally **not** shipped. Set these after import under **Solutions → Corporate Banking – Account Planning → Environment variables**:

| Variable | Description |
|---|---|
| `fsi_AoaiEndpoint` | Azure OpenAI chat completions URL, e.g. `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-10-21` |
| `fsi_AoaiTenantId` | Entra tenant id the flow authenticates against |
| `fsi_AoaiClientId` | Entra application (client) id with **Cognitive Services User** on the Azure OpenAI resource |
| `fsi_AoaiClientSecret` | Client secret for that app registration |

These drive the **Global Team Inputs AI service** flow, which summarises and rewrites contributions from coverage teams.

> **Leave them blank to disable AI features** — the rest of the solution works without them. For production, back the secret with **Azure Key Vault** rather than a plain environment variable.

No credentials of any kind are included in this repository.

---

## Notes for implementers

**Publisher prefixes.** Most components use the `fsi` prefix. One table — `cr640_covenant` — uses the source environment's default prefix. This is intentional and preserved deliberately: renaming it would break existing form, view and script references. It imports cleanly as-is.

**Web resources.** The HTML tabs are self-contained (inline CSS/JS, no external CDN calls) and query Dataverse through the Web API using the parent form context. They read the Account Plan record id from the form and resolve the related account from there. Deep links are built relative to the current organisation, so the solution is portable across environments.

**Demo branding.** Screens contain branding and sample content from the original demonstration, including fictional corporate group hierarchies and `@bankfabdemo.com` sample addresses. No real customer data, real email addresses or production endpoints are present.

**Cloud flow.** `GTI - Information Request AI Service` is HTTP-triggered and has **no connection references**, so import will not prompt for connections. After import, copy its HTTP trigger URL into the `__CONFIG__` row of `fsi_informationrequesttemplate` (field `fsi_formjson`, key `aiEndpoint`) to activate AI summarisation.

---

## Demo data

The solution ships **schema only** — no records. To populate a demo dataset, use the
one-click seeder in `tools/demo-data-seeder.html`.

It creates a fictional corporate group ("ADNOC" — illustrative demo data, not the real
company) spanning the account, the account plan and 17 related tables — around 180
records covering group hierarchy, financials, facilities, deposits, covenants,
stakeholders (including the reporting hierarchy that drives the org chart), coverage
team, opportunities, service cases and saved market intelligence.

**To use it**, the seeder also ships **inside the solution** as the web resource
`fsi_APDemoDataSeeder` — after import, open it from **Settings → Customisations →
Web Resources** (or browse to `/WebResources/fsi_APDemoDataSeeder`). The copy in
`tools/` is byte-identical and provided for review or standalone upload. It offers:

| Action | Behaviour |
|---|---|
| **Seed demo data** | Creates anything missing. Safe to re-run — existing records are skipped, not duplicated. |
| **Check status** | Read-only. Reports what is already present, scoped to the demo account. |
| **Remove demo data** | Deletes only the records it seeded, for that account. |

It runs entirely in the browser against the Web API using the signed-in user's
privileges, and needs no connections, flows or external services.

> Records are matched by name and scoped to the demo account, so the seeder will not
> touch data belonging to other customers in the environment.

> **The demo account is created under a fixed record id.** Two screens carried over
> from the original demonstration — the Account Plan summary and Customer 360 —
> resolve the saved market-data issuer identity from that specific account id. The
> seeder therefore recreates the account under the same id so those panels populate
> in any environment. Delete and re-seed rather than hand-creating the account.

### Market intelligence tables

The seeder populates `lseg_analysisrunv2` and `lseg_agentresultv2` with a **saved,
point-in-time snapshot** of previously retrieved market intelligence. Nothing is
fetched live: there is no external call, no API key and no LSEG connectivity in this
solution. The tabs are a presentation layer over records already stored in Dataverse.

> **Layering caveat.** These two tables are carried **inside this managed solution**.
> If you already run a separate intelligence integration that owns tables of the same
> names, import the unmanaged zip instead, or remove those two tables before importing,
> to avoid a managed-layer conflict.

---

## Repository layout

```
solution/     Importable managed and unmanaged solution zips
src/          Unpacked solution source for diffing and source control
tools/        Standalone demo data seeder (HTML web resource)
docs/images/  Screenshots
```

---

## Licence and disclaimer

Released under the [MIT Licence](LICENSE).

This is a **demonstration asset and community sample**, not a product. It is **not an
official Microsoft product**, is not endorsed or supported by Microsoft or by any bank, and
is **not production hardened**.

It is provided **"as is", without warranty of any kind**, express or implied, including but
not limited to the warranties of merchantability, fitness for a particular purpose and
non-infringement. **No support, maintenance or service-level commitment of any kind is
offered, and no support contract is available.** In no event shall the authors or copyright
holders be liable for any claim, damages or other liability arising from its use.

You are responsible for your own security, privacy, accessibility and regulatory review
before any production use. Review and test it in a non-production environment first.
