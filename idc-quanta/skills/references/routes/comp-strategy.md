Read this file when the dispatcher selects the idc.comp-strategy route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.comp-strategy

**JTBD**: Understand a competitor's strategy and long-term intent for a vendor's own strategy team.

This route answers the qualitative "what is competitor X doing and where are they headed" question. It is distinct from `idc.comp-share` (which quantifies share performance) and `idc.battlecard` (which packages a reusable sales asset). Use it when the user wants a synthesized read on a competitor's direction, not a single number.

**Workflow**:

1. Capture the target competitor, the focal markets that matter to the user, and the decision the read will inform.
2. Resolve the competitor, its product lines, and the relevant markets to IDC taxonomy IDs via `search_lookup_entities`.
3. Gather the qualitative evidence base: pull analyst research, CIS documents, and Fact Lake signals that mention the competitor via `search_search_documents`, and retrieve full text for the most relevant items via `search_get_full_document`.
4. Pull the quantitative backbone — the competitor's share, revenue, and growth trajectory — from the relevant tracker via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`. If a SHARE figure is used, apply the SHARE sanity check (see Known MCP issues) and cross-check against a published document.
5. Synthesize across research, data, and signals into a single strategy narrative: where the competitor is investing, the moves it has made, and the intent those reveal. Score strategic intent from investment patterns, hiring, M&A, and product direction.
6. Map the competitor's trajectory against IDC's 3-year market forecast for each focal segment to project how the competition evolves. Identify capability gaps and white space in its positioning.
7. Flag where that trajectory intersects the user's priority markets as a direct competitive threat, and note the confidence level for each claim given evidence density.
8. If the user wants ongoing coverage, recommend chaining `idc.signal-scan` to track how the competitor's narrative shifts quarter over quarter.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a layered Strategic Brief — a 3–5 page structured narrative (PDF/doc-ready) with an executive summary first, then evidence sections (investment and moves, trajectory versus forecast, capability gaps, threat assessment), every claim cited to IDC research, data, or a Fact Lake signal. Lead with the intent read, then the evidence. Keep it to the narrative and cited tables; build interactive drill-downs only if the user explicitly asks.
