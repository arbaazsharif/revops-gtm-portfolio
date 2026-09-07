# Semiconductor Manufacturing Outbound Intelligence Pipeline

Built in Clay to demonstrate account-level buying-signal scoring and outreach personalization, targeting the kind of complex-facility buyer I already prospect in my BDR role.

## ICP

Semiconductor Manufacturing companies, 201–1,000 employees, with operations in the US or Canada. This mirrors the real buyer profile for enterprise facilities/maintenance software, complex sites with genuine maintenance budgets.

## Pipeline (in order)

1. **Source** — Clay's Find Companies tool, filtered by Industry (Semiconductor Manufacturing), Company Size (201–500, 501–1,000), and Location (at least one location in Canada/US). Capped at 25 results to control credit usage; manually reviewed down to 17 after catching the data nuance below.
2. **Enrich** — Ran Clay's "Enrich company" and "Job Openings" enrichments to pull refreshed firmographic data, domain, and hiring-signal counts.
3. **Ground with real content** — Added a "Scrape website" step (0.1 credits/row) so the AI steps downstream work from real scraped page content instead of open-ended web browsing. More reliable, and cheaper.
4. **Score** — "Lead Temperature" column, a plain rule-based formula, not AI, zero credit cost: Hot = 501–1,000 employees, Warm = 201–500. Deliberately simple and fully explainable rather than an opaque AI score for a signal this basic.
5. **Personalize** — "Outreach Hook" Claygent column. Reads each company's Description and writes one natural first-line opener referencing a real, specific fact. Explicit fallback: outputs exactly `NO_SIGNAL` when nothing specific is found, rather than inventing a generic line. 1 of 16 rows correctly hit this fallback.
6. **Export** — Copied into Google Sheets, downloaded as CSV.

## Data-quality issues I caught, and why they matter

- **Location filter nuance**: an "at least one location matches" filter is not the same as "headquartered in." Several companies with a genuine US/Canada office but a different global HQ showed up in results. I manually reviewed and removed ambiguous rows rather than trust the filter blindly.
- **Scraped emails were unreliable**: generic "info@" inboxes and even one false positive, a JavaScript library version string mistaken for an email because it contained an "@", showed up in scraped contact data. This is exactly why person-level contact-finding needs a dedicated people-search step, not text pattern-matching, a lesson I'd carry directly into a real client build.
- **One row failed enrichment outright** (Navitas Semiconductor). Left visible as an error rather than silently dropped or faked.

## What this is not

This is account-level scoring, not contact-level outreach. It ranks companies by buying-signal strength; finding the named Facilities Manager or Director at each one would be the natural next phase, intentionally out of scope here given free-tier credit limits.

## Cost

17 rows, roughly 1–3 credits per row across enrichment and AI steps depending on the column. Comfortably inside Clay's free-tier credit allowance.

## What I'd do differently at scale

Lock a verified domain as the anchor before running any name-based AI enrichment, to avoid entity-mismatch risk. Add a dedicated people-search step scoped to verified domains rather than relying on scraped page text for contacts.
