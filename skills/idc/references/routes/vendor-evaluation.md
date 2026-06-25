# Routes — Vendor Evaluation & Competitive Enablement

Read this file when the dispatcher routes to: idc.vendor-eval, idc.marketscape, idc.battlecard, idc.deal-strategy. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.vendor-eval

**JTBD**: Identify, evaluate, and shortlist technology vendors for a buyer's key initiatives.

**Triggered by**: "evaluate vendors", "vendor shortlist", "which vendors should we consider", "vendor evaluation", "shortlist for our initiative", "compare vendors for purchase", "vendor selection".

**Workflow**:

1. Capture the technology requirement and evaluation criteria, plus deployment model, industry, geography, and company-size constraints. If criteria are missing, ask once.
2. Resolve the technology category and any named vendors via `search_lookup_entities`.
3. Search IDC MarketScape rankings and evaluations for the relevant technology category and use case via `search_search_documents`, then `search_get_full_document` on the best matches.
4. Extract each vendor's capability profile, strengths, weaknesses (cautions), and analyst positioning from the MarketScape and supporting research.
5. Filter the vendor set by industry fit, geography support, deployment model, and company-size requirements.
6. If the user has specific must-have criteria, weight the analyst dimensions accordingly and re-rank.
7. Identify the vendors with the strongest IDC analyst endorsement for the buyer's specific deployment model and segment.
8. Pull current tracker market-share/growth via `qda_qda_execute_query` to validate market traction for each shortlisted vendor.
9. Produce a buy/evaluate/avoid recommendation per vendor with an IDC assessment summary and recommended next steps. Where successive MarketScape editions exist, note positioning changes over time.

**Mandatory output format**: Comparative Table — a static or sortable side-by-side matrix of shortlisted vendors against the evaluation dimensions, with color-coded scoring (RAG or Leader/Major Player/Contender), analyst commentary, market-share validation, and the buy/evaluate/avoid call, plus IDC citations. Exportable to Excel/PDF and print-friendly for embedding in selection decks.

### Route: idc.marketscape

**JTBD**: Leverage IDC MarketScape evaluations to support a client's technology vendor selection (consultant-led).

**Triggered by**: "MarketScape", "use MarketScape for vendor selection", "Leaders vs Major Players", "MarketScape-based shortlist", "vendor positioning from MarketScape", "weight client priorities against MarketScape".

**Workflow**:

1. Capture the client's technology requirement and any existing vendor shortlist.
2. Resolve the technology category and vendors via `search_lookup_entities`.
3. Retrieve the IDC MarketScape evaluation for the relevant category via `search_search_documents` and `search_get_full_document`.
4. Extract each vendor's positioning (Leaders, Major Players, Contenders) and the capability/strategy dimension scores.
5. Map vendor capabilities against the client's stated requirements.
6. If the client's priorities are known, weight the evaluation criteria accordingly and re-rank.
7. Overlay Tracker market-share and growth data via `qda_qda_execute_query` for market validation of the analyst positioning.
8. Generate a weighted vendor comparison with the MarketScape positioning and IDC citations.

**Mandatory output format**: Comparative Table — a static or sortable side-by-side matrix of vendors against weighted capability/strategy dimensions, color-coded by MarketScape tier and score, with market-share validation and IDC citations. Exportable to Excel/PDF and print-friendly for embedding in client selection presentations.

### Route: idc.battlecard

**JTBD**: Produce competitor battlecards with features, differentiators, and IDC analyst insights for product marketing.

**Triggered by**: "battlecard", "competitor battlecard", "win themes", "objection counters", "competitive differentiators", "sales enablement against competitor", "feature comparison vs competitor".

**Workflow**:

1. Capture the competitor list and product-category scope.
2. Resolve the focal product, competitors, and category via `search_lookup_entities`.
3. Pull competitor product and positioning data from IDC MarketScape, ProductScape, and MaturityScape reports via `search_search_documents` and `search_get_full_document`. If a structured evaluation exists, extract strengths/cautions/ratings per competitor; if not, synthesize positioning from research plus tracker share data.
4. Generate a feature-level comparison using the category's standard evaluation dimensions, with IDC analyst-validated differentiators and strengths/weaknesses.
5. Identify pricing and packaging deltas from IDC Tracker and Pricing Service data via `qda_qda_execute_query`.
6. Draft win/loss counters for the top competitive objections, grounded in IDC research and buyer-priority data.
7. Add an IDC-cited proof point for each differentiator claim (market share, adoption rate, analyst rating), and include analyst positioning commentary and tone on each competitor.
8. If the user requests vertical-specific variants, re-weight the evaluation criteria and regenerate. Configure an auto-refresh trigger when new IDC MarketScape and ProductScape data publishes.

**Mandatory output format**: Comparative Table — a static or sortable side-by-side battlecard matrix (Differentiator/Dimension, Our Position, Competitor Position, IDC Proof Point, Win Theme, Objection Counter) with color-coded scoring, exportable to Excel/PDF and ready to embed in sales-enablement presentations.

### Route: idc.deal-strategy

**JTBD**: Develop competitive deal strategy for strategic opportunities using IDC positioning and buyer data.

**Triggered by**: "deal strategy", "win this deal", "competitive deal", "trap-setting questions", "deal-specific battlecard", "how do we beat competitor X in this account", "competitive pricing strategy for deal".

**Workflow**:

1. Capture the deal context: target account, competitors in the deal, product scope, evaluation criteria.
2. Resolve the account and all competing vendors via `search_lookup_entities`.
3. Extract IDC MarketScape and ProductScape positioning for all vendors in the deal via `search_search_documents` and `search_get_full_document`.
4. Build a deal-specific competitive comparison matrix using IDC analyst evaluation criteria, ratings, and strengths/cautions.
5. Identify the company's IDC-validated strengths and each competitor's analyst-documented weaknesses for deal positioning.
6. Surface IDC buyer decision-factor data from Knowledge Hound via `search_search_documents` to anticipate which evaluation criteria the buyer will weight most heavily, by role.
7. Where the company demonstrably excels on an IDC-defined dimension, draft competitive trap-setting questions that steer the evaluation toward those strengths; where a competitor is weak on a dimension the buyer cares about, build an objection narrative.
8. Prepare win-theme messaging grounded in IDC analyst perspective, market validation, and peer adoption evidence, and model a competitive pricing strategy using IDC market pricing benchmarks and competitor pricing intelligence from Trackers via `qda_qda_execute_query`.

**Mandatory output format**: Workflow Playbook — a semi-interactive, step-by-step deal plan with checklists, a decision tree for objections, and embedded IDC data (positioning matrix, trap-setting questions, win themes, pricing guidance), with citations. Best used as a guided execution tool the rep works through before and during the deal.
