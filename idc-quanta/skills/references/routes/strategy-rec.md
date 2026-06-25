Read this file when the dispatcher selects the idc.strategy-rec route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.strategy-rec

**JTBD**: Build data-backed strategy recommendations grounded in vendor share and market dynamics (consultant-led).

**Workflow**:

1. Capture the client's strategic question and industry context. Determine whether the client is a vendor (include competitive positioning) or a buyer (focus on market landscape).
2. Resolve the relevant market, vendors, and segments via `search_lookup_entities`.
3. Query IDC Trackers for vendor market share, growth rates, and ranking changes via `qda_qda_execute_query`, and analyze the competitive dynamics and market shifts.
4. Pull Spending Guide use-case data for technology adoption trends via `qda_qda_execute_query`.
5. Cross-reference IDC MarketScape for vendor capability evaluations via `search_search_documents` where positioning matters.
6. If the client is a vendor, build a competitive positioning matrix; if a buyer, frame the market landscape and demand outlook.
7. Synthesize the evidence into a clear strategic recommendation with the trade-offs stated.
8. Tie every recommendation point to an IDC citation.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): Executive Summary — a static 1–2 page narrative (PDF/doc-ready) with key metrics highlighted, pull-quotes and callout boxes, leading with the recommendation and supporting it with cited market and share evidence and a competitive positioning matrix where applicable. Best delivered as an email-ready brief for client C-suite, no interaction needed.
