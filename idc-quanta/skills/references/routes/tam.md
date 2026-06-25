Read this file when the dispatcher selects the idc.tam route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.tam

**JTBD**: Size the total addressable market (TAM/SAM/SOM) for a vendor strategy team with cited IDC data.

**Workflow**:

1. Capture the market definition parameters: technology, geography, vertical, segment. If the definition does not map cleanly to IDC taxonomy, propose the closest alignment and explain boundary differences.
2. Resolve all entities via `search_lookup_entities`.
3. Query IDC spending guides (ICT SG, DX SG, Industry SGs) and Black Book for relevant market estimates via `qda_qda_list_libraries` / `qda_qda_list_datasets` (full-catalog discovery; `search_search_data_products` returns only a ranked subset — see Known MCP issues), then `qda_qda_execute_query` for TAM/SAM/SOM by market, region, product line, vertical, and segment.
4. If multiple IDC programs cover the same market, cross-validate estimates and surface discrepancies with confidence ranges.
5. Build a layered model: TAM (full market) → SAM (addressable by company capability) → SOM (realistic capture based on current share and market growth rates). State assumptions inline.
6. Identify which areas grow fastest and where the company has low share in high-growth markets.
7. If the user is setting acquisition targets, establish vertical-specific TAM estimates to frame how many targets, how big, and expected cost.
8. Generate an executive summary with cited IDC data, growth rates, and forecast horizon.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a short, cited read-out or small table of the layered TAM -> SAM -> SOM with CAGR and confidence ranges, plus the scenario assumptions and a segment/geo/deployment drill-down in text. Build a chart only if the user explicitly asks.
