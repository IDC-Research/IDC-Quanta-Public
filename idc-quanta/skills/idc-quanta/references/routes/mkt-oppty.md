Read this file when the dispatcher selects the idc.mkt-oppty route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.mkt-oppty

**JTBD**: Size and validate a client's market opportunity using spending forecasts and market sizing (consultant-led).

**Workflow**:

1. Capture the client's industry, geography, and technology domain.
2. Resolve the entities and align to IDC taxonomy via `search_lookup_entities`.
3. Pull IDC Spending Guide forecasts for the client's technology area by industry and geography via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`.
4. Extract Black Book TAM data for macro market framing via `qda_qda_execute_query`.
5. Cross-reference Tracker vendor share/growth data for competitive context, and surface any discrepancies across programs with confidence ranges.
6. If the opportunity exceeds a materiality threshold, develop the opportunity brief; if it is niche, flag it as such and explain why.
7. Validate the opportunity against IDC Survey buyer investment priorities via `search_search_documents`.
8. Build a 5-year projection with cited IDC data, growth rates, and assumptions.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a Strategic Brief — a concise, cited advisory write-up: opportunity framing, market size and 5-year projection, competitive context, buyer-demand validation, and a recommendation; use small tables for parallel figures. Add embedded charts only if the user explicitly asks.
