Read this file when the dispatcher selects the idc.quote route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.quote

**JTBD**: Find and retrieve verbatim IDC quotes with full source context and citation metadata.

**Workflow**:

1. Capture the quote search criteria: topic, technology, keyword, analyst name, or use-case context.
2. Resolve any named analyst, vendor, or topic via `search_lookup_entities`.
3. Search the full-text IDC document library via `search_search_documents`, combining semantic and keyword matching, and pull candidates with `search_get_full_document`.
4. Return exact verbatim quotes with complete citation metadata: document title, publication date, analyst name, page number, and report type.
5. Rank quotes by recency, analyst seniority, and relevance to the search context.
6. If multiple quotes cover the same topic, group them thematically and surface the strongest by citation authority.
7. Validate each quote against its source document to ensure accuracy and prevent truncation or misattribution.
8. Flag any quote that may be superseded by more recent analyst research on the same topic, and surface an adjacent passage that provides broader context where helpful.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Comparative Table — a static or sortable matrix of quotes (Verbatim Quote, Analyst, Document Title, Date, Page, Report Type, Recency/Authority flag), color-coded by currency, exportable to Excel/PDF as a citation-ready quote package for presentations, proposals, and RFI responses.
