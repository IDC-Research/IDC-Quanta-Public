# IDC MCP Playbook

Read this file before any structured-data (QDA) work. It defines the exact MCP tool names (the `qda_qda_` chain), the discovery path, and the standard query sequence.

## IDC MCP tool reference

The IDC MCP connector exposes two tool families. The names below are the **logical tool names** published by the bundled `idc` MCP server — refer to them exactly as written. At runtime Claude Code surfaces each tool under an `mcp__…__` namespace prefix derived from the plugin and server names, and resolves and calls the right tool automatically when you reference it by the logical name. Do not hardcode the runtime `mcp__…__` prefix anywhere — it varies with the plugin and server names and is brittle.

**QDA family (structured data):**

- `qda_qda_list_libraries` — list IDC tracker and spending guide libraries available to the user.
- `qda_qda_list_datasets` — list datasets within a library.
- `qda_qda_list_profiles` — list profiles within a dataset (quarterly vs annual, regional cuts).
- `qda_qda_gather_context_for_dataset` — return the schema, dimensions, and metadata for a dataset before querying it.
- `qda_qda_list_attributes_values` — return the valid values for a given attribute (used to build filters).
- `qda_qda_execute_query` — execute the query and return the rows.

**Search family (research documents and entities):**

- `search_search_data_products` — keyword spot-check for IDC data products. It returns only a relevance-ranked subset of the catalog (its default `limit` is 5), not the full library list. Do not use it as the primary library-discovery path — use `qda_qda_list_libraries` (full catalog) and treat this tool as a spot-check only.
- `search_lookup_entities` — resolve vendor names, technology segments, and geographies to IDC taxonomy identifiers.
- `search_search_documents` — semantic search over IDC research documents (used for analyst commentary, headwinds and tailwinds, qualitative context, MarketScape positioning, Fact Lake signals).
- `search_get_full_document` — retrieve the full body of a research document by ID.

Standard chain for a quantitative request: `search_lookup_entities` → `qda_qda_list_libraries` (full-catalog discovery; cache the result for the session) → `qda_qda_list_datasets` → `qda_qda_list_profiles` → `qda_qda_gather_context_for_dataset` → `qda_qda_list_attributes_values` → `qda_qda_execute_query`. Skip steps only when you already have the identifier the next step needs. Use `search_search_data_products` only as a quick keyword spot-check, never as the primary discovery path — it returns only a relevance-ranked subset of the catalog.

**URL extraction for live links:** `search_search_documents` and `search_get_full_document` return document metadata that may include a document URL or IDC platform deep link. `qda_qda_list_libraries` and `qda_qda_gather_context_for_dataset` may return a URL for the data product. When a URL is present in any MCP response, capture it immediately and attach it to the corresponding citation. Do not attempt to construct or recall URLs from training data — only surface URLs that appear explicitly in the tool response.
