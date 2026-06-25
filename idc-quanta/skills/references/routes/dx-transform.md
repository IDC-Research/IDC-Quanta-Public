Read this file when the dispatcher selects the idc.dx-transform route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.dx-transform

**JTBD**: Identify technology-driven transformation opportunities for a client using DX use-case forecasts (consultant-led).

**Workflow**:

1. Capture the client's industry and transformation priorities.
2. Resolve the industry, use cases, and technology domains via `search_lookup_entities`.
3. Pull DX and AI Spending Guide forecasts by use case via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`.
4. Extract AI Spending Guide detail for AI-specific transformation opportunities via `qda_qda_execute_query`.
5. Cross-reference Technographics for the client's current technology gaps via `search_search_documents`.
6. Rank opportunities by growth rate × addressable market.
7. If the client has technology gaps, recommend build/buy/partner for each priority opportunity.
8. Validate with peer adoption patterns and IDC survey data via `search_search_documents`.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a Comparative Table — the weighted scoring matrix of transformation opportunities (Use Case, Growth Rate, Addressable Market, Client Gap, Weighted Score, RAG indicator, Build/Buy/Partner), with IDC citations. Build the interactive or sortable scorecard only if the user explicitly asks.
