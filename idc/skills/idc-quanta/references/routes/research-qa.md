# Routes — Research & Quotes

Read this file when the dispatcher routes to: idc.qa, idc.quote. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.qa

**JTBD**: Deliver a comprehensive, entitlement-checked answer that merges IDC research and structured data in a single response.

**Triggered by**: "what does IDC say about", broad multi-part questions, "combine the research and the data", "give me everything IDC has on", "is this in our subscription", "entitlement", and any open question spanning more than one IDC program.

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

**Mandatory output format**: Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes for the strongest analyst statements, and a citations list with dates and analyst names. Best delivered as an email-ready brief for a C-suite reader who needs the merged answer with no further interaction.

### Route: idc.quote

**JTBD**: Find and retrieve verbatim IDC quotes with full source context and citation metadata.

**Triggered by**: "IDC quote", "verbatim quote", "analyst quote", "pull a quote for the deck", "citation-ready quote", "what did the analyst say exactly", "quote for our RFI/proposal".

**Workflow**:

1. Capture the quote search criteria: topic, technology, keyword, analyst name, or use-case context.
2. Resolve any named analyst, vendor, or topic via `search_lookup_entities`.
3. Search the full-text IDC document library via `search_search_documents`, combining semantic and keyword matching, and pull candidates with `search_get_full_document`.
4. Return exact verbatim quotes with complete citation metadata: document title, publication date, analyst name, page number, and report type.
5. Rank quotes by recency, analyst seniority, and relevance to the search context.
6. If multiple quotes cover the same topic, group them thematically and surface the strongest by citation authority.
7. Validate each quote against its source document to ensure accuracy and prevent truncation or misattribution.
8. Flag any quote that may be superseded by more recent analyst research on the same topic, and surface an adjacent passage that provides broader context where helpful.

**Mandatory output format**: Comparative Table — a static or sortable matrix of quotes (Verbatim Quote, Analyst, Document Title, Date, Page, Report Type, Recency/Authority flag), color-coded by currency, exportable to Excel/PDF as a citation-ready quote package for presentations, proposals, and RFI responses.
