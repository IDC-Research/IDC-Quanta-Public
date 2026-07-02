Read this file when the dispatcher selects the idc.emerging-tech route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.emerging-tech

**JTBD**: Identify and evaluate emerging technologies for enterprise business impact (buyer-side innovation team).

This route serves a technology buyer assessing whether and when to adopt an emerging technology and what the risks are. It is distinct from `idc.vendor-eval` and `idc.marketscape`, which shortlist named vendors for a defined purchase — here the unit of evaluation is the technology itself, its maturity, and its readiness for the enterprise.

**Workflow**:

1. Capture the technologies under consideration (or the business problem to scan against), the user's industry, and the decision horizon. Continuously scan IDC research and the Black Book for disruptive-technology signals and adoption inflection points where the request is open-ended.
2. Resolve the technologies and the user's industry to IDC taxonomy IDs via `search_lookup_entities`.
3. Pull IDC's view on each technology's adoption and spend trajectory from the Black Book and the relevant Spending Guides via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query` (demand-side forecasts by technology, industry, and use case).
4. Establish each technology's IDC adoption-curve positioning (innovator, early adopter, mainstream) and pull hype-cycle / maturity analysis, IDC research reports, and survey/Knowledge Hound data on investment priorities and sentiment via `search_search_documents` and `search_get_full_document`. Flag any technology crossing into mainstream that warrants immediate strategic attention.
5. Assess business relevance and strategic fit by industry vertical, and map each technology's capabilities to the specific enterprise business problems and use cases it would address for the user.
6. Where peer-adoption signals are available (for example IDC Knowledge Hound), benchmark peer adoption timing to calibrate explore / evaluate / adopt decisions against comparable enterprises.
7. Score each technology on a consistent set of criteria — business impact, IDC maturity / adoption-curve stage, adoption momentum, cost/spend outlook, and risk — using RAG (red/amber/green) indicators, with every score tied to an IDC data point or document, and assemble the prioritization matrix.
8. Translate the matrix into a recommendation per technology — adopt now, pilot, watch, or hold — with the rationale and key risks stated, and flag the watch-items and the signals that would change the call.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a weighted Scorecard — a sortable matrix of technologies × evaluation criteria with RAG indicators, an overall score and an adopt/pilot/watch/hold call per technology, each cell cited to IDC data or research, followed by a short narrative on the top recommendation and its risks. Build the interactive scorecard only if the user explicitly asks.
