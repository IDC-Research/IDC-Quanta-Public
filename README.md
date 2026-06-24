# idc v1.4 (modular)

IDC market intelligence and advisory skill for Cowork, invoked via `/idc`. A lean dispatcher routes each request to one of 20 specialist routes, then loads the matching reference file and runs its workflow. This is the modular rebuild of the v1.4 monolith — same content and behavior, reorganized for maintainability.

## Routes by cluster

| Cluster | Routes |
|---|---|
| Market sizing & forecast | market-size, tam, it-outlook |
| Market & competitive share | market-share, comp-share |
| Vendor evaluation & enablement | vendor-eval, marketscape, battlecard, deal-strategy |
| Account & spend intelligence | wallet-share, buyer-intel, acct-spending, account-plan, spend-bench |
| Research & quotes | qa, quote |
| Executive & consultant advisory | exec-intel, strategy-rec, mkt-oppty, dx-transform |

## Prerequisites

- IDC MCP configured globally in Cowork: `claude mcp add --transport http idc https://mcp.idc.com/mcp`
- Active IDC subscription with entitlement to the trackers, spending guides, and research you intend to query

## Files

```
.claude-plugin/plugin.json   Plugin manifest
.mcp.json                    MCP connection (inherits global session)
README.md                    This file
skills/idc/
  SKILL.md                   Dispatcher — protocol, routing table, precedence, disclosure
  references/
    rules.md                 The nine operating rules (read first, every request)
    mcp-playbook.md          MCP tool reference, QDA chain, known issues, SHARE cross-check
    brand-voice.md           IDC brand voice + four-part response structure + citation standards
    routes/
      market-sizing.md       market-size, tam, it-outlook
      market-share.md        market-share, comp-share
      vendor-evaluation.md   vendor-eval, marketscape, battlecard, deal-strategy
      account-intelligence.md wallet-share, buyer-intel, acct-spending, account-plan, spend-bench
      research-qa.md         qa, quote
      advisory.md            exec-intel, strategy-rec, mkt-oppty, dx-transform
```

