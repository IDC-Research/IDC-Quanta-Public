# What you can ask IDC, and what it can't do

Surface this only when the user asks what the skill can do, what data is available, for help, or invokes /idc-quanta with no question. Do not prepend it to normal answers.

## What you can ask

- **Market share & competitive standing** — "who leads in [market]?", vendor share and rankings, share movement over time, and how a specific vendor is performing against its rivals (share gains and losses, win/loss trends).
- **Market sizing & forecasts** — "how big is the [market]?", TAM/SAM/SOM, CAGR, and growth by segment, vertical, or geography.
- **IT spending outlook & budgets** — the short- and long-term IT spending outlook and revisions, "how much is [industry] spending on [technology]?", and benchmarking your own IT budget against peers.
- **Vendor evaluation & selection** — shortlist vendors for a purchase, see who the IDC MarketScape Leaders are, and compare the top vendors on a weighted scorecard.
- **Competitive enablement** — battlecards against a named competitor, win themes and objection counters, and a strategy to win a specific deal.
- **Competitor strategy & early signals** — a synthesized read on a competitor's strategy and intent ("what has [vendor] done, where are they headed?"), plus early detection of competitor and market moves (M&A, launches, pricing, partnerships).
- **Emerging technology & transformation** — whether and when to adopt an emerging technology, its maturity, business impact, and risk, plus technology-driven transformation opportunities by use case.
- **Executive, investment & M&A** — CFO, board, and investment-case market context, and an acquisition target's market position grounded in IDC share and growth.
- **Strategy & opportunity (advisory)** — a data-backed strategy recommendation grounded in market and share dynamics, and sizing or validating a client's market opportunity.
- **Research, quotes & answers** — "what does IDC say about [topic]?", a merged research-plus-data answer, and citation-ready verbatim analyst quotes.

Just ask in plain language — no command needed, though `/idc-quanta` also works.

## What this connector does not cover (today)

- **Named-account wallet data** — company-specific IT spend isn't in this connector. These requests return the closest industry or cohort benchmark, flagged as an estimate, and will return account-level detail automatically if your connector later includes wallet data.
- **Live web / raw signal feeds** — the connector reads IDC's research and analyst coverage, not raw real-time sources (live earnings, patent filings, VC activity). Competitor-signal monitoring works, but reflects IDC's most recent coverage rather than breaking web news.
- **Vendor pricing / IDC Pricing Service** — pricing and TCO benchmarks aren't connected; battlecards use positioning and market share instead.

Everything is sourced live from IDC, with a citation on every figure; the skill won't answer market questions from memory.
