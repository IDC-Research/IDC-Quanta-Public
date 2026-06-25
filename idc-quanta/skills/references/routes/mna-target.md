Read this file when the dispatcher selects the idc.mna-target route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.mna-target

**JTBD**: Assess an acquisition target's market position using Tracker vendor share and growth trajectory (corporate strategy / M&A).

This route evaluates a specific company as an acquisition target by grounding its market position in IDC Tracker data. It is narrower and more data-anchored than `idc.exec-intel` (which builds the broader board/CFO briefing) — use `idc.mna-target` when the job is to assess one target's standing, and chain into `idc.exec-intel` when the output needs to become a board-ready investment thesis.

**Workflow**:

1. Capture the target company, the markets it competes in, and the deal question (position, growth durability, or valuation support).
2. Resolve the target, its competitors, and the relevant markets to IDC taxonomy IDs via `search_lookup_entities`.
3. Identify the relevant tracker via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`, then confirm dimensions via `qda_qda_gather_context_for_dataset`.
4. Pull the target's market share, 3–5 year share history, revenue, and quarterly/annual growth across all relevant technology markets via `qda_qda_execute_query`, alongside the top competitors for context. **SHARE sanity check (precaution):** the SHARE operationType had a reported 100%-for-all-entries defect (see Known MCP issues); cross-check the top 2–3 share entries against a published IDC document via `search_search_documents`, and present at Low confidence if figures do not reconcile.
5. Size the target's served markets from the Black Book / Spending Guide to validate or challenge the target's own TAM claims.
6. Assess trajectory — gaining, stable, or declining share — and compare the target's growth rate against overall market growth and peer competitors to judge how durable its position is.
7. Calculate an explicit market-position score for the target (leader, challenger, niche, or declining) from its share, rank, and trajectory, and flag concentration and competitive-pressure risks.
8. Pull analyst commentary on the target and its markets via `search_search_documents` to explain the numbers and surface qualitative risks or strengths.
9. Summarize the market-position score with the evidence and the open risks; if the user wants pricing or thesis framing, recommend chaining `idc.exec-intel`.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a cited market-position assessment in text and tables — the target's share and rank, share trajectory over time, market growth outlook, competitive context versus peers, and a risk list — leading with the leader/challenger/niche/declining position score. The multi-panel interactive dashboard (filters, drill-down charts) is available on request; default to the cited tables and narrative.
