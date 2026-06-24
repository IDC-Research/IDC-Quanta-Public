# Routes — Market & Competitive Share

Read this file when the dispatcher routes to: idc.market-share, idc.comp-share. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.market-share

**JTBD**: Find and present accurate market share and ranking data quickly.

**Triggered by**: "market share", "vendor share", "share rankings", "competitive ranking", "share movement", "QoQ share", "YoY share", "share leader", "share gainer", "concentration ratio".

**Workflow**:

1. Parse the query to extract technology, segment, region, and time period. If any dimension is missing, ask once before proceeding.
2. Resolve named vendors and technology terms to IDC taxonomy IDs via `search_lookup_entities`.
3. Identify the relevant tracker via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`. Use `qda_qda_list_libraries` as the primary discovery path — it returns the full catalog (89 libraries). Do not rely on `search_search_data_products`, which returns only a relevance-ranked subset and may miss the right library (see Known MCP issues).
4. Call `qda_qda_gather_context_for_dataset` to confirm dimensions, then `qda_qda_list_attributes_values` to validate filters before querying.
5. Run `qda_qda_execute_query` across all requested dimensions. If a dimension lacks coverage, identify the closest proxy and flag the approximation explicitly. If the query returns no data, stop and report the gap. **SHARE sanity check (precaution):** the SHARE operationType had a reported 100%-for-all-entries defect (see Known MCP issues; not reproduced in live testing on 2026-06-05, retained as a precaution). Cross-check the top 2–3 share entries against a published IDC document via `search_search_documents`; if a query returns uniform 100% values or figures that do not reconcile, present at Low confidence and disclose.
6. Decide whether QoQ or YoY is the more meaningful view based on market maturity and seasonal patterns. State which you chose and why in one sentence.
7. If the user requests vendor concentration analysis, calculate HHI or top-N concentration ratios from the share data. Default to top-5 concentration unless otherwise specified.
8. If the user references a client or focal company, compare its growth rate against the overall market growth to flag outperformance or underperformance.
9. Pull analyst commentary excerpts on share drivers via `search_search_documents` where relevant.

**Mandatory output format**: Comparative Table — a static or sortable side-by-side vendor-share matrix (Vendor, Segment, Region, Share %, YoY Delta, QoQ Delta, CAGR where requested, Source) with color-coded movement, exportable to Excel/PDF and ready to embed in presentations. Follow the table with an analyst-commentary block with attribution and a one-line top-line takeaway.

### Route: idc.comp-share

**JTBD**: Measure competitive market share performance for a vendor's own strategy team.

**Triggered by**: "competitive market share performance", "how are we doing vs competitors", "share performance", "competitive ranking", "win/loss trends", "ARPU vs competitors", "competitor share gains", "share KPI", "competitive scorecard".

**Workflow**:

1. Capture the focal company identity, product lines, and target market segments. If the focal company is not named, ask once before proceeding.
2. Resolve the focal company, named competitors, and technology terms to IDC taxonomy IDs via `search_lookup_entities`.
3. Identify the relevant tracker via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`. Use `qda_qda_list_libraries` as the primary discovery path — it returns the full catalog (89 libraries). Do not rely on `search_search_data_products`, which returns only a relevance-ranked subset and may miss the right library (see Known MCP issues).
4. Call `qda_qda_gather_context_for_dataset` to confirm dimensions, then `qda_qda_list_attributes_values` to validate filters before querying.
5. Run `qda_qda_execute_query` to extract current and historical market share by vendor, segment, region, and product line. Build a ranking table with YoY and QoQ share movement and CAGR. **SHARE sanity check (precaution):** the SHARE operationType had a reported 100%-for-all-entries defect (see Known MCP issues; not reproduced in live testing on 2026-06-05, retained as a precaution). Cross-check the top 2–3 share entries against a published IDC document via `search_search_documents`; if a query returns uniform 100% values or figures that do not reconcile, present at Low confidence and disclose.
6. Where IDC revenue and unit data exist, calculate the focal company's ARPU versus competitor ARPU and surface the gap.
7. Compare win/loss trends across the top 5 competitors by segment and geography. Flag any competitor whose share gain versus the focal company exceeds a materiality threshold.
8. Pull analyst commentary on share drivers and competitive dynamics via `search_search_documents` to explain the movements.
9. Compress findings into a cited executive performance summary with IDC benchmark annotations.

**Mandatory output format**: Interactive Dashboard — a multi-panel cockpit with filters (vendor, geography, time period) and drill-down charts (ranking bar chart, share-trend line, segment treemap) over the share, ARPU, and win/loss data, refreshable as new tracker editions publish. Include a one-line top-line takeaway and IDC citations on every panel. Best embedded in the strategy team's daily workflow as a live performance view.
