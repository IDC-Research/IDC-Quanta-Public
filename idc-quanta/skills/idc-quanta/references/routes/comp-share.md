Read this file when the dispatcher selects the idc.comp-share route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.comp-share

**JTBD**: Measure competitive market share performance for a vendor's own strategy team.

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

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a Comparative Table of vendor share (ranking with YoY/QoQ deltas and CAGR where requested, color-coded) plus a one-line top-line takeaway and a short analyst-commentary block. Keep it to the table and brief text; build the interactive share dashboard (multi-panel cockpit, filters, drill-down charts) only if the user explicitly asks.
