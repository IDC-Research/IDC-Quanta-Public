# IDC Data Landscape

Read this file before the first QDA query on any market you have not queried yet this session, and whenever a user asks "what data do you have", "what markets does IDC cover", or "Tracker or Spending Guide?". It maps IDC's supply-side and demand-side data products to their library IDs so you pick the right library before calling the QDA chain, instead of discovering blind.

Library IDs below were verified against the live IDC connector on 2026-06-13 via `qda_qda_list_libraries` (89 libraries returned). IDs are stable but can change; call `qda_qda_list_libraries` once per session and treat its output as authoritative if it disagrees with this file.

## How IDC data is organized

IDC produces two kinds of content, and this connector reaches both.

- **Quantitative data (QDA)** - structured, queryable numbers: market size, vendor revenue and share, growth, CAGR, shipments, spend. Organized as Library -> Dataset -> Profile -> Query. Resolve a profile before you query.
- **Qualitative research (Search)** - narrative documents: MarketScapes, Market Perspectives, Forecasts, FutureScapes, surveys. Retrieved and read for context, positioning, and analyst judgment.

Most strong answers use both: QDA for the cited number, Search for the "why".

## Choosing the data source: Tracker vs Spending Guide

This is the most common source-selection error, so decide deliberately.

- **Tracker = supply side (what vendors sell).** Use for a technology product market: market size, vendor revenue, vendor share, unit shipments, growth of a product market. "How big is the cloud IaaS market?", "who leads in security products?", "what is the CAGR for AI software?"
- **IT Spending Guide = demand side (what buyers spend).** Use for IT budgets and spend allocation by industry, vertical, or category. "How much is the banking industry spending on AI?" is a Spending Guide question, not a Tracker question. "What share of retail IT spend goes to cloud?", "what is the DX spend CAGR for healthcare?"

Quick test: if the subject is a vendor or a product market, reach for a Tracker. If the subject is an industry's or buyer segment's budget, reach for a Spending Guide. Many questions want both - size the market with a Tracker, then show who is buying with a Spending Guide.

## Tracker libraries (supply side) - confirmed IDs

| Market area | Library | ID |
|---|---|---|
| Cross-market IT spend forecast | Worldwide Black Book Live Edition | 96 |
| Public cloud (IaaS/PaaS/SaaS) | IDC Semiannual Public Cloud Services Tracker | 26 |
| Server, storage, datacenter infrastructure | IDC Quarterly Enterprise Infrastructure Tracker | 70 |
| AI software | IDC Semiannual Core AI Software Tracker | 310 |
| AI infrastructure (hardware) | IDC Quarterly Artificial Intelligence Infrastructure Tracker | 59 |
| Security products | IDC Semiannual Security Products Tracker | 78 |
| Software (broad) | IDC Semiannual Software Tracker | 25 |
| IT services | IDC Semiannual Services Tracker | 22 |
| Telecom services | IDC Semiannual Telecom Services Tracker | 53 |
| Networking | IDC Quarterly Network Infrastructure Tracker | 119 |
| PCs and client devices | IDC Quarterly Personal Computing Device Tracker | 38 |
| Phones | IDC Quarterly Mobile Phone Tracker | 28 |
| Wearables | IDC Quarterly Wearable Device Tracker | 21 |
| Datacenter facilities | IDC Semiannual Datacenter Facilities Tracker | 409 |
| Robotics | IDC Annual Robotics Trackers / IDC Quarterly Robotics Trackers | 277 / 343 |

"Black Book" referenced in the routes is library 96 (Worldwide Black Book Live Edition), IDC's cross-market IT and ICT spend forecast. Use it for the it-outlook route and any broad multi-segment IT spend outlook. For markets not listed above, call `qda_qda_list_libraries` for the full catalog.

## IT Spending Guide libraries (demand side) - confirmed IDs

All Spending Guide data is at industry, segment, or category level. None of it is named-account or company-specific.

| Spending Guide | ID | Use for |
|---|---|---|
| Worldwide ICT Spending Guide Enterprise and SMB by Industry | 90 | Broad industry IT spend by enterprise/SMB segment |
| Worldwide Software and Public Cloud Services Spending Guide | 74 | Software and cloud category spend |
| Worldwide AI and Generative AI Spending Guide | 91 | AI/GenAI investment by category and industry |
| Worldwide Agentic AI Adoption Guide | 376 | Agentic AI adoption and investment |
| Worldwide Digital Transformation Spending Guide | 56 | DX spend across technology pillars |
| Worldwide Security Spending Guide | 105 | Cybersecurity spend by category and industry |
| Worldwide Data and Analytics Spending Guide | 94 | Data and analytics investment |
| Worldwide IT Spending Guide Line of Business | 103 | IT spend by business function (HR, Finance, etc.) |
| Worldwide Augmented and Virtual Reality Spending Guide | 92 | Immersive technology investment |
| Worldwide Edge Spending Guide | 108 | Edge computing spend |
| Worldwide Internet of Things Spending Guide | 102 | IoT investment by segment |
| Worldwide Banking IT Spending Guide | 93 | Banking vertical IT spend |
| Worldwide Capital Markets IT Spending Guide | 99 | Capital markets vertical IT spend |
| Worldwide Insurance IT Spending Guide | 101 | Insurance vertical IT spend |
| Worldwide Retail Industry IT Spending Guide | 57 | Retail vertical IT spend |
| US Federal Government IT Spending Guide | 61 | Federal agency IT spend |
| US State and Local Government IT Spending Guide | 60 | State/local government IT spend |
| Worldwide 3rd Platform Spending Guide: Government | 82 | Government 3rd-platform spend |
| Worldwide 3rd Platform Spending Guide: Healthcare | 83 | Healthcare 3rd-platform spend |

Note: Joe's draft listed a "Robotics and Drones Spending Guide (104)". That library is not in the current live catalog, so it is omitted here; use the Robotics Trackers (277/343) for robotics, or confirm via `qda_qda_list_libraries`.

## Question-to-library routing matrix

| Question shape | Source | Notes |
|---|---|---|
| "How big is the X market?" | Tracker (SUM) | Filter by year and geography |
| "CAGR / forecast for X?" | Tracker (CAGR) | Use a Forecast dataset to the horizon year |
| "Who leads / share in X?" | Tracker (SHARE) | Apply the SHARE cross-check in mcp-playbook.md |
| "How does [vendor] compare to the market?" | Tracker | Vendor revenue plus total market in one query |
| "How much is [industry] spending on [tech]?" | Spending Guide | Pick the vertical or category guide |
| "IT budget CAGR for [vertical]?" | Spending Guide (CAGR) | Vertical-specific guide |
| "What share of IT spend goes to [category]?" | Spending Guide (SHARE) | Allocation; apply SHARE cross-check |
| "IDC's view / take on X trend or vendor?" | Search documents | Narrative, not structured figures |
| "Is there a MarketScape / report on X?" | Search documents | Filter by content group |
| "What libraries/trackers cover X?" | `qda_qda_list_libraries` | Full catalog; more reliable than search_search_data_products |

## No-Tracker fallback

If neither a Tracker nor a Spending Guide covers the market (for example an emerging category like quantum computing):

1. Confirm with `qda_qda_list_libraries` that nothing matches.
2. Pivot to `search_search_documents` for narrative market sizing from IDC research.
3. Extract the figure from the document, cite the document and date, and present at **Medium** confidence.
4. Record the gap in the answer: "No IDC Tracker or Spending Guide covers this market; the figure is from IDC research."

Never fill a no-Tracker gap with training-data numbers.

## What this connector does NOT cover

| Not available | Handling |
|---|---|
| Named-account / company-level spend (wallet data) | Offer the closest industry or cohort Spending Guide benchmark, flagged as an estimate |
| Live / raw real-time signal feeds (live earnings, patent filings, VC activity) | `idc.signal-scan` and `idc.comp-strategy` work over IDC research and analyst coverage via Search; flag that output reflects IDC's latest coverage, not breaking web news |
| Vendor pricing / IDC Pricing Service (TCO benchmarks) | Use Tracker revenue plus positioning research instead |

This matches the user-facing limits in `references/capabilities.md`. Apply the matching failure mode rather than substituting outside data.

## Discovery note

`search_search_data_products` returns only a relevance-ranked subset of the catalog (not a hard cap, per live testing on 2026-06-05; see `references/mcp-playbook.md`), so it can miss the right library. Use `qda_qda_list_libraries` as the primary discovery path and cache its result for the session; use `search_search_data_products` only as a keyword spot-check. A library's live link is the `library_url` returned by `search_data_products` (an idctracker.com address); the QDA chain returns no URL, so call `search_data_products` for a library when you need its link for a citation.

## Scope and vintage caveats

- A QDA figure and a published-document figure for the "same" market can differ because of scope definitions (deployment categories, vendor inclusion), not error. Note it; do not treat one as wrong.
- Trackers release on a quarterly or semiannual cadence. When you compare a QDA figure to a document figure, note both release dates.
