# idc v1.0.0 (modular)

IDC market intelligence and advisory skill for Cowork, invoked via `/idc`. A lean dispatcher routes each request to one of 20 specialist routes, then loads the matching reference file and runs its workflow. This is the modular rebuild of the earlier monolithic skill — same content and behavior, reorganized for maintainability.

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

- Active IDC subscription with entitlement to the trackers, spending guides, and research you intend to query

## Authentication

This plugin bundles its own IDC MCP server (see `.mcp.json`) — you do not need to add one separately. The server authenticates over OAuth:

1. Install and enable the plugin.
2. On first use, run `/mcp`, select the `idc` server, and authenticate. Claude Code opens the IDC login page in your browser.
3. After you log in, the token is stored and reused, and renews automatically.

No API key configuration is required. On a headless or SSH session, run `claude mcp login idc` to get a paste-back URL; check status anytime with `claude mcp get idc`.

## Files

```
.claude-plugin/plugin.json   Plugin manifest
.mcp.json                    Bundled IDC MCP server (OAuth on first use)
README.md                    This file
skills/idc/
  SKILL.md                   Dispatcher — protocol, routing table, precedence
  references/
    rules.md                 The nine operating rules (read first, every request)
    mcp-playbook.md          MCP tool reference, QDA chain, discovery path
    brand-voice.md           IDC brand voice + four-part response structure + citation standards
    routes/
      market-sizing.md       market-size, tam, it-outlook
      market-share.md        market-share, comp-share
      vendor-evaluation.md   vendor-eval, marketscape, battlecard, deal-strategy
      account-intelligence.md wallet-share, buyer-intel, acct-spending, account-plan, spend-bench
      research-qa.md         qa, quote
      advisory.md            exec-intel, strategy-rec, mkt-oppty, dx-transform
```

