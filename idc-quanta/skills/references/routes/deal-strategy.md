Read this file when the dispatcher selects the idc.deal-strategy route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.deal-strategy

**JTBD**: Develop competitive deal strategy for strategic opportunities using IDC positioning and buyer data.

**Workflow**:

1. Capture the deal context: target account, competitors in the deal, product scope, evaluation criteria.
2. Resolve the account and all competing vendors via `search_lookup_entities`.
3. Extract IDC MarketScape and ProductScape positioning for all vendors in the deal via `search_search_documents` and `search_get_full_document`.
4. Build a deal-specific competitive comparison matrix using IDC analyst evaluation criteria, ratings, and strengths/cautions.
5. Identify the company's IDC-validated strengths and each competitor's analyst-documented weaknesses for deal positioning.
6. Surface IDC buyer decision-factor data from Knowledge Hound via `search_search_documents` to anticipate which evaluation criteria the buyer will weight most heavily, by role.
7. Where the company demonstrably excels on an IDC-defined dimension, draft competitive trap-setting questions that steer the evaluation toward those strengths; where a competitor is weak on a dimension the buyer cares about, build an objection narrative.
8. Prepare win-theme messaging grounded in IDC analyst perspective, market validation, and peer adoption evidence, and model a competitive pricing strategy using IDC market pricing benchmarks and competitor pricing intelligence from Trackers via `qda_qda_execute_query`.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a structured, cited deal plan in text and tables — positioning matrix, trap-setting questions, win themes, objection counters, and pricing guidance. Build the semi-interactive playbook (checklists, decision tree) only if the user explicitly asks.
