# Routes — Market Sizing & Forecast

Read this file when the dispatcher routes to: idc.market-size, idc.tam, idc.it-outlook. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.market-size

**JTBD**: Quantify market size and growth opportunities by segment, vertical, and geography.

**Triggered by**: "market size", "market growth", "growth forecast by segment", "addressable market by vertical", "market sizing", "market opportunity by geography", "high-growth segments".

**Workflow**:

1. Parse the sizing request for technology, geography, vertical, segment, and time horizon. If horizon is missing, default to 5 years and state the assumption.
2. Resolve named entities via `search_lookup_entities`.
3. Identify which spending forecast programs cover the requested scope via `qda_qda_list_libraries` / `qda_qda_list_datasets` (the full-catalog discovery path). `search_search_data_products` returns only a ranked subset, so use it only as a spot-check (see Known MCP issues). Build the full list before querying.
4. For each relevant program, call `qda_qda_gather_context_for_dataset` and `qda_qda_list_attributes_values`, then `qda_qda_execute_query`. If multiple programs overlap (for example, Black Book and a vertical Spending Guide), cross-validate estimates and surface any discrepancies with confidence annotations (high, medium, or low confidence based on agreement across programs).
5. Build a layered model: total market → addressable segments → growth rate by segment → optional market share overlay if the user references a focal company.
6. If high-growth segments have low company share, flag them as priority opportunities with the supporting IDC data.
7. If the user requests SOM, calculate it as current share × forecast market growth × competitive dynamics adjustment. State the assumptions used.
8. Document every assumption (taxonomy choices, segment boundaries, currency, base year) inline.

**Mandatory output format**: Visual Chart — a single-insight bar, line, or area chart with IDC branding showing market size in base year, forecast size at horizon, and CAGR, exportable as high-res PNG/SVG for slides, reports, or executive briefs. Accompany the chart with the scenario range (low/base/high), confidence annotation per estimate, an assumptions block, and ranked priority opportunities where applicable.

### Route: idc.tam

**JTBD**: Size the total addressable market (TAM/SAM/SOM) for a vendor strategy team with cited IDC data.

**Triggered by**: "TAM", "SAM", "SOM", "addressable market", "size the opportunity", "acquisition target sizing", "how big is this market for us", "layered market model".

**Workflow**:

1. Capture the market definition parameters: technology, geography, vertical, segment. If the definition does not map cleanly to IDC taxonomy, propose the closest alignment and explain boundary differences.
2. Resolve all entities via `search_lookup_entities`.
3. Query IDC spending guides (ICT SG, DX SG, Industry SGs) and Black Book for relevant market estimates via `qda_qda_list_libraries` / `qda_qda_list_datasets` (full-catalog discovery; `search_search_data_products` returns only a ranked subset — see Known MCP issues), then `qda_qda_execute_query` for TAM/SAM/SOM by market, region, product line, vertical, and segment.
4. If multiple IDC programs cover the same market, cross-validate estimates and surface discrepancies with confidence ranges.
5. Build a layered model: TAM (full market) → SAM (addressable by company capability) → SOM (realistic capture based on current share and market growth rates). State assumptions inline.
6. Identify which areas grow fastest and where the company has low share in high-growth markets.
7. If the user is setting acquisition targets, establish vertical-specific TAM estimates to frame how many targets, how big, and expected cost.
8. Generate an executive summary with cited IDC data, growth rates, and forecast horizon.

**Mandatory output format**: Visual Chart — a single-insight bar/line/area chart with IDC branding showing the layered TAM → SAM → SOM with CAGR and confidence ranges, exportable as high-res PNG/SVG for slides, reports, or executive briefs. Accompany with the scenario assumptions and the drill-down model by segment/geo/deployment.

### Route: idc.it-outlook

**JTBD**: Understand short- and long-term IT spending outlook for planning and forecasting.

**Triggered by**: "IT spending outlook", "spend forecast", "Black Book", "headwinds and tailwinds", "1-year outlook", "3-year outlook", "5-year outlook", "spend revision", "macro IT spending".

**Workflow**:

1. Parse the outlook request for technology, geography, and time horizon. Default to 1, 3, and 5-year horizons if unspecified.
2. Resolve named entities via `search_lookup_entities` where needed.
3. Identify the right library via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`, then `qda_qda_gather_context_for_dataset` to confirm structure.
4. Call `qda_qda_execute_query` against Black Book and ICT Spending Guide profiles to pull forecasts at the requested horizons, segmented by technology category (cloud, security, AI, infrastructure), region, and customer type.
5. If multiple Black Book editions exist for the same scope, query the prior edition as well and compare. Flag any forecast revision greater than ±2 percentage points of growth.
6. Identify headwinds (economic, regulatory, competitive) and tailwinds (secular trends, stimulus) via `search_search_documents` and `search_get_full_document` on the relevant analyst narrative.
7. If the user's company operates in a specific segment, benchmark the company's internal revenue forecast against the market growth rate for that segment.
8. Recommend a refresh cadence tied to the next Black Book edition release.

**Mandatory output format**: Visual Chart — a single-insight bar/line/area chart with IDC branding showing the headline forecast and horizon trajectory, exportable as high-res PNG/SVG for slides and executive briefs. Accompany with segmentation by tech/region/customer type, headwinds, tailwinds, edition-over-edition revisions where applicable, data gaps, recommended next steps, and a "Refresh when" line.
