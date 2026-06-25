Read this file when the dispatcher selects the idc.signal-scan route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.signal-scan

**JTBD**: Detect early competitor signals and market moves so a vendor's strategy team can react before rivals do.

This route covers point-in-time and ongoing scanning of competitive and market signals. It differs from `idc.comp-strategy` (which synthesizes a full strategy read) and `idc.comp-share` (which measures share): here the unit of work is the individual signal, classified and triaged. IDC Fact Lake is the differentiated asset — analyst-grounded signals rather than scraped web content.

**Workflow**:

1. Capture the scope: which competitors, markets, and signal types matter, and whether the user wants a one-time scan or a recurring digest.
2. Resolve the named competitors and markets to IDC identifiers via `search_lookup_entities`.
3. Scan IDC Fact Lake, research documents, and recent tracker updates for competitor mentions via `search_search_documents`; retrieve full context for high-relevance hits via `search_get_full_document`.
4. Classify each signal by type — M&A, product launch, pricing change, partnership, hiring, market entry — and assess materiality (raw patent and live-feed signals are out of scope; see step 7).
5. Score signal priority by strategic relevance to the user's stated priorities and estimated market impact. Escalate strategic-level moves (acquisitions, major pivots) with a full context note; queue tactical signals into a lower-priority digest.
6. If multiple signals cluster around one competitor, flag the pattern and recommend chaining `idc.comp-strategy` for a deep-dive synthesis.
7. Ground each signal in its IDC source so the user can trace it; never present an unsourced signal as fact. Note the data limit: this connector reads IDC research and analyst coverage, not raw live feeds, so signals reflect IDC's most recent coverage rather than breaking web news (see `references/capabilities.md`).
8. For a recurring request, propose a cadence and channel (for example a weekly digest) and define the escalation threshold that triggers an immediate alert.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a prioritized signal list in text and tables — each row showing the signal, type, competitor, materiality/severity flag, an IDC source, and a recommended action — ordered by priority with strategic escalations called out at the top. The streaming alert-feed or push-notification panel is available on request; default to the cited list.
