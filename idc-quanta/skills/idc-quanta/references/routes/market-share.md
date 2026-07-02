Read this file when the dispatcher selects the idc.market-share route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.market-share

**JTBD**: Find and present accurate market share and ranking data quickly.

**Workflow**:

1. Parse the query to extract technology, segment, region, and time period. If any dimension is missing, ask once before proceeding.
2. Resolve named vendors and technology terms to IDC taxonomy IDs via `search_lookup_entities`.
3. Identify the relevant tracker via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`. Use `qda_qda_list_libraries` as the primary discovery path — it returns the full catalog (89 libraries). Do not rely on `search_search_data_products`, which returns only a relevance-ranked subset and may miss the right library (see Known MCP issues). At `qda_qda_list_profiles`, enumerate profiles and select the MOST RECENT published profile/period — do not anchor to a prior year; query the latest profile, then filter to the period the user asked for (see the recency rule in `references/mcp-playbook.md`).
4. Call `qda_qda_gather_context_for_dataset` to confirm dimensions, then `qda_qda_list_attributes_values` to validate filters before querying.
5. Run `qda_qda_execute_query` across all requested dimensions. If a dimension lacks coverage, identify the closest proxy and flag the approximation explicitly. If the query returns no data, stop and report the gap. **SHARE sanity check (precaution):** the SHARE operationType had a reported 100%-for-all-entries defect (see Known MCP issues; not reproduced in live testing on 2026-06-05, retained as a precaution). Cross-check the top 2–3 share entries against a published IDC document via `search_search_documents`; if a query returns uniform 100% values or figures that do not reconcile, present at Low confidence and disclose.
6. Decide whether QoQ or YoY is the more meaningful view based on market maturity and seasonal patterns. State which you chose and why in one sentence.
7. If the user requests vendor concentration analysis, calculate HHI or top-N concentration ratios from the share data. Default to top-5 concentration unless otherwise specified.
8. If the user references a client or focal company, compare its growth rate against the overall market growth to flag outperformance or underperformance.
9. Pull analyst commentary excerpts on share drivers via `search_search_documents` where relevant.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Comparative Table — a static or sortable side-by-side vendor-share matrix (Vendor, Segment, Region, Share %, YoY Delta, QoQ Delta, CAGR where requested, Source) with color-coded movement, exportable to Excel/PDF and ready to embed in presentations. Follow the table with an analyst-commentary block with attribution and a one-line top-line takeaway.
