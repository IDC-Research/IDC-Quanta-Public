Read this file when the dispatcher selects the idc.exec-intel route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.exec-intel

**JTBD**: Supply market intelligence to CFOs and CxOs to validate budgets, M&As, and strategic investments.

**Workflow**:

1. Identify the executive decision type: investment thesis validation, M&A market context, or budget justification. Each routes to a different evidence stack.
2. Pull the data assets relevant to the decision type. For all three, start with `search_lookup_entities` to resolve the focal market and entities, then `qda_qda_list_libraries` to identify the right libraries (full-catalog discovery; `search_search_data_products` returns only a ranked subset and should not be the primary discovery path — see Known MCP issues).
   - Investment thesis: run `idc.tam` (or `idc.market-size`) and a competitive intensity pass from `idc.comp-share`.
   - M&A: add startup activity and funding signals from Fact Lake via `search_search_documents`, and competitor financial benchmarks via `qda_qda_execute_query` against the relevant tracker.
   - Budget justification: add a peer benchmark comparison via `idc.spend-bench` and market opportunity sizing via `idc.market-size`.
3. Model financial impact scenarios (including the revenue impact of competitor share shifts) using IDC forecast data and competitive dynamics. State the assumptions inline.
4. If the request is for board-level distribution, restructure into executive format: 1-page summary, analyst-cited evidence appendix, and an explicit risk assessment section.
5. Validate data currency: every cited number must be from the most recent published IDC edition. Flag any data point older than two editions. Note: although `idc.exec-intel` is a relevancy-first route per Rule 7, all quantitative data pulls inside this route are recency-first per the data-pull clarifier in Rule 7. Relevancy-first only governs framing, taxonomy, and analyst commentary selection.
6. Validate citation accuracy: every claim in the executive summary must map to a citation in the appendix. Render the mapping as a visible table in board-level outputs.
7. If the user requests it, propose a calibration follow-up — track the decision outcome against the market data so future briefings improve.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes, every claim cited. Structure as Executive Summary (three to five bullets), Evidence Appendix (tables and analyst excerpts grouped by data product), and Risk Assessment (market, competitive, execution risks each with an IDC data point). For M&A, add Target Market Activity (Fact Lake); for budget justification, add Peer Benchmark. Best delivered as an email-ready brief for a C-suite or board audience.
