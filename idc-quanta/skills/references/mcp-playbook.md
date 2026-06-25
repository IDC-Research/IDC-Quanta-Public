# IDC MCP Playbook

Read this file before any structured-data (QDA) work. It defines the exact MCP tool names (the `qda_qda_` chain), the discovery path, the standard query sequence, and the known connector issues with their required mitigations.

## IDC MCP tool reference

The IDC MCP connector exposes two tool families. Use these exact names. They are not aliases.

**QDA family (structured data):**

- `qda_qda_list_libraries` — list IDC tracker and spending guide libraries available to the user.
- `qda_qda_list_datasets` — list datasets within a library.
- `qda_qda_list_profiles` — list profiles within a dataset (quarterly vs annual, regional cuts).
- `qda_qda_gather_context_for_dataset` — return the schema, dimensions, and metadata for a dataset before querying it.
- `qda_qda_list_attributes_values` — return the valid values for a given attribute (used to build filters).
- `qda_qda_execute_query` — execute the query and return the rows.

**Search family (research documents and entities):**

- `search_search_data_products` — keyword spot-check for IDC data products. **It returns only a relevance-ranked subset, not the full catalog (a broad query at `limit: 50` returned 38–43 of the connector's 89 libraries in testing on 2026-06-05), and its default `limit` is 5. Do not use it as the primary library-discovery path — use `qda_qda_list_libraries` (full catalog) and treat this tool as a spot-check only. See Known MCP issues.**
- `search_lookup_entities` — resolve vendor names, technology segments, and geographies to IDC taxonomy identifiers.
- `search_search_documents` — semantic search over IDC research documents (used for analyst commentary, headwinds and tailwinds, qualitative context, MarketScape positioning, Fact Lake signals).
- `search_get_full_document` — retrieve the full body of a research document by ID.

Standard chain for a quantitative request: `search_lookup_entities` → `qda_qda_list_libraries` (full-catalog discovery; cache the result for the session) → `qda_qda_list_datasets` → `qda_qda_list_profiles` → `qda_qda_gather_context_for_dataset` → `qda_qda_list_attributes_values` → `qda_qda_execute_query`. Skip steps only when you already have the identifier the next step needs. **Resolve the most recent period before querying (recency-first for all quantitative data pulls):** at the `qda_qda_list_profiles` step, enumerate the available profiles and select the newest published profile/period for the market. Never anchor to the year named in the user's question and never default to a prior-year profile — the latest profile is authoritative. If the user asks about an earlier period, still open the latest profile, then filter to the requested period inside it. This applies to every route that pulls Tracker or Spending Guide data (market-share, comp-share, market-size, tam, it-outlook, mna-target, spend-bench, and any chained quantitative step). Use `search_search_data_products` only as a quick keyword spot-check, never as the primary discovery path — it returns only a relevance-ranked subset of the catalog, not all 89 libraries (see Known MCP issues).

**URL extraction for live links:** Research documents (including IDC Links, Quick Takes, Vendor Profiles, and Executive Snapshots) carry a `document_url` in the `search_documents` result — capture it there and link the citation to it; `get_full_document` returns no URL, so take the link from the earlier search result. Trackers and spending guides carry their link as `library_url` in the `search_data_products` result (an idctracker.com address); the QDA data chain — `list_libraries`, `list_datasets`, `list_profiles`, `gather_context_for_dataset`, `execute_query` — returns no URL, so when you cite a tracker or spending guide whose numbers came from the QDA chain, make one `search_data_products` call for that library to capture its `library_url`. Capture every URL the moment the tool returns it; never construct or recall URLs from training data — only surface URLs that appear explicitly in a tool response.

**Title extraction follows the same rule.** Cite a document title only when the tool response carries a literal `title` field, and reproduce it verbatim. `search_documents` returns a real `title` for essentially every document — including Links, Quick Takes, Vendor Profiles, and Executive Snapshots — so cite those by their returned title, hyperlinked to the `document_url`, like any other document. `get_full_document` returns content only and no title field, so carry the title and URL from the search result and never recompose a title from the document body. Only when a document genuinely returns no title do you fall back to a container-ID-plus-descriptor citation (still attaching the `document_url` if present); never present a composed descriptor as the document's real title.

## Known MCP issues and required mitigations

This section logs MCP connector defects reported by IDC engineering alongside the result of live verification. Re-check it against the live connector before each release, because the underlying API changes. Status below reflects testing on 2026-06-05 against the production connector under a standard analyst entitlement.

**`search_search_data_products` does not return the full catalog.** Reported by J. Bradley (May 2026) as a hard cap of 12 results regardless of query. Live testing on 2026-06-05 did not reproduce a hard cap: two broad queries at `limit: 50` returned 38 and 43 products, and the result set varied with the query. The tool still returns only a relevance-ranked subset, not the full catalog — `qda_qda_list_libraries` returned 89 libraries the same day — its default `limit` is 5, and a poorly phrased query can miss the right library. Guidance: use `qda_qda_list_libraries` as the primary discovery path (full catalog; cache it for the session) and treat `search_search_data_products` only as a keyword spot-check. The original 12-result figure appears fixed or limit-dependent; confirm with J. Bradley before relying on either reading.

**SHARE operationType — reported 100%-for-all-entries defect.** Reported by J. Bradley (May 2026): `qda_qda_execute_query` with `operationType: "SHARE"` could return 100% for every row instead of the true split, most often on category-spend allocation. Live testing on 2026-06-05 did not reproduce it: a Tracker vendor-share query (Ethernet Switch, 2024 worldwide) returned differentiated, reconcilable shares (Cisco 34.3%, Arista 13.0%, Huawei 10.0%), and a Spending Guide category-allocation query (AI and GenAI SG, 2024 worldwide) returned correct shares (Server 41.3%, IT Services 12.1%). The defect appears fixed or intermittent. Because it was reported as intermittent and only two query shapes were tested, the cross-check below is retained as a precaution rather than removed. Routes that present share rankings — `idc.market-share`, `idc.comp-share`, `idc.mna-target`, `idc.vendor-eval`, and any other route that pulls a SHARE figure — should still sanity-check the top 2–3 entries against a published IDC document via `search_search_documents`; if a query returns uniform 100% values or figures that do not reconcile, present at **Low confidence**, disclose the issue, and do not present a single-source SHARE result as fact. Confirm current status with J. Bradley before relaxing the cross-check.
