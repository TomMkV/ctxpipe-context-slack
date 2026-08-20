---
source: slack
channel_id: "C0AMVSR0EBW"
channel_name: "general"
is_private: false
thread_ts: "1786332612.955909"
team_id: "T0AMNSG03E1"
permalink: "https://ctxpipe.slack.com/archives/C0AMVSR0EBW/p1786332612955909?thread_ts=1786332612.955909&cid=C0AMVSR0EBW"
captured_at: "2026-08-20T04:18:31.667Z"
captured_by:
  handle: "tom"
  name: "Tom"
message_count: 21
participant_ids: ["U0AMKTFPG1H","U0AT5UDGTL1"]
oldest: "1786332612.955909"
latest: "1787199503.892579"
---

# Thread in #general

### Tom (@tom) · 2026-08-10T03:30:12.955Z

Getting MCP connection failures during agent runs in Claude Code, right after authentication.

### Tom (@tom) · 2026-08-10T03:31:00.664Z

Claude reported this feedback:
```The org context layer (ctx_advisor) is unreachable right now — connection errors on both attempts — so I couldn't check for existing ADRs on docs tooling. Not a blocker; the ingestion side is simple anyway: ctxpipe consumes this repo as git-tracked Markdown, which is exactly why plain-markdown-friendliness will weigh heavily in my comparison.```


### Tom (@tom) · 2026-08-10T03:31:20.636Z

Is this a related error?

```Worker tick failed: AggregateError [ETIMEDOUT]:
    at internalConnectMultiple (node:net:1135:18)
    at internalConnectMultiple (node:net:1211:5)
    at Timeout.internalConnectMultipleTimeout (node:net:1743:5)
    at listOnTimeout (node:internal/timers:587:11)
    at process.processTimers (node:internal/timers:521:7)
    at cachedError (file:///app/node_modules/.pnpm/postgres@3.4.8/node_modules/postgres/src/query.js:170:23)
    at new Query (file:///app/node_modules/.pnpm/postgres@3.4.8/node_modules/postgres/src/query.js:36:24)
    at BackendPostgres.sql [as pg] (file:///app/node_modules/.pnpm/postgres@3.4.8/node_modules/postgres/src/index.js:112:11)
    at BackendPostgres.claimWorkflowRun (file:///app/node_modules/.pnpm/openworkflow@0.8.0_postgres@3.4.8/node_modules/openworkflow/dist/postgres/backend.js:216:41) {
  code: 'ETIMEDOUT',
  [errors]: [
    Error: connect ETIMEDOUT 54.209.204.248:5432
        at createConnectionError (node:net:1679:14)
        at Timeout.internalConnectMultipleTimeout (node:net:1738:38)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -110,
      code: 'ETIMEDOUT',
      syscall: 'connect',
      address: '54.209.204.248',
      port: 5432
    },
    Error: connect ENETUNREACH 2600:1f18:4c3a:2921:6e89:52bc:59a6:6d53:5432 - Local (:::0)
        at internalConnectMultiple (node:net:1207:16)
        at Timeout.internalConnectMultipleTimeout (node:net:1743:5)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -101,
      code: 'ENETUNREACH',
      syscall: 'connect',
      address: '2600:1f18:4c3a:2921:6e89:52bc:59a6:6d53',
      port: 5432
    },
    Error: connect ETIMEDOUT 34.206.177.121:5432
        at createConnectionError (node:net:1679:14)
        at Timeout.internalConnectMultipleTimeout (node:net:1738:38)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -110,
      code: 'ETIMEDOUT',
      syscall: 'connect',
      address: '34.206.177.121',
      port: 5432
    },
    Error: connect ENETUNREACH 2600:1f18:4c3a:2916:d3ed:8521:ece0:2536:5432 - Local (:::0)
        at internalConnectMultiple (node:net:1207:16)
        at Timeout.internalConnectMultipleTimeout (node:net:1743:5)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -101,
      code: 'ENETUNREACH',
      syscall: 'connect',
      address: '2600:1f18:4c3a:2916:d3ed:8521:ece0:2536',
      port: 5432
    },
    Error: connect ETIMEDOUT 100.49.65.130:5432
        at createConnectionError (node:net:1679:14)
        at Timeout.internalConnectMultipleTimeout (node:net:1738:38)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -110,
      code: 'ETIMEDOUT',
      syscall: 'connect',
      address: '100.49.65.130',
      port: 5432
    },
    Error: connect ENETUNREACH 2600:1f18:4c3a:2904:3a9e:10db:a9af:2f9f:5432 - Local (:::0)
        at internalConnectMultiple (node:net:1207:16)
        at Timeout.internalConnectMultipleTimeout (node:net:1743:5)
        at listOnTimeout (node:internal/timers:587:11)
        at process.processTimers (node:internal/timers:521:7) {
      errno: -101,
      port: 5432
    }
  ]
      code: 'ENETUNREACH',
      syscall: 'connect',
      address: '2600:1f18:4c3a:2904:3a9e:10db:a9af:2f9f',
}```

### Tom (@tom) · 2026-08-10T03:34:09.921Z

@U0AT5UDGTL1 can you investigate why a newly authed MCP session could return a connection failure? I looked into Railway logs and only found this error. I also didn't see anything in Cursor locally to report on (Output).

### Cursor (@cursor) · 2026-08-10T03:34:49.398Z

Looking into the post-auth MCP connection failures and whether that Railway Postgres timeout is related.

### Cursor (@cursor) · 2026-08-10T03:35:40.065Z

*Related root cause, not the same bug path.* That Railway stack is not an MCP auth defect.  *What the Railway error actually is*  OpenWorkflow’s *worker* failed TCP to *Neon Postgres* while claiming a run (`claimWorkflowRun`). Dual-stack connect: IPv4 `ETIMEDOUT`, IPv6 `ENETUNREACH` (Railway usually has no IPv6; the real signal is IPv4 timing out to Neon’s A records). That is infra reachability, not OAuth token validation logic.  *What Claude reported*  Claude did *not* say “MCP OAuth failed”. It said *`ctx_advisor`* (org context layer) got *connection errors on both attempts*. That tool path hits Postgres hard: • `ensureConversation` • LangGraph `PostgresSaver` checkpointer (`connectionTimeoutMillis: 5_000` in `conversationGraph`)  So two quick advisor retries against a wedged/unreachable Neon look exactly like what Claude paraphrased.  *Why “right after auth” can still happen*  Assumption to challenge: auth succeeding means the whole stack was healthy. Not necessarily. • OAuth can complete in a good window; the next `/mcp` tool call can miss Neon. • Or MCP *session* came up fine, and only *tool* calls failed — clients often narrate that as “connection failure”, and Cursor’s Output panel frequently stays empty for remote Streamable HTTP MCP.  Post-auth MCP itself still depends on Postgres (`withBearerAuth` session lookup + `withNetworkOrgContext` org lookup). Same Neon outage can break both session setup and `ctx_advisor`.  *Verdict*  ``` | Surface | Related? | |---|---| | Worker `ETIMEDOUT` to Neon | Direct evidence of DB path failure | | `ctx_advisor` connection errors | Same class of failure (Postgres) | | MCP OAuth “auth bug” | **Unlikely** from this log alone | | Empty Cursor Output | Expected for server-side remote MCP failures | ``` I’d treat this as a *Railway → Neon connectivity blip* (or Neon compute/network flake), not a newly-authed MCP protocol bug — unless backend logs show 401/`invalid_token` at the same timestamp.  If you want a follow-up, I can harden advisor/checkpointer error surfacing so clients get “Postgres unreachable” instead of a vague connection failure, or help correlate Neon/Railway timestamps for that incident.

### Tom (@tom) · 2026-08-10T03:37:11.418Z

I thought we had retries now in place in case of connection-pool issues or container-warm delays. Can you verify that @U0AT5UDGTL1 in the context of this potential issue?

### Cursor (@cursor) · 2026-08-10T03:37:24.190Z

Checking whether the existing transient DB retries actually cover this Neon timeout path (worker + ctx_advisor).

### Cursor (@cursor) · 2026-08-10T03:38:00.006Z

*Short answer: yes for the main Drizzle pool, no for the paths that actually failed here.* The assumption that “we have retries” covers this incident doesn’t hold.  *What exists*  `#257` / CTX-116 added `wrapPoolQueryWithTransientRetry` on the *backend `pg` pool* in `client.ts`: • Classifies `ETIMEDOUT` / nested `AggregateError` as transient (`#259` improved that) • Retries *once* (2 attempts total) • Default backoff: *100ms*  That helps stale Neon pooler sockets / brief idle disconnects on *Drizzle queries* (including MCP bearer session lookups on that pool).  *What it does not cover (this incident)*  ``` | Path | Retry? | |---|---| | OpenWorkflow worker `claimWorkflowRun` (`BackendPostgres` / `postgres` package) | **No** — separate client, no wrapper. Your Railway stack is here. | | `ctx_advisor` LangGraph `PostgresSaver` pool | **No** — own `Pool` in `conversationGraph/graph.ts`, no wrap. Connect timeout is only **5s**. | | Sustained Neon unreachable | **No** — one retry won’t outlast multi-IP `ETIMEDOUT`. | ``` CTX-116 originally aligned the checkpointer with the hardened pool factory. That alignment is gone; the checkpointer is a raw pool again (further tweaked in `#255` for PR-sleep).  *The warm-delay claim is weak*  The comment says the delay lets Neon compute finish waking. *100ms + one retry is not a Neon cold-start strategy.* It’s a stale-socket recovery. Container/DB warm delays measured in seconds will still lose.  *Bottom line*  Your mental model is right for *auth session lookups on the API pool*. It is *not* right for: 1. the worker tick error you pasted, or   2. `ctx_advisor` checkpoint DB access.  Those gaps are why retries “we already have” wouldn’t have saved this. If you want, I can close them: wrap the checkpointer pool the same way, and decide whether worker/OpenWorkflow needs its own retry or we just treat that log as infra noise.

### Tom (@tom) · 2026-08-10T03:56:57.900Z

Some interrogation with an agent in Cursor. Verify this @U0AT5UDGTL1:

Most likely not a flaky MCP transport. Cursor’s `MCP task kjmwr5pg (ctxpipe/ctx_advisor) failed` lines up with a codesearch Zoekt warmup stall during that tool call — plus a couple of separate auth/SSE issues that are easy to misread as “MCP is broken.”
What happened in the failure window (prod)
Timeline around your earlier Zoekt logs (~03:43 UTC / ~1:43pm AEST):
Time (UTC)Evidence`03:43:35`
Codesearch Zoekt: `loading` 10 shards for org `org_agox…`
`03:43:35` → `03:44:59`
Backend: repeated `transient HTTP 503` (codesearch search retries)
`03:45:31`
`POST /mcp` 200 after ~128s
That is one story: `ctx_advisor` ran → agent tools hit `/search` → codesearch pinned cold shards → Zoekt started loading → warmup didn’t finish in time → 503 → backend retried hard → call dragged on for ~2 minutes.
Causal chain (this is the substantive bug)
1. Hot shards idle-unload after5 minutes(`PIN_IDLE_TTL_MS`).
2. Search re-pins and waits for Zoekt load with an8swarmup budget (`ZOEKT_WARMUP_TIMEOUT_MS`).
3. Loadingmanyrepos at once can exceed 8s → codesearch returns503(`ZoektWarmupTimeoutError`).
4. Backend`zoektSearchRepository`retries up to10times (backoff capped at 30s) under a10sper-attempt fetch timeout.
5. Cursor surfaces that as`ctx_advisor`failed — even though Railway later recorded the MCP POST as200after ~128s (server eventually finished; client may have already given up, or the tool errored inside a 200 JSON-RPC body).
Assumption to drop: those Zoekt `loading shard(s)` lines were unrelated noise. They are the smoking gun for this incident.
Other “connection” symptoms (real, but different)
Intermittent `/mcp` 401s (several in the last few days, including today `00:47` / `01:16`). Documented behaviour: OAuth access tokens are long-lived (~4h) but clients must refresh; stale Bearer → 401. That is auth, not Zoekt.
`GET /mcp` lasting exactly `900000ms` (15 min) repeatedly. That looks like Railway edge killing long-lived streamable-HTTP/SSE sessions. Feels like “MCP disconnected”; it is proxy lifetime, not `ctx_advisor` logic.
Skeptical take
Calling this generically “MCP connection issues” overfits. Evidence for the specific Cursor failure points at tool execution / codesearch readiness, not broken OAuth discovery or a down backend. MCP itself was accepting traffic (200/202) throughout.
Also: in 7 days of searchable backend logs, these `transient HTTP 503` bursts appear only in this window — so it is not a constant outage; it is a cold-pin / multi-repo warmup cliff.
What would actually fix it
In priority order:
1. Raise or make adaptive Zoekt warmup timeout when pinning many shards (8s is tight for 10 repos).
2. Avoid stampeding: pin/warm fewer repos per search, or warm in the background before failing.
3. Align client/tool timeouts with worst-case retry budget (10 × ~10s+backoff can exceed Cursor’s patience).
4. Treat 401s separately (force MCP re-auth / refresh).
5. Treat 15-minute GET cutoffs as infra (keepalive / shorter sessions / Railway timeout config) — not the same as`ctx_advisor`failure.
If you want, I can dig into whether that ~128s `POST /mcp` returned a successful advisor answer or a tool-error payload, or sketch a concrete warmup timeout change

### Cursor (@cursor) · 2026-08-10T03:57:15.980Z

Verifying that Zoekt cold-pin / warmup + retry budget story against the code — I'll call out what holds and what doesn't.

### Cursor (@cursor) · 2026-08-10T03:58:03.606Z

*Mostly holds.* The Zoekt cold-pin / warmup cliff is real in code, and it is a better fit for a Cursor `ctx_advisor` failure than “MCP transport is flaky.” A few details need tightening.  *Verified in code*  ``` | Claim | Verdict | |---|---| | `PIN_IDLE_TTL_MS` = 5 min idle unload | True (`pinManager.ts`) | | Warmup budget 8s (`ZOEKT_WARMUP_TIMEOUT_MS`) | True (`warmup.ts`) | | Warmup miss → `ZoektWarmupTimeoutError` → **503** | True (`search.ts`) | | `ctx_advisor` hits org `/search` and can pin many repos at once | True: graph → `retrievalChannels` → `codeSearch` sends `RepoIDs: repos.map(...)` | | Backend retries 503 up to 10×, backoff capped 30s, logs `transient HTTP 503` | True (`withTransientHttpRetry` + `codeSearch`) | | Stale Bearer → `/mcp` 401; tokens ~4h, clients must refresh | True (docs + auth path) | | Cursor can show tool failure while `POST /mcp` later lands **200** | Plausible: Streamable HTTP 200 can still carry a JSON-RPC tool error | ``` So the causal chain — idle unload → multi-repo pin → 8s warmup cliff → 503 storm → long advisor call — is mechanistically sound. Treating Zoekt `loading shard(s)` as noise would be wrong.  *Corrections / gaps*  1. *The “10s per-attempt fetch timeout” is the wrong path for this incident.* That `AbortSignal.timeout(10_000)` is on `zoektSearchRepository` (repo-scoped agent tools). `ctx_advisor` retrieval uses `codeSearch`, which has *no* per-attempt abort — only the retry loop around whatever codesearch returns (~8s 503 on warmup miss). So “10 × ~10s” overstates the advisor path; worst case is still large because of retries + backoff (~2 min is believable).  2. *I cannot re-verify the prod timestamps from here* (no Railway log access in this session). The mechanism matches the story; the exact `03:43` correlation depends on those logs being accurate.  3. *Do not collapse earlier evidence into this.* The worker `ETIMEDOUT` to Neon is still a separate failure mode. This Zoekt story explains the Cursor/`ctx_advisor` symptom better; it does not prove Neon was irrelevant for the worker tick.  4. *15-minute `GET /mcp`:* nothing in-repo sets `900000ms`. Railway/proxy lifetime is a reasonable hypothesis, not something this codebase confirms.  5. *`AbortError` is not retried.* If the 10s-abort tool path ever fires, retries stop. Irrelevant for advisor `codeSearch`, relevant if someone generalises the retry story to all search clients.  *Skeptical bottom line*  Calling this generically “MCP connection issues” does overfit. Evidence-shaped reading: *tool execution stalled on codesearch readiness*, while MCP was still accepting traffic. The proposed fixes (adaptive/longer warmup for large pin sets, avoid stampeding all cold shards, align client timeouts with retry budget, keep 401s and 15‑min GETs as separate buckets) are the right priority order.  One assumption still worth pressure-testing before you ship a warmup change: did that ~128s `POST /mcp` return a successful advisor answer, or a tool-error payload inside 200? That distinguishes “client gave up too early” from “server eventually failed after burning the retry budget.”

### Tom (@tom) · 2026-08-10T04:15:43.599Z

---

@U0AT5UDGTL1, I've rerun this with 5.6 Sol, and it did a deeper analysis into the Zoetk process, as we were getting a bunch of transient 503 logs that were fishy.

Here's its analysis:

The earlier conclusion was wrong. The 503s are connected to the MCP failure, but they are not Zoekt warmup timeouts.
Verified timeline:
• First MCP call: 54.5s; loaded ten shards.
• Second MCP call: 127.4s.
• During the second call,`/search`returned11 consecutive 503s, each in ~230ms.
• Warmup timeout is 8s, so those fast 503s cannot be warmup failures.
• Eleven attempts exactly matches one request plus`retries: 10`.
• Backoff delays total ~111s, explaining the apparent two-minute MCP hang.
• Codesearch had no restart, OOM, CPU pressure or private-network failure.
Actual problem:
1. Zoekt or the codesearch route rejects a search deterministically.
2. `search.ts`converts every Zoekt non-2xx response—including likely query-parser 400s—into generic 503.
3. The backend discards the response body and retries ten times.
4. Cursor times out while Railway eventually records HTTP 200 for the MCP JSON-RPC response.
The precise underlying Zoekt error is currently unknowable because both layers discard/mask it. A malformed model-generated Zoekt query is plausible, but not proven.
Correct fix order:
1. Preserve and log Zoekt’s upstream status and bounded response body.
2. Pass upstream 4xx through as 400; never retry invalid queries.
3. Add endpoint/operation context to retry logs.
4. Reduce interactive retries from 10 to 2–3 with a hard 20–30s budget.
5. Donotincrease warmup timeout or pin TTL yet; that would not fix this incident.
Also, `auth.identified: false` is not causal—it appears on successful MCP POSTs too. The earlier claim that it was Railway edge telemetry was inaccurate.
Targeted tests passed: 20/20.
The <vscode-file://vscode-app/Applications/Cursor.app/Contents/Resources/app/out/vs/code/electron-sandbox/workbench/74eed478-cd0e-4651-bbab-353cebd6424f|Zoekt audit> used coarse timing and inferred 8s warmup failures. Raw Railway logs obtained afterwards falsify that: every `/search` 503 took ~230ms, not 8s.
Its code-path analysis remains useful, but its root-cause conclusion and adaptive-warmup fix should be disregarded. The corrected fix order in my previous response stands.

### Cursor (@cursor) · 2026-08-10T04:16:07.382Z

Checking the revised fast-503 / status-masking story against `search.ts` and the retry client — specifically whether Zoekt 4xx can become retried 503s.

### Cursor (@cursor) · 2026-08-10T04:16:43.209Z

*The revised diagnosis holds.* The warmup story is falsified by the ~230ms 503s; the status-masking + retry-amplification story matches the code.  *What the code confirms*  1. *Warmup cannot be these 503s.* `ZOEKT_WARMUP_TIMEOUT_MS` is 8s. Eleven ~230ms failures are a different branch.  2. *Status collapsing is real.* In `search.ts`, any Zoekt non-2xx becomes:    ```ts    return c.json({ error: `Zoekt returned status ${res.status}` }, 503)    ```    So a Zoekt 400 (or anything else) is presented to the backend as a “transient” gateway error.  3. *Retries then burn the budget.* `codeSearch` uses `retries: 10` (11 attempts). Backoff sum is *111s* (+ tiny jitter); plus ~11×230ms ≈ *113.5s* of wait. That lines up with a ~127s MCP call once you add graph/LLM/DB around it.  4. *The real upstream reason is discarded.* Retry path does `await result.text()` then throws `transient HTTP 503` — body gone, no endpoint/operation context in the log. Codesearch also swallows pin/fetch exceptions into generic `"Zoekt webserver is unavailable"`.  5. *Raising warmup/pin TTL would not fix this incident.* Agree — don’t touch those yet.  *What is still not proven*  • *“Malformed model-generated Zoekt query”* is plausible, not established. The code makes that class of failure _look_ like infra 503s; it does not prove the query was bad. Could also be other fast Zoekt non-2xx, or the generic pin/fetch catch (also ~fast 503, also masked). • I cannot re-audit the Railway timings/`auth.identified` claim from this environment. Mechanistically, `auth.identified: false` as causal was already weak.  *Fix order — agree, with one sharpening*  1. Log Zoekt upstream status + bounded body (and log the swallowed pin/fetch errors). 2. Pass Zoekt *4xx → 400*; *do not retry* client errors. 3. Put endpoint/operation on retry logs. 4. Cut interactive search retries to 2–3 with a hard 20–30s budget. 5. Leave warmup/pin TTL alone for now.  Sharpening: today even the useful string `Zoekt returned status 400` dies in the retry helper. Fix (1)+(2) only works if the backend *stops treating all 503s as identical* _or_ codesearch stops emitting 503 for 4xx in the first place. Prefer fixing codesearch status mapping first so retries never see invalid queries as gateway failures.  Happy to implement that fix order next if you want.

### Tom (@tom) · 2026-08-10T05:05:06.530Z

The semi-frequenct worker-tick related error is partly related, but not the direct cause of the `/search` 503 loop.

What Railway confirms:
• OpenWorkflow had 20 Postgres TCP failures over 12 hours, including one at`04:29 UTC`.
• It connects to Neon’s pooled endpoint in`us-east-1`.
• All three IPv4 addresses timed out; IPv6 was unreachable.
• Backend also logged repeated`Connection terminated unexpectedly`.
• DB round-trip latency is ~220–240ms, strongly suggesting Railway and Neon are in different regions.
• No official Neon or Railway incident is currently reported.
This is a genuine shared infrastructure problem. It can disrupt workflow polling, authentication, conversation persistence and checkpoints. However, the specific codesearch 503 requests completed in ~230ms after successful DB queries, so those were not caused by these connection timeouts.

Claude Code is connecting successfully:
• OAuth completed at`01:38 UTC`.
• Claude Code received repeated MCP`200/202`responses through`04:19 UTC`.
• The observed 401 belonged to Codex, not Claude Code.
Therefore, “MCP unavailable” is actually tool execution failing or timing out after connection—principally the deterministic `/search` 503 plus the destructive ten-retry policy.

Recommended actions:
1. Verify Railway’s compute region; co-locate it with Neon`us-east-1`.
2. Check Neon compute/autosuspend events. Low usage can worsen stale-connection and cold-start behaviour.
3. Temporarily disable or lengthen Neon autosuspend for production.
4. Redeploy the worker to clear stale sockets—mitigation only.
5. Add proper reconnect handling and DB health telemetry.
6. Implement the earlier`/search`status/body preservation and reduce retries.
Low usage is irrelevant here: these are network, regional-placement and error-handling failures—not capacity exhaustion.

### Tom (@tom) · 2026-08-10T05:06:24.670Z

A lot here @U0AP1GU2FS4, not sure if the suggested fixes are spreading beyond a reasonable scope, but we have an issue with the MCP. Let me know your thoughts and I will continue with the agent.

### Tom (@tom) · 2026-08-10T06:07:12.109Z

A user just reported this from their session in Claude Code:

```ctx_advisor answers org-standards questions fine but drops the connection (Connection terminated unexpectedly) on every repo-specific query — 4 attempts, including a deliberately tiny one. So the org-standards half of your "ask ctx_advisor first" answer landed; the repo-context half isn't available. Proceeding from code and the running stack```
@U0AT5UDGTL1 is this helping us narrow down?

### Cursor (@cursor) · 2026-08-10T06:07:37.364Z

Checking whether that org-standards OK / repo-query fails split maps cleanly onto the advisor retrieval branches — and what actually emits `Connection terminated unexpectedly`.

### Cursor (@cursor) · 2026-08-10T06:08:01.072Z

*Yes — this narrows the MCP symptom hard.* It also creates a useful tension with the error string.  *What it rules in / out*  ``` | Hypothesis | Still live? | |---|---| | OAuth / “MCP unavailable” / session broken | **No** — org-standards answers prove the session and `ctx_advisor` entry work | | Failure on every `ctx_advisor` call | **No** — selective | | Repo-context retrieval branch | **Yes** — planner adds `code_search` for implementation/repo questions; standards lean `claim_aggregation` / graph / hybrid | | Query size / “too big” | **No** — tiny repo query failed too | | Zoekt `/search` 503 + 10-retry burn | **Still primary for the earlier Cursor incident** | | Neon `Connection terminated unexpectedly` | **Also live** — that string is our Postgres wording, not Zoekt’s | ``` *Why the split matches the code*  Org-standards questions are planned toward claim/graph channels. Repo-specific ones add `code_search` → `codeSearch()` → codesearch `/search` (and more parallel DB work with hybrid). So “standards work, repo fails” is exactly what you’d expect if the broken edge is on the code-search path or the heavier DB fan-out around it.  *The catch — don’t over-read the error text*  `Connection terminated unexpectedly` is the *Neon/pg* message (and what our transient DB retry is built for). A pure Zoekt 503 loop should surface more like `transient HTTP 503` / codesearch failure — unless: • Claude Code is paraphrasing a dropped MCP stream, or • the long `/search` retry holds the request open until a *checkpoint/DB* connection dies and _that_ is what the client sees.  So this report narrows *where* in the product it breaks (repo retrieval), not yet *which* of the two infra bugs is the proximate cause for this user.  *Scope advice (for the Jakub thread)*  Don’t expand into a regional migration + autosuspend project just to unblock MCP. Keep the blast radius tight:  1. *MCP-critical (do now):* stop masking Zoekt 4xx as 503; preserve status/body; cut interactive retries. That fixes the deterministic `/search` hang class. 2. *Shared infra (track separately):* Railway:left_right_arrow:Neon placement / stale sockets. Real, but not required to explain every `/search` 503. 3. *For this user report:* one Langfuse/backend trace for a failing repo `ctx_advisor` call will tell you immediately whether the thrown error is pg-terminate or exhausted 503 retries.  So: yes, it helps — it confirms “tool path after connect,” specifically *repo `code_search`*, and keeps the fix order honest instead of “MCP is down.”

### Tom (@tom) · 2026-08-20T04:18:23.892Z

@U0BMN61CASK capture
