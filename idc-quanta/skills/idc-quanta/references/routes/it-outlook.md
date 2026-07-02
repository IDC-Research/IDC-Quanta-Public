Read this file when the dispatcher selects the idc.it-outlook route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.it-outlook

**JTBD**: Understand short- and long-term IT spending outlook for planning and forecasting.

**Workflow**:

1. Parse the outlook request for technology, geography, and time horizon. Default to 1, 3, and 5-year horizons if unspecified.
2. Resolve named entities via `search_lookup_entities` where needed.
3. Identify the right library via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`, then `qda_qda_gather_context_for_dataset` to confirm structure.
4. Call `qda_qda_execute_query` against Black Book and ICT Spending Guide profiles to pull forecasts at the requested horizons, segmented by technology category (cloud, security, AI, infrastructure), region, and customer type.
5. If multiple Black Book editions exist for the same scope, query the prior edition as well and compare. Flag any forecast revision greater than ±2 percentage points of growth.
6. Identify headwinds (economic, regulatory, competitive) and tailwinds (secular trends, stimulus) via `search_search_documents` and `search_get_full_document` on the relevant analyst narrative.
7. If the user's company operates in a specific segment, benchmark the company's internal revenue forecast against the market growth rate for that segment.
8. Recommend a refresh cadence tied to the next Black Book edition release.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a short, cited read-out (or small table) of the forecast at the requested horizons, with segmentation by tech/region/customer type, headwinds and tailwinds, edition-over-edition revisions where applicable, data gaps, and a "Refresh when" line. Build a chart only if the user explicitly asks.
