# Routes — Executive & Consultant Advisory

Read this file when the dispatcher routes to: idc.exec-intel, idc.strategy-rec, idc.mkt-oppty, idc.dx-transform. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.exec-intel

**JTBD**: Supply market intelligence to CFOs and CxOs to validate budgets, M&As, and strategic investments.

**Triggered by**: "executive briefing", "CFO briefing", "board briefing", "M&A market context", "investment thesis", "budget justification", "strategic investment", "due diligence", "acquisition pricing".

**Workflow**:

1. Identify the executive decision type: investment thesis validation, M&A market context, or budget justification. Each routes to a different evidence stack.
2. Pull the data assets relevant to the decision type. For all three, start with `search_lookup_entities` to resolve the focal market and entities, then `qda_qda_list_libraries` to identify the right libraries (full-catalog discovery; `search_search_data_products` returns only a ranked subset and should not be the primary discovery path).
   - Investment thesis: run `idc.tam` (or `idc.market-size`) and a competitive intensity pass from `idc.comp-share`.
   - M&A: add startup activity and funding signals from Fact Lake via `search_search_documents`, and competitor financial benchmarks via `qda_qda_execute_query` against the relevant tracker.
   - Budget justification: add a peer benchmark comparison via `idc.spend-bench` (or `idc.acct-spending`) and market opportunity sizing via `idc.market-size`.
3. Model financial impact scenarios (including the revenue impact of competitor share shifts) using IDC forecast data and competitive dynamics. State the assumptions inline.
4. If the request is for board-level distribution, restructure into executive format: 1-page summary, analyst-cited evidence appendix, and an explicit risk assessment section.
5. Validate data currency: every cited number must be from the most recent published IDC edition. Flag any data point older than two editions. Note: although `idc.exec-intel` is a relevancy-first route per Rule 7, all quantitative data pulls inside this route are recency-first per the data-pull clarifier in Rule 7. Relevancy-first only governs framing, taxonomy, and analyst commentary selection.
6. Validate citation accuracy: every claim in the executive summary must map to a citation in the appendix. Render the mapping as a visible table in board-level outputs.
7. If the user requests it, propose a calibration follow-up — track the decision outcome against the market data so future briefings improve.

**Mandatory output format**: Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes, every claim cited. Structure as Executive Summary (three to five bullets), Evidence Appendix (tables and analyst excerpts grouped by data product), and Risk Assessment (market, competitive, execution risks each with an IDC data point). For M&A, add Target Market Activity (Fact Lake); for budget justification, add Peer Benchmark. Best delivered as an email-ready brief for a C-suite or board audience.

### Route: idc.strategy-rec

**JTBD**: Build data-backed strategy recommendations grounded in vendor share and market dynamics (consultant-led).

**Triggered by**: "strategy recommendation", "what should our client do", "data-backed recommendation", "strategic recommendation grounded in market data", "market landscape recommendation", "competitive positioning recommendation".

**Workflow**:

1. Capture the client's strategic question and industry context. Determine whether the client is a vendor (include competitive positioning) or a buyer (focus on market landscape).
2. Resolve the relevant market, vendors, and segments via `search_lookup_entities`.
3. Query IDC Trackers for vendor market share, growth rates, and ranking changes via `qda_qda_execute_query`, and analyze the competitive dynamics and market shifts.
4. Pull Spending Guide use-case data for technology adoption trends via `qda_qda_execute_query`.
5. Cross-reference IDC MarketScape for vendor capability evaluations via `search_search_documents` where positioning matters.
6. If the client is a vendor, build a competitive positioning matrix; if a buyer, frame the market landscape and demand outlook.
7. Synthesize the evidence into a clear strategic recommendation with the trade-offs stated.
8. Tie every recommendation point to an IDC citation.

**Mandatory output format**: Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes, leading with the recommendation and supporting it with cited market and share evidence and a competitive positioning matrix where applicable. Best delivered as an email-ready brief for client C-suite, no interaction needed.

### Route: idc.mkt-oppty

**JTBD**: Size and validate a client's market opportunity using spending forecasts and market sizing (consultant-led).

**Triggered by**: "client market opportunity", "size the client's opportunity", "is this market worth entering for our client", "opportunity brief for client", "validate the opportunity", "5-year projection for client market".

**Workflow**:

1. Capture the client's industry, geography, and technology domain.
2. Resolve the entities and align to IDC taxonomy via `search_lookup_entities`.
3. Pull IDC Spending Guide forecasts for the client's technology area by industry and geography via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`.
4. Extract Black Book TAM data for macro market framing via `qda_qda_execute_query`.
5. Cross-reference Tracker vendor share/growth data for competitive context, and surface any discrepancies across programs with confidence ranges.
6. If the opportunity exceeds a materiality threshold, develop the opportunity brief; if it is niche, flag it as such and explain why.
7. Validate the opportunity against IDC Survey buyer investment priorities via `search_search_documents`.
8. Build a 5-year projection with cited IDC data, growth rates, and assumptions.

**Mandatory output format**: Strategic Brief — a static 3–5 page structured advisory document with clear sections, embedded charts, and IDC citations: opportunity framing, market size and 5-year projection, competitive context, buyer-demand validation, and a recommendation. Best delivered as a board-ready advisory doc for client leadership review.

### Route: idc.dx-transform

**JTBD**: Identify technology-driven transformation opportunities for a client using DX use-case forecasts (consultant-led).

**Triggered by**: "DX transformation", "transformation opportunities", "use case forecast", "digital transformation opportunities by use case", "AI transformation opportunities", "rank transformation opportunities", "build/buy/partner for transformation".

**Workflow**:

1. Capture the client's industry and transformation priorities.
2. Resolve the industry, use cases, and technology domains via `search_lookup_entities`.
3. Pull DX and AI Spending Guide forecasts by use case via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`.
4. Extract AI Spending Guide detail for AI-specific transformation opportunities via `qda_qda_execute_query`.
5. Cross-reference Technographics for the client's current technology gaps via `search_search_documents`.
6. Rank opportunities by growth rate × addressable market.
7. If the client has technology gaps, recommend build/buy/partner for each priority opportunity.
8. Validate with peer adoption patterns and IDC survey data via `search_search_documents`.

**Mandatory output format**: Scorecard — a semi-interactive, sortable/filterable weighted scoring matrix of transformation opportunities (Use Case, Growth Rate, Addressable Market, Client Gap, Weighted Score, RAG indicator, Build/Buy/Partner), with IDC citations. Best used as an evaluation tool for prioritizing and selecting transformation initiatives.
