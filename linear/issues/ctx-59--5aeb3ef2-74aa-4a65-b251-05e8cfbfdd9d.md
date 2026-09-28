---
source: linear
type: issue
id: 5aeb3ef2-74aa-4a65-b251-05e8cfbfdd9d
identifier: CTX-59
title: Fix ReadMe work
url: https://linear.app/ctxpipe/issue/CTX-59/fix-readme-work
state: Done
priority: High
team: Ctxpipe
teamKey: CTX
teamId: 080a5a17-fa66-45af-9009-35d46eed1fc9
project: Product development
projectId: 29b1bae6-2c19-4e9f-b8b0-4a7a2bd28b3e
cycle: null
cycleId: null
assignee: tom
assigneeId: 4b501e62-2fb1-40f2-b918-10505ad31010
creator: tom
creatorId: 4b501e62-2fb1-40f2-b918-10505ad31010
labels:
  - Bug
  - Docs
  - Improvement
labelIds:
  - 81bb09c4-a8eb-41b2-ac0a-bd2168a227d4
  - e0063282-6b2e-4e71-8ed5-b36616ae0bc0
  - 66a2e21f-a34f-4439-af5b-39e2a103e3f5
createdAt: 2026-04-16T10:40:08.094Z
updatedAt: 2026-04-17T11:32:54.414Z
githubReferences:
  - kind: pull_request
    url: https://github.com/ctxpipe-ai/ctxpipe/pull/109
    title: "docs: fix README separation (root vs backend) — CTX-59"
attachments: []
---

# CTX-59: Fix ReadMe work

Recent work done in the `readme-styling` branch was accidentally done in the /backend ReadMe file, not the one at root - which was the purpose of the ticket.

Please review the content in both the backend's ReadMe and the root ReadMe, and make the root to be more visually styled and public and the backend to be less root-like (licensing, branding etc).

Consider the content, too - and ensure there is a clear separation of concerns.

## Comments

### 2026-04-16T10:43:34.086Z · cursor

[**Readme files separation**](<https://www.cursor.com/agents/bc-e0082f60-b8ea-4551-a715-62b48f27793e>)

Here is what was wrong and what we changed.

**Assumption to check:** The old backend README pointed at `../docs/public/ctxpipe-logo-readme.png`, but there is no `docs/public` in this repo (and no PNG in the search). That would have shown a broken image on GitHub. The root README now uses `apps/ui/public/ctx_.svg`, which exists. If you want the wide marketing PNG back, add it to the repo and point the `<img>` at that path.

### What I did (CTX-59)

1. **Root** [`README.md`](<https://github.com/ctxpipe-ai/ctxpipe/blob/cursor/fix-readme-separation-793e/README.md>) — public, “landing” style: centred logo, ELv2 badge, Website / Issues / Docs links, **architecture diagram** (`apps/ui/public/images/ctxpipe-onboarding-diagram.svg`), product blurb, docs + local dev + Compose + scripts table, and a **Licence** section with the docs link.
2. `apps/backend/README.md` — **package-only**: title `@ctxpipe/backend`, one-line role, pointer to the **root** README for monorepo clone/dev/Compose, then stack, API/MCP/webhooks, **backend** scripts table, `src/` layout, and a short ELv2 pointer (no duplicate branding, no full-stack Docker quick start, no duplicate diagram).

Draft PR: [**https://github.com/ctxpipe-ai/ctxpipe/pull/109**](<https://github.com/ctxpipe-ai/ctxpipe/pull/109>) (branch `cursor/fix-readme-separation-793e`).

**Repository:** `ctxpipe-ai/ctxpipe`
**Branch:** `cursor/fix-readme-separation-793e`

[View PR](<https://github.com/ctxpipe-ai/ctxpipe/pull/109>) · [Open in Desktop](<https://cursor.com/background-agent?bcId=bc-e0082f60-b8ea-4551-a715-62b48f27793e>) · [Open in Web](<https://www.cursor.com/agents/bc-e0082f60-b8ea-4551-a715-62b48f27793e>)

### 2026-04-16T10:40:10.544Z · unknown

This thread is for an agent session with Cursor. [View on Cursor →](https://www.cursor.com/agents/bc-e0082f60-b8ea-4551-a715-62b48f27793e)
