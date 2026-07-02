Read this file when the dispatcher selects the idc.spend-bench route. Run its workflow in order; on a simple single-fact ask, run only the minimal step(s) needed and keep the answer short.

### Route: idc.spend-bench

**JTBD**: Benchmark a buyer's IT spend by category against industry peers and set investment priorities.

**Workflow**:

1. Capture the enterprise IT budget breakdown by category, plus the industry vertical, company-size cohort, and geography. If the breakdown is not provided, ask for it or proceed with category-level estimates and state the limitation.
2. Map the budget categories to IDC technology classifications via `search_lookup_entities`.
3. Identify the right spending benchmark dataset via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_gather_context_for_dataset`.
4. Pull IDC spending benchmarks for the matching industry, company-size, and geography cohort via `qda_qda_execute_query`. If an exact cohort match exists, generate a direct comparison; if not, use the closest cohort and flag the approximation.
5. Compare the user's allocation percentages by category (cloud, security, AI, etc.) against peer norms to identify over- and under-investment using IDC spending distribution data.
6. Where a category is significantly misaligned with peers, note whether the misalignment looks strategic (intentional) or operational (drift), and validate against IDC survey peer-CIO investment priorities via `search_search_documents`.
7. Evaluate the user's roadmap initiatives against IDC peer adoption rates and investment benchmarks.
8. Model budget-reallocation scenarios using IDC reference ranges by category and peer cohort.

**Body format** (inside the standard response wrapper — the IDC Quanta header and disclaimer still apply): a Comparative Table of the user's allocation by category versus peer norms, flagging over- and under-investment, with IDC citations and a short read-out. Build the interactive benchmarking dashboard (filters, treemaps, reallocation sliders) only if the user explicitly asks.
