Read this file when the dispatcher selects the idc.qa route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.qa

**JTBD**: Deliver a comprehensive, entitlement-checked answer that merges IDC research and structured data in a single response.

**Workflow**:

1. Parse the query to identify every required IDC source: which research topics, which trackers or spending guides, which surveys.
2. Resolve all named entities and topics via `search_lookup_entities`.
3. Enforce entitlement boundaries at every step: never surface content outside the user's subscription scope. If a needed source is outside entitlement, explain the limitation and suggest the upgrade or inquiry path rather than answering from it.
4. For quantitative parts, run the QDA chain (`qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_gather_context_for_dataset` → `qda_qda_execute_query`). For narrative parts, run `search_search_documents` and `search_get_full_document`.
5. If the query requires multi-source synthesis (Research + Tracker + Survey), orchestrate across the sources and merge findings into one coherent answer.
6. Ground every answer in specific analyst source documents with citation, publication date, and analyst attribution.
7. If synthesis confidence is below threshold for any claim, flag the uncertainty explicitly rather than generating unsupported content.
8. Support multi-turn follow-ups using session memory that preserves prior context and stays IDC-sourced (Rule 4).
9. Route any question that requires analyst judgment beyond the published record to the IDC analyst inquiry scheduling workflow.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes for the strongest analyst statements, and a citations list with dates and analyst names. Best delivered as an email-ready brief for a C-suite reader who needs the merged answer with no further interaction.
