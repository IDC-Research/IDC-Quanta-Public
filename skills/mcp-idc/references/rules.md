# IDC Skill — Operating Rules

Read this file at the start of every `/idc` request and apply all nine rules. They are the non-negotiable operating constraints for the skill: they govern when to call IDC tools, how to cite, how to rank documents, and how to keep every answer IDC-sourced. The dispatcher does not repeat them, so they must be read here.

## Rule 1. Always call IDC MCP tools before answering.

Before producing any response, call the IDC MCP tools. Call IDC tools to verify coverage of any vendor or vertical before answering.

## Rule 2. Prefer structured data over document search.

For quantitative questions (e.g., market share, market size, forecast numbers, rankings, growth rates), prioritize the QDA tool family and pull structured data first. Use document search only for narrative content such as analyst commentary, headwinds and tailwinds, and qualitative context. Do not default to document search when structured tracker data exists for the question.

## Rule 3. Cite every source in-line, with a live link when available.

Every data point or narrative excerpt derived from the MCP must include an in-line citation at the end of the sentence that uses it. When the MCP response returns a URL or document deep link for that source, format the citation as a hyperlink — in chat using markdown `[text](URL)`, in exported files by embedding the URL as a clickable hyperlink on the source text. If the MCP does not return a URL for a source, use the plain-text citation format. Never construct, guess, or extrapolate a URL from training memory — only use URLs returned directly by MCP tool calls in the current session. If a number cannot be tied to an MCP response, do not include the number.

## Rule 4. No source drift across conversation turns.

In multi-turn conversations, every IDC-sourced answer must stay IDC-sourced. If a follow-up requires data IDC does not cover, say so explicitly: "IDC does not cover X. I can show you what IDC has on Y, or recommend an analyst inquiry for X."

Do not silently introduce facts from training data, public news, or other sources. If you find yourself about to mention something that did not come from an IDC tool call in this conversation, stop and either call the IDC tools again to verify, or explicitly flag the source as non-IDC. The exception is if the user explicitly asks to use a specific source.

## Rule 5. Full research outranks non-research (hard floor).

IDC blogs, document abstracts, press releases, and marketing materials always rank below any full IDC research document. This is a hard rule and does not vary by intent or recency.

## Rule 6. Framework documents are always relevancy-weighted.

TechScapes, MaturityScapes, Taxonomies, and Planning Guides derive value from the frameworks they provide, not from how current their data is. A two-year-old TechScape should not be displaced by a recent Market Note when the query needs definitional or structural grounding.

## Rule 7. Recency-first vs relevancy-first ranking axis.

Some routes favor the most recent document. Others favor the most topically relevant. Apply the axis below when ranking documents inside any route.

**Recency-first routes (hard numbers, current standings):** `idc.market-share`, `idc.market-size`, `idc.it-outlook`, `idc.acct-spending`, `idc.comp-share`, `idc.tam`, `idc.spend-bench`, `idc.wallet-share`, `idc.buyer-intel`, `idc.mkt-oppty`, and the data-pull steps inside any other route.

**Relevancy-first routes (framing, positioning, narrative):** `idc.exec-intel`, `idc.vendor-eval`, `idc.marketscape`, `idc.battlecard`, `idc.deal-strategy`, `idc.strategy-rec`, `idc.dx-transform`, `idc.account-plan`, `idc.qa`, `idc.quote`, and the taxonomy or framing steps in any other route.

**Recency-first data pulls inside relevancy-first routes:** when a relevancy-first route reaches a step that pulls hard numbers (market size, share, revenue, growth rate, edition currency), that step is recency-first. Framing, taxonomy, and analyst commentary remain relevancy-first.

**Emerging technology exception:** for AI, generative AI, and agentic systems, recency weight is elevated even in relevancy-first routes. Documents older than 12 months should be treated with caution and supplemented with newer sources where available.

## Rule 8. Parent vs child documents.

**Parent documents** (foundational, framework, or scoping content) serve market sizing, trend analysis, self-benchmarking, definitions, research discovery, strategic advisory, and content generation. Examples: FutureScapes, Market Glances, Perspectives, TechBriefs, TechScapes, MaturityScapes, Planning Guides, PlanScapes, Tech Buyer Presentations, MarketScapes, PeerScapes, Market Analysis Perspectives, Market Perspectives, Taxonomies, Technology Assessments, IDC Surveys.

**Child documents** (derivative, specific, or operational content) serve vendor revenue and share, data extraction, vendor profiles, comparisons, negotiation. Examples: Survey Spotlights, Tech Buyer Survey Spotlights, MaturityScape Benchmarks, Innovators, ProductScapes, Market Forecasts, Market Presentations, Market Share reports, CIS Vendor Profiles, IDC Links, IDC Blinks, Market Notes.

**Parent-First Materiality Override:** parent-first preference yields to a child document when the parent is materially outdated (default threshold: 16 months) and the query is recency-sensitive. Framework documents (TechScapes, MaturityScapes, Taxonomies, Planning Guides) are exempt from this override per Rule 6.

## Rule 9. Data vs research product source-of-truth.

**Data takes precedence over research documents.** When a data product and a research report contain the same figure for the same scope, cite the data product.
