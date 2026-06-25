Read this file when the dispatcher selects the idc.vendor-eval route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.vendor-eval

**JTBD**: Identify, evaluate, and shortlist technology vendors for a buyer's key initiatives.

**Workflow**:

1. Capture the technology requirement and evaluation criteria, plus deployment model, industry, geography, and company-size constraints. If criteria are missing, ask once.
2. Resolve the technology category and any named vendors via `search_lookup_entities`.
3. Search IDC MarketScape rankings and evaluations for the relevant technology category and use case via `search_search_documents`, then `search_get_full_document` on the best matches.
4. Extract each vendor's capability profile, strengths, weaknesses (cautions), and analyst positioning from the MarketScape and supporting research.
5. Filter the vendor set by industry fit, geography support, deployment model, and company-size requirements.
6. If the user has specific must-have criteria, weight the analyst dimensions accordingly and re-rank.
7. Identify the vendors with the strongest IDC analyst endorsement for the buyer's specific deployment model and segment.
8. Pull current tracker market-share/growth via `qda_qda_execute_query` to validate market traction for each shortlisted vendor. **SHARE sanity check (precaution):** the SHARE operationType had a reported 100%-for-all-entries defect (see Known MCP issues; not reproduced in live testing on 2026-06-05, retained as a precaution). Cross-check the top 2–3 share entries against a published IDC document via `search_search_documents`; if a query returns uniform 100% values or figures that do not reconcile, present at Low confidence and disclose.
9. Produce a buy/evaluate/avoid recommendation per vendor with an IDC assessment summary and recommended next steps. Where successive MarketScape editions exist, note positioning changes over time.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Comparative Table — a static or sortable side-by-side matrix of shortlisted vendors against the evaluation dimensions, with color-coded scoring (RAG or Leader/Major Player/Contender), analyst commentary, market-share validation, and the buy/evaluate/avoid call, plus IDC citations. Exportable to Excel/PDF and print-friendly for embedding in selection decks.
