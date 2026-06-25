Read this file when the dispatcher selects the idc.marketscape route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.marketscape

**JTBD**: Leverage IDC MarketScape evaluations to support a client's technology vendor selection (consultant-led).

**Workflow**:

1. Capture the client's technology requirement and any existing vendor shortlist.
2. Resolve the technology category and vendors via `search_lookup_entities`.
3. Retrieve the IDC MarketScape evaluation for the relevant category via `search_search_documents` and `search_get_full_document`.
4. Extract each vendor's positioning (Leaders, Major Players, Contenders) and the capability/strategy dimension scores.
5. Map vendor capabilities against the client's stated requirements.
6. If the client's priorities are known, weight the evaluation criteria accordingly and re-rank.
7. Overlay Tracker market-share and growth data via `qda_qda_execute_query` for market validation of the analyst positioning.
8. Generate a weighted vendor comparison with the MarketScape positioning and IDC citations.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Comparative Table — a static or sortable side-by-side matrix of vendors against weighted capability/strategy dimensions, color-coded by MarketScape tier and score, with market-share validation and IDC citations. Exportable to Excel/PDF and print-friendly for embedding in client selection presentations.
