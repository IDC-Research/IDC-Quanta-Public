# Routes — Account & Spend Intelligence

Read this file when the dispatcher routes to: idc.wallet-share, idc.buyer-intel, idc.acct-spending, idc.account-plan, idc.spend-bench. Each route below lists its job-to-be-done, trigger language, ordered workflow, and mandatory output format. Find the matched route and execute its workflow in order without skipping or reordering steps.

### Route: idc.wallet-share

**JTBD**: Benchmark a competitor's share of a target buyer's total technology wallet for sales.

**Triggered by**: "wallet share", "share of wallet", "competitor share of account", "displacement opportunity", "who owns the budget in this account", "wallet share over time", "expansion opportunity gap".

**Workflow**:

1. Capture the target account and the competitive scope.
2. Resolve the account and competitors to IDC identifiers via `search_lookup_entities`.
3. Identify the IDC Wallet library and dataset via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_gather_context_for_dataset`.
4. Pull vendor-level spending for the account or peer cohort via `qda_qda_execute_query` and estimate each competitor's share of the account's technology budget by category. If vendor-level data exists, give a precise share-of-wallet breakdown; if only market-level share is available, extrapolate (market share × account spending) and flag it as an estimate.
5. Track wallet-share shifts over time by vendor and technology category to detect competitive gains or losses.
6. Identify categories where competitors dominate budget share for displacement targeting.
7. Model account-expansion scenarios based on the wallet-share opportunity gap and buyer spending trajectory.
8. Score the account by displacement opportunity using IDC Wallet share versus competitive market-share benchmarks, and recommend an engagement/expansion play.

**Mandatory output format**: Comparative Table — a static or sortable matrix of vendors × technology categories showing share-of-wallet %, trend, and displacement opportunity, color-coded by opportunity size, exportable to Excel/PDF for account planning and QBR decks.

### Route: idc.buyer-intel

**JTBD**: Understand a target customer's IT budget allocation across key product categories for sales.

**Triggered by**: "target customer's IT budget", "account budget allocation", "what is this account spending on", "budget by category for account", "spending trajectory at account", "ICP scoring by spend", "pre-meeting account prep".

**Workflow**:

1. Capture the target account identifier (company name, industry, size).
2. Resolve the company to IDC firmographic identifiers via `search_lookup_entities`.
3. Identify the IDC Wallet library and dataset via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_gather_context_for_dataset`.
4. Query account-level technology spending via `qda_qda_execute_query`. If Wallet data exists for the named account, return the full technology budget profile with category breakdown; if only cohort-level data is available, generate an estimate from the peer cohort and flag the confidence level.
5. Model the spending distribution across cloud, security, infrastructure, applications, and services to identify where the company's products align with budget.
6. Compare the account's allocation to industry peer benchmarks to identify over/under-indexed categories and headroom.
7. If QoQ changes are available, highlight spending-trajectory shifts that signal buying intent or budget contraction.
8. Build an ICP score from the budget estimates to prioritize outreach by propensity to spend.

**Mandatory output format**: Account Profile — an interactive single-entity dossier card with tabs (Overview, Spend, Tech Stack, Signals) presenting the budget estimate, category allocation versus peers, trajectory signals, and ICP score, with IDC citations. Best used as pre-meeting prep or a CRM-embedded company snapshot for the sales rep.

### Route: idc.acct-spending

**JTBD**: Estimate technology spending by target company or cohort for account intelligence.

**Triggered by**: "account intelligence", "wallet data", "account-level spending", "target account spend", "IT budget by company", "spending propensity", "cohort spending profile", "spending trajectory shift".

**Workflow**:

1. Parse the account or cohort definition (named company, industry, size band, geography). Confirm whether the user wants named-account or cohort-level analysis.
2. Resolve company names to IDC firmographic identifiers via `search_lookup_entities`.
3. Identify the Wallet library and dataset via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_list_profiles`, then `qda_qda_gather_context_for_dataset`.
4. Call `qda_qda_execute_query` for technology spending estimates. If Wallet data exists for the named company, return an account-level spending profile. If only cohort-level data is available, generate an aggregate profile with an explicit confidence range and state the limitation.
5. Model spending allocation across technology categories. Identify categories where the account or cohort invests above the peer median.
6. If the user requests peer comparison, build a cohort-level benchmark table showing spending distribution percentiles (P25, P50, P75) and place the focal account within it.
7. Flag accounts with spending-trajectory shifts (acceleration or deceleration) detected from recent Wallet updates.
8. If displacement opportunity is requested, surface wallet share by competing vendor inside the target account, and build an account-scoring model to prioritize outreach by spending propensity.

**Mandatory output format**: Account Profile — an interactive single-entity dossier card with tabs (Overview, Spend, Tech Stack, Signals) covering Account/Cohort Identity, Total IT Spend Estimate (with confidence band), Spend Allocation by Technology Category, Peer Benchmark (percentile placement), Vendor Wallet Share Inside Account (if applicable), Trajectory Signals, and Recommended Action. Best used as pre-meeting prep or a CRM-embedded snapshot for product, sales, or strategy teams.

### Route: idc.account-plan

**JTBD**: Build a strategic, multi-year account plan using IDC market, wallet, and competitive intelligence.

**Triggered by**: "account plan", "strategic account plan", "multi-year account roadmap", "expansion roadmap for account", "executive stakeholder map", "QBR account strategy".

**Workflow**:

1. Capture the named account or account list.
2. Resolve the account, its industry, and key competitors via `search_lookup_entities`.
3. Build a comprehensive technology spending profile per account from IDC Wallet via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_execute_query`. If the account is named in Wallet, use it; if not, construct a peer-cohort proxy and flag it.
4. Map the account's technology footprint and spending allocation against IDC industry peer benchmarks, and identify growth areas where the company's products align with above-average spending trajectory.
5. Surface IDC Knowledge Hound, CIS, and survey data on the account's industry-specific technology priorities and pain points via `search_search_documents`, and compare to peer-CIO survey data to anticipate strategic direction.
6. Identify executive stakeholders and their likely priorities by role using IDC buyer research and persona data.
7. Pull the competitor landscape inside the account (chain `idc.wallet-share` if displacement detail is needed) for competitive context.
8. Build a multi-year expansion roadmap anchored to IDC technology-adoption forecasts for the account's industry, and set a quarterly update cadence that flags significant account-level changes.

**Mandatory output format**: Account Profile — an interactive single-entity dossier card with tabs (Overview, Spend, Tech Stack, Signals) plus an expansion-roadmap view, presenting IDC-cited market context, spending trajectory, competitive landscape, stakeholder map, and the multi-year opportunity map. Best used as a living account record and pre-QBR prep.

### Route: idc.spend-bench

**JTBD**: Benchmark a buyer's IT spend by category against industry peers and set investment priorities.

**Triggered by**: "benchmark our IT spend", "are we over/under-investing", "peer benchmark", "IT budget vs peers", "spend allocation by category", "budget reallocation", "investment priorities vs peers".

**Workflow**:

1. Capture the enterprise IT budget breakdown by category, plus the industry vertical, company-size cohort, and geography. If the breakdown is not provided, ask for it or proceed with category-level estimates and state the limitation.
2. Map the budget categories to IDC technology classifications via `search_lookup_entities`.
3. Identify the right spending benchmark dataset via `qda_qda_list_libraries` → `qda_qda_list_datasets` → `qda_qda_gather_context_for_dataset`.
4. Pull IDC spending benchmarks for the matching industry, company-size, and geography cohort via `qda_qda_execute_query`. If an exact cohort match exists, generate a direct comparison; if not, use the closest cohort and flag the approximation.
5. Compare the user's allocation percentages by category (cloud, security, AI, etc.) against peer norms to identify over- and under-investment using IDC spending distribution data.
6. Where a category is significantly misaligned with peers, note whether the misalignment looks strategic (intentional) or operational (drift), and validate against IDC survey peer-CIO investment priorities via `search_search_documents`.
7. Evaluate the user's roadmap initiatives against IDC peer adoption rates and investment benchmarks.
8. Model budget-reallocation scenarios using IDC reference ranges by category and peer cohort.

**Mandatory output format**: Interactive Dashboard — a multi-panel cockpit with filters (category, peer cohort, geography, time) and drill-down charts (allocation bar vs peer norm, over/under-investment treemap, reallocation scenario sliders), refreshing against the latest IDC benchmark data, with IDC citations on every panel. Best embedded as a live cockpit for CFO and board review.
