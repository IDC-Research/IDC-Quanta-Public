Read this file when the dispatcher selects the idc.battlecard route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.battlecard

**JTBD**: Produce competitor battlecards with features, differentiators, and IDC analyst insights for product marketing.

**Workflow**:

1. Capture the competitor list and product-category scope.
2. Resolve the focal product, competitors, and category via `search_lookup_entities`.
3. Pull competitor product and positioning data from IDC MarketScape, ProductScape, and MaturityScape reports via `search_search_documents` and `search_get_full_document`. If a structured evaluation exists, extract strengths/cautions/ratings per competitor; if not, synthesize positioning from research plus tracker share data.
4. Generate a feature-level comparison using the category's standard evaluation dimensions, with IDC analyst-validated differentiators and strengths/weaknesses.
5. Identify pricing and packaging deltas from IDC Tracker and Pricing Service data via `qda_qda_execute_query`. If pricing data is not in the connector, omit the pricing row, note that pricing is not covered, and build the battlecard from MarketScape/ProductScape positioning and tracker share instead.
6. Draft win/loss counters for the top competitive objections, grounded in IDC research and buyer-priority data.
7. Add an IDC-cited proof point for each differentiator claim (market share, adoption rate, analyst rating), and include analyst positioning commentary and tone on each competitor.
8. If the user requests vertical-specific variants, re-weight the evaluation criteria and regenerate. Configure an auto-refresh trigger when new IDC MarketScape and ProductScape data publishes.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Comparative Table — a static or sortable side-by-side battlecard matrix (Differentiator/Dimension, Our Position, Competitor Position, IDC Proof Point, Win Theme, Objection Counter) with color-coded scoring, exportable to Excel/PDF and ready to embed in sales-enablement presentations.
