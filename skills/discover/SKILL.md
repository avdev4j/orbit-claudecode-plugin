---
description: "Discover APIs from the Postman API Network using Orbit's agent-friendly search. Returns endpoints with evaluateGuide fields showing what each API can and can't do, and can generate an integration task brief for the ones you pick."
allowed-tools:
  - "mcp__plugin_orbit_orbit__search"
  - "mcp__plugin_orbit_orbit__integrate"
  - "Bash"
  - "Write"
  - "Read"
---

# orbit:discover

Search the Postman API Network for APIs matching a capability query, then generate an
integration task brief for the endpoints you select.

This plugin bundles Orbit's MCP server, so the `search` and `integrate` tools are
available without setup. No authentication is required.

## Input

A capability description — what you need the API to do.
Multiple capabilities: comma-separated or as separate arguments.

## Steps

1. **Decompose.** Break the request into one focused capability query per intent. Run
   each as a separate `search` call — do not cram intents into one query.

2. **Search.** Call `mcp__plugin_orbit_orbit__search` for each capability:
   - `q` — the query (required, max 512 chars)
   - `limit` — results per page (default 10, max 25)
   - `clientName` — pass `"claude-code/orbit-plugin"` for anonymous usage analytics

   Query style matters. Include the product or provider name alongside the endpoint
   detail: `"PayPal create invoice"` or `"PayPal API to create an invoice"`. Avoid
   jumbled keyword piles (`"paypal invoice payment delivery ordering"`) and avoid
   `OR`-separated queries — run separate calls instead.

3. **Extract.** For each result in `data`, keep:
   - `id` — the `urn:orbit:endpoint:v1:...` identifier. Preserve it verbatim; step 5
     needs it. Never parse, edit, or construct one.
   - `resourceType` — needed as `type` in step 5
   - `name`, `method`, `url`, `provider`
   - `evaluateGuide` — the three-part breakdown: summary, `Use for:`, `Not supported:`

4. **Paginate if needed.** If `meta.nextCursor` is present, call `search` again with
   `cursor` set to that value. `nextCursor` is *absent* on the last page, not null.
   Pagination caps at 40 results per query.

5. **Integrate.** When the user has a concrete task and the endpoint set looks right,
   call `mcp__plugin_orbit_orbit__integrate`:
   - `task` — what they're building (required, max 512 chars)
   - `resources` — entries of `{id, type}`, where `id` and `type` come from a search
     result's `id` and `resourceType`. The schema allows up to 10, but **keep calls
     narrow — 2 or 3 related endpoints**. Wide calls have been observed to return a
     one-line restatement instead of a real brief. To cover more endpoints, make
     several focused calls grouped by sub-task rather than one wide call.

   Save the returned `taskBrief` to `orbit-output/<slugified-task>-brief.md`. It covers
   auth requirements, base URLs, ordered request steps, parameters, inter-step
   dependencies, and gotchas.

6. **Save and summarize.** Write the search results to
   `orbit-output/<slugified-query>.md` and present the top matches to the user, leading
   with the `Not supported:` lines — those are the design gaps worth acting on.

## Output format

For each API result:

```
### <name> (<provider>)
- **Method:** <method>
- **URL:** <url>
- **ID:** <id>
- **Evaluate Guide:** <evaluateGuide summary>
```

Group results under `## <capability query>` headings when multiple queries are run.

## Notes

- If a query returns zero results, say so — don't fabricate endpoints.
- Never construct or edit an `id`. Pass search `id`s to `integrate` byte-for-byte.
- The `evaluateGuide` field is the key value: it tells you what an API is good for and
  what it can't do, saving trial-and-error.
- Both tools are read-only and safe to retry. On a rate-limit error, back off and retry.
- Orbit is designed for agent consumption (compact payloads, structured guidance) vs
  human browsing on the Postman API Network website.

## Fallback

If the MCP tools are unavailable — the server is unreachable, or you're running in an
environment where the plugin's MCP server did not load — read
`references/orbit-api.md` and call the equivalent REST endpoints with curl. The
request and response shapes are identical.
