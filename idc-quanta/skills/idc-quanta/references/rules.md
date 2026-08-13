# IDC Skill — Operating Rules

Read this file at the start of every `/idc-quanta` request and apply all nine rules. They are the non-negotiable operating constraints for the skill: they govern when to call IDC tools, how to cite, how to rank documents, how to handle event-driven queries, and how to keep every answer IDC-sourced. The dispatcher does not repeat them, so they must be read here.

## Rule 1. Always call IDC MCP tools before answering.

Before producing any response, call the IDC MCP tools. Call IDC tools to verify coverage of any vendor or vertical before answering.

## Rule 2. Prefer structured data over document search.

For quantitative questions (e.g., market share, market size, forecast numbers, rankings, growth rates), prioritize the QDA tool family and pull structured data first. Use document search only for narrative content such as analyst commentary, headwinds and tailwinds, and qualitative context. Do not default to document search when structured tracker data exists for the question.

## Rule 3. Use inline superscript citations; collect sources before the disclaimer.

Every figure, table, or narrative claim drawn from an IDC tool call is tagged with an inline superscript number — ¹, ², ³, and so on — placed immediately after the claim it supports. At the end of every response, between the body and the Disclaimer, include a **Sources** section that lists each citation in order. This format lets the reader trace exactly which content relies on which IDC source without interrupting the flow of the response.

**Inline placement:** Add the superscript directly after the sentence it supports. For tables and figures, do not place a superscript inside the table or floating on its own line beneath it. Instead, give every table or figure a descriptive title on the line directly below it, formatted in italics, that describes what the table or figure represents. Place the superscript at the end of that title:

*[Descriptive title of what the table or figure shows] ¹*

For example: *Worldwide Cloud IaaS Vendor Revenue and Market Share, 2H 2025 ¹*

Use the same superscript number for repeated references to the same source. Unicode superscripts cover ¹ through ⁹. For a tenth or higher source, use the bracketed form `[10]`, `[11]`, etc.

**Sources section (required on every response, placed before the Disclaimer):**

```
**Sources**
¹ [Title](live IDC URL), Year
² [Title](live IDC URL), Year
```

- **Title** — the exact title string the connector returns for that document or data product, reproduced verbatim. Use only a title that appears literally in the tool response; never compose, paraphrase, or infer one.
- **Year** — the publication year (from the connector's `published_date` field). Always include it, even when the title already contains a year or year-range.
- **Nothing else** — the entry is exactly the title, its link, and the year. Do not append the month, the IDC document or container number, an "IDC #" string, the analyst name, or any other metadata.
- **Link** — a live IDC URL embedded as a markdown hyperlink on the title. Capture the URL at the search step: `document_url` from `search_documents` for research, or `library_url` from `search_data_products` for trackers and spending guides. The QDA data chain returns no URL, so when tracker or spending guide figures came from it, make one `search_data_products` call for that library to capture its `library_url`. Keep any captured URL even after calling `get_full_document`, which returns no URL of its own. Accept any IDC-owned domain (my.idc.com, idctracker.com). Use only URLs the connector returned this session; never construct or guess one.

Because the search tools return a URL for essentially every entitled item, almost every source should carry a live link. If you genuinely have no URL for a real cited figure, show the title without a link rather than dropping the figure, and never fabricate a URL. If a number cannot be tied to an MCP response, do not include the number.

IDC Links, Quick Takes, Vendor Profiles, and Executive Snapshots are full research documents: cite them the normal way — `¹ [Title](document_url), Year` — never by a bare descriptor. The descriptor form is a last resort only for a document that genuinely returns no title; in that case use a short plain descriptor (not a title in quotes) with the `document_url` when present, for example `² [IDC Link](document_url), 2025 — Quick Take on the Palo Alto / CyberArk deal`. Never invent a title or present a composed descriptor as the document's real title. Titles come from the `search_documents` `title` field, reproduced verbatim; `get_full_document` returns content only with no title field.

## Rule 4. No source drift across conversation turns.

In multi-turn conversations, every IDC-sourced answer must stay IDC-sourced. If a follow-up requires data IDC does not cover, say so explicitly: "IDC does not cover X. I can show you what IDC has on Y, or recommend an analyst inquiry for X."

Do not silently introduce facts from training data, public news, or other sources. If you find yourself about to mention something that did not come from an IDC tool call in this conversation, stop and either call the IDC tools again to verify, or explicitly flag the source as non-IDC. The exception is if the user explicitly asks to use a specific source.

## Rule 5. Full research outranks non-research (hard floor, with event-oriented exception).

IDC blogs, document abstracts, press releases, and marketing materials always rank below any full IDC research document. This is a hard rule and does not vary by intent or recency.

**Event-oriented exception:** when a query is explicitly news- or event-driven — for example, "What has [vendor] announced recently?", "What just happened with [company]?", or any request whose primary intent is to surface the latest discrete development rather than substantive analysis — IDC Links, IDC Blinks, and Market Notes may be elevated to primary and ranked by recency. This exception applies only when the query's dominant intent is the event itself; it does not permit these document types to displace substantive full-research documents when the user's underlying need is analysis, framing, or market context.

## Rule 6. Framework documents are always relevancy-weighted.

TechScapes, MaturityScapes, Taxonomies, and Planning Guides derive value from the frameworks they provide, not from how current their data is. A two-year-old TechScape should not be displaced by a recent Market Note when the query needs definitional or structural grounding.

## Rule 7. Recency-first vs relevancy-first ranking axis.

Some routes favor the most recent document. Others favor the most topically relevant. Apply the axis below when ranking documents inside any route.

**Recency-first routes (hard numbers, current standings):** `idc.market-share`, `idc.market-size`, `idc.it-outlook`, `idc.comp-share`, `idc.tam`, `idc.spend-bench`, `idc.mna-target`, `idc.mkt-oppty`, and the data-pull steps inside any other route.

**Relevancy-first routes (framing, positioning, narrative):** `idc.exec-intel`, `idc.vendor-eval`, `idc.marketscape`, `idc.battlecard`, `idc.deal-strategy`, `idc.strategy-rec`, `idc.dx-transform`, `idc.comp-strategy`, `idc.signal-scan`, `idc.emerging-tech`, `idc.qa`, `idc.quote`, and the taxonomy or framing steps in any other route.

**Recency-first data pulls inside relevancy-first routes:** when a relevancy-first route reaches a step that pulls hard numbers (market size, share, revenue, growth rate, edition currency), that step is recency-first. Framing, taxonomy, and analyst commentary remain relevancy-first.

**Emerging technology exception:** for AI, generative AI, and agentic systems, recency weight is elevated even in relevancy-first routes. Documents older than 12 months should be treated with caution and supplemented with newer sources where available.

## Rule 8. Data vs research product source-of-truth.

**Data takes precedence over research documents.** When a data product and a research report contain the same figure for the same scope, cite the data product.

## Rule 9. Follow user source instructions, but flag outdated or misaligned sources.

When a user explicitly requests a specific source, document type, or data product, follow that instruction. Do not override the user's stated preference in favor of the default document hierarchy.

However, if the requested source is materially outdated (generally older than 16 months for recency-sensitive topics) or methodologically misaligned with the question (for example, a demand-side Spending Guide cited for a supply-side vendor share question), surface a brief inline note immediately after the citation — for example: *Note: this source is from [year]; more current IDC data may be available.* or *Note: this source measures buyer spend rather than vendor revenue, which may not directly answer the question.* Continue with the user's requested source; the note is informational, not a refusal.

## Confidence scale (label every response on header line 3)

Rate the whole response and show it as `Confidence: <level>` under the route:

- **High** — the answer rests on direct, on-point live IDC data (e.g., the exact tracker figure for the asked scope).
- **Medium** — a reasonable inference: closest-cohort or proxy data, a single source, or a near taxonomy match.
- **Low** — a data gap, an extrapolation, partial alignment, or a SHARE figure that did not reconcile in the cross-check.
