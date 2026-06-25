Read this file when the dispatcher selects the idc.market-size route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.market-size

**JTBD**: Quantify market size and growth opportunities by segment, vertical, and geography.

**Workflow**:

1. Parse the sizing request for technology, geography, vertical, segment, and time horizon. If horizon is missing, default to 5 years and state the assumption.
2. Resolve named entities via `search_lookup_entities`.
3. Identify which spending forecast programs cover the requested scope via `qda_qda_list_libraries` / `qda_qda_list_datasets` (the full-catalog discovery path). `search_search_data_products` returns only a ranked subset, so use it only as a spot-check (see Known MCP issues). Build the full list before querying.
4. For each relevant program, call `qda_qda_gather_context_for_dataset` and `qda_qda_list_attributes_values`, then `qda_qda_execute_query`. If multiple programs overlap (for example, Black Book and a vertical Spending Guide), cross-validate estimates and surface any discrepancies with confidence annotations (high, medium, or low confidence based on agreement across programs).
5. Build a layered model: total market → addressable segments → growth rate by segment → optional market share overlay if the user references a focal company.
6. If high-growth segments have low company share, flag them as priority opportunities with the supporting IDC data.
7. If the user requests SOM, calculate it as current share × forecast market growth × competitive dynamics adjustment. State the assumptions used.
8. Document every assumption (taxonomy choices, segment boundaries, currency, base year) inline.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a short, cited read-out — market size in the base year, forecast size at the horizon, and CAGR — with the scenario range (low/base/high), a confidence note, and an assumptions block; use a small table when several figures share structure. Build a chart only if the user explicitly asks.
