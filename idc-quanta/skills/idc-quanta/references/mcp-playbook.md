# IDC MCP Playbook

Read this file before any structured-data (QDA) work. It defines the exact MCP tool names (the `qda_qda_` chain), the discovery path, the standard query sequence, and the SHARE cross-check procedure.

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

- `search_search_data_products` — keyword spot-check for IDC data products. **It returns only a relevance-ranked subset, not the full catalog and its default `limit` is 5. Do not use it as the primary library-discovery path — use `qda_qda_list_libraries` (full catalog) and treat this tool as a spot-check only.**
- `search_lookup_entities` — resolve vendor names, technology segments, and geographies to IDC taxonomy identifiers.
- `search_search_documents` — semantic search over IDC research documents (used for analyst commentary, headwinds and tailwinds, qualitative context, MarketScape positioning, Fact Lake signals).
- `search_get_full_document` — retrieve the full body of a research document by ID.

Standard chain for a quantitative request: `search_lookup_entities` → `qda_qda_list_libraries` (full-catalog discovery; cache the result for the session) → `qda_qda_list_datasets` → `qda_qda_list_profiles` → `qda_qda_gather_context_for_dataset` → `qda_qda_list_attributes_values` → `qda_qda_execute_query`. Skip steps only when you already have the identifier the next step needs. **Resolve the most recent period before querying (recency-first for all quantitative data pulls):** at the `qda_qda_list_profiles` step, enumerate the available profiles and select the newest published profile/period for the market. Never anchor to the year named in the user's question and never default to a prior-year profile — the latest profile is authoritative. If the user asks about an earlier period, still open the latest profile, then filter to the requested period inside it. This applies to every route that pulls Tracker or Spending Guide data (market-share, comp-share, market-size, tam, it-outlook, mna-target, spend-bench, and any chained quantitative step). Use `search_search_data_products` only as a quick keyword spot-check, never as the primary discovery path — it returns only a relevance-ranked subset of the catalog, not all 89 libraries.

**SHARE cross-check:** When querying with `operationType: "SHARE"`, cross-check the top 2–3 entries against a published IDC document via `search_search_documents`. If results appear uniform or do not reconcile with published figures, present at **Low confidence** and disclose the discrepancy.

**URL extraction for live links:** Research documents (including IDC Links, Quick Takes, Vendor Profiles, and Executive Snapshots) carry a `document_url` in the `search_documents` result — capture it there and link the citation to it; `get_full_document` returns no URL, so take the link from the earlier search result. Trackers and spending guides carry their link as `library_url` in the `search_data_products` result (an idctracker.com address); the QDA data chain — `list_libraries`, `list_datasets`, `list_profiles`, `gather_context_for_dataset`, `execute_query` — returns no URL, so when you cite a tracker or spending guide whose numbers came from the QDA chain, make one `search_data_products` call for that library to capture its `library_url`. Capture every URL the moment the tool returns it; never construct or recall URLs from training data — only surface URLs that appear explicitly in a tool response.

**Title extraction follows the same rule.** Cite a document title only when the tool response carries a literal `title` field, and reproduce it verbatim. `search_documents` returns a real `title` for essentially every document — including Links, Quick Takes, Vendor Profiles, and Executive Snapshots — so cite those by their returned title, hyperlinked to the `document_url`, like any other document. `get_full_document` returns content only and no title field, so carry the title and URL from the search result and never recompose a title from the document body. Only when a document genuinely returns no title do you fall back to a container-ID-plus-descriptor citation (still attaching the `document_url` if present); never present a composed descriptor as the document's real title.
