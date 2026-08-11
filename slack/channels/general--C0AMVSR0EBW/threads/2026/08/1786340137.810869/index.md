---
source: slack
channel_id: "C0AMVSR0EBW"
channel_name: "general"
is_private: false
thread_ts: "1786340137.810869"
team_id: "T0AMNSG03E1"
message_count: 16
participant_ids: ["U0AMKTFPG1H","U0AT5UDGTL1","U0BMN61CASK"]
oldest: "1786340137.810869"
latest: "1786455425.547369"
---

# Thread in #general

### Tom · 2026-08-10T05:35:37.810Z

Another bug to query. Someone in the Tru org tried using the CLI to auth their Claude session to the ctxpipe MCP - and the CLI auto-chose a gmail account and prompted them to add an org slug. This would create a mismatch if the user forgot any of these details. Shouldn't we force the user to log into the platform first via the URL?

- [image.png](./assets/F0BP60SKV0R--image.png)

### Tom · 2026-08-10T05:38:33.225Z

And once the user got through this, they saw that only one of the MCPs is authenticated. See pic:

They then asked:
"that worked, have a second mcp on claude code rn, is that the one i should use?"
Can we add some hint text to explain one is remote graph and the other is local memory?

Also, the user said: "ok can auth the first mcp now, had to paste the localhost url back into c to complete the auth"

- [image.png](./assets/F0BP7P837C4--image.png)

### Tom · 2026-08-10T05:42:12.654Z

@U0AT5UDGTL1 what are some UX improvements here we can make on the CLI auth flow? Should we make signing into [ctxpipe.ai](http://ctxpipe.ai) a mandatory step to ensure a handoff match with user credentials? How do we ensure both MCPs get authenticated together? Can we simplify this flow to be easier for end users?

### Cursor · 2026-08-10T05:42:51.417Z

Looking into the CLI auth / MCP setup flow now — I'll dig into how device login, org selection, and the two MCPs interact, then come back with concrete UX recommendations.

### Cursor · 2026-08-10T05:44:39.649Z

Short answer: *no, do not make a prior visit to [ctxpipe.ai](http://ctxpipe.ai) mandatory.* The device URL already forces a web session. The Tru friction is silent account selection, dual auth, and opaque MCP labels — not a missing “please log in first” step.  *What’s actually going wrong*  1. *CLI setup auth ≠ agent MCP OAuth.* `npx ctxpipe init` stores a _setup_ token (orgs, memory models). Remote `ctxpipe` still needs a _separate_ OAuth consent inside Claude/Cursor. Memory is local stdio and often needs no auth. So one green check + one “needs authentication” is expected — and currently unexplained. 2. *Gmail auto-pick is a device-page problem.* With a prefilled code + existing session, `/.auth/device` auto-approves. Google may reuse the wrong account with no “Signed in as X / Not you?” gate. 3. *Org mismatch is real, but typed slug isn’t the only path.* `init` loads membership orgs; `mcp add` still takes free-text org with no login. Membership checks on `/mcp` are also weak (proposal already flags this).  *Should we force platform login first?*  I’d push back. Assumptions that don’t hold: • That users skip web login today (they don’t — device approval requires it). • That pre-visiting the site fixes wrong Google account (it doesn’t; silent session reuse gets worse). • That it makes both MCPs “auth together” (different credential stores; clients own remote OAuth).  Better: keep device flow, but *never auto-approve without an explicit identity confirmation*.  *Can both MCPs auth together?*  Not honestly as one step — different transports (remote OAuth vs local stdio). What we _can_ do is make the _handoff_ feel like one setup:  • After CLI init, state clearly: *memory is ready; remote ctx| still needs one agent approve.* • Brand `/.auth/consent` as “Authorize ctx| for {org}” instead of generic “Authorize Client”. • Add MCP hint/description text so Claude’s list reads like:   • `ctxpipe` — remote graph / repos / advisor (needs browser auth)   • `ctxpipe-memory` — local `.ai/memory` (no browser auth) • Optionally open/preflight the remote MCP URL once from the CLI so the first agent auth is less surprising (won’t eliminate localhost callback paste — that’s often the client).  *Highest-leverage UX fixes (ranked)*  1. *Device page: show email + “Not you?” before approve* (kill silent Gmail handoff). 2. *Post-init copy:* spell out the two-step auth; don’t imply setup = MCP done. 3. *MCP labels/descriptions* in written configs + server titles. 4. *`mcp add` should reuse device login + org picker*, not free-text slug. 5. *Consent page branding* with org context. 6. *Enforce org membership on `/mcp`* so wrong-slug fails loudly. 7. *Doctor check* for org drift between `.ctxpipe/config.json` and MCP URL.  Simplest end-state for users: `npx ctxpipe init` → confirm account → pick org → agents restart → one clear “Authorize remote ctx|” prompt. Memory should never look like a second mystery product.  Want me to implement (1)–(3) as a first PR? That’s the biggest clarity win without redesigning auth.

### Tom · 2026-08-11T12:33:13.636Z

@U0BMN61CASK capture this

### Tom · 2026-08-11T12:59:27.528Z

@U0BMN61CASK capture this thread please

### Tom · 2026-08-11T13:00:17.853Z

@U0BMN61CASK capture this

### Tom · 2026-08-11T13:07:04.511Z

@U0BMN61CASK capture this pelase

### Tom · 2026-08-11T13:07:25.956Z

Hey @U0BMN61CASK capture this thread

### Tom · 2026-08-11T13:16:49.636Z

Hey @U0BMN61CASK grab this thread

### Tom · 2026-08-11T13:17:09.365Z

Hey @U0BMN61CASK capture this thread

### Tom · 2026-08-11T13:17:50.226Z

Hey @U0BMN61CASK capture this thread

### ctxpipe-dev · 2026-08-11T13:17:54.484Z

ctx| agent capturing engineering context…

### Tom · 2026-08-11T13:18:09.982Z

Thank god

### Tom · 2026-08-11T13:37:05.547Z

@U0BMN61CASK capture this thread
