---
description: "Discover APIs from the Postman API Network using Orbit's agent-friendly search. Returns endpoints with evaluateGuide fields showing what each API can and can't do, and can generate an integration task brief for the ones you pick."
allowed-tools: ["Bash", "Write", "Read"]
---

# orbit:discover

Search the Postman API Network for APIs matching a capability query, then optionally
generate an integration task brief for the endpoints you select.

## Input

A capability description — what you need the API to do.
Multiple capabilities: comma-separated or as separate arguments.

## Steps

1. Read `references/orbit-api.md` for the API contract.
2. For each capability query, `POST https://api.buildwithorbit.ai/v1/search` using curl.
   No auth header is needed. The body takes exactly one field, `q` (max 512 chars) —
   any other body field returns `400`. Use the `limit` query parameter (default 10,
   max 25) to control page size.
3. Parse the JSON response. For each result in `data`, extract:
   - `id` — the `urn:orbit:endpoint:v1:...` identifier. Keep it verbatim; it's needed
     for step 6.
   - `resourceType`, `name`, `method`, `url`, and `provider` when present
   - `evaluateGuide` — the agent-oriented breakdown of what the endpoint does, what
     it's good for, and what it doesn't support
4. If more results are needed and `meta.nextCursor` is present, repeat the request with
   `?cursor=<nextCursor>` as a **query parameter** — not a body field. `nextCursor` is
   absent (not null) on the last page. Pagination caps at 40 results per query.
5. Format results as a readable markdown list grouped by capability, and save to
   `orbit-output/<slugified-query>.md`.
6. If the user has a concrete task in mind and endpoints look like a fit, offer to call
   `POST https://api.buildwithorbit.ai/v1/integrate` with `{"task": "...", "resources":
   [{"id": "<id from search>", "type": "<resourceType>"}]}`. Save the returned
   `data[0].taskBrief` to `orbit-output/<slugified-task>-brief.md` — it contains the
   auth requirements, base URLs, ordered steps, and gotchas needed to write the
   integration.
7. Present a summary to the user highlighting the top matches and their evaluateGuide
   insights, especially the "Not supported" lines.

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
- Never construct or edit an `id`. Pass search `id`s to `/v1/integrate` byte-for-byte.
- The `evaluateGuide` field is the key value: it tells you what an API is good for and
  what it can't do, saving trial-and-error.
- On `429`, back off and retry — both endpoints are read-only, so retries are safe.
- Orbit is designed for agent consumption (compact payloads, structured guidance) vs
  human browsing on the Postman API Network website.
