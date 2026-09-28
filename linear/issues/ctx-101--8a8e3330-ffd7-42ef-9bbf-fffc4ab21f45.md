---
source: linear
type: issue
id: 8a8e3330-ffd7-42ef-9bbf-fffc4ab21f45
identifier: CTX-101
title: Can't connect Github during onboarding flow
url: https://linear.app/ctxpipe/issue/CTX-101/cant-connect-github-during-onboarding-flow
state: Canceled
priority: High
team: Ctxpipe
teamKey: CTX
teamId: 080a5a17-fa66-45af-9009-35d46eed1fc9
project: Product development
projectId: 29b1bae6-2c19-4e9f-b8b0-4a7a2bd28b3e
cycle: null
cycleId: null
assignee: vietanhtran.dev
assigneeId: b18de5ef-1f2c-42b9-8520-d767be2640a1
creator: tom
creatorId: 4b501e62-2fb1-40f2-b918-10505ad31010
labels:
  - Bug
  - Self host
labelIds:
  - 81bb09c4-a8eb-41b2-ac0a-bd2168a227d4
  - 80d42cb6-db9b-4e36-b05b-64aeb4f1d668
createdAt: 2026-05-09T04:22:43.851Z
updatedAt: 2026-05-29T07:10:13.004Z
githubReferences: []
attachments: []
---

# CTX-101: Can't connect Github during onboarding flow

The GitHub app pops up for me to select repos, but the callback just resets the 'Connect GitHub page' to say 'Connect Github'. I've tried a dozen times and it's caught in a loop.

Connect Github -> Opens select org popup -> Select repo(s) -> Return to ctx| -> 'Connect Github'

## Comments

### 2026-05-29T07:10:10.590Z · tom

This is an issue in preview branches only, despite callback in the Github app. It works in prod.

### 2026-05-22T10:45:53.367Z · cursor

[**Github onboarding connection**](<https://www.cursor.com/agents/bc-1de10d61-deb6-40f4-a2b7-f197609f5c76>)

## Diagnosis

The loop was not random OAuth flakiness. Two separate failures stacked:

1. **Backend resolution** — Self-hosted setup can leave draft/placeholder `connections` rows. After a successful GitHub App install, an org could have both a **linked** row (`installationId` set) and **draft** rows. `resolveGithubInstallationForOrgDetailed` treated *any* multiple rows as ambiguous, so `GET /github/installation` returned **400**. The UI query failed and onboarding stayed on “Connect GitHub” even though registration had worked. That fits “tried a dozen times” (more draft rows, still ambiguous).
2. **Popup callback timing** — The callback wrote `installation_id` to `localStorage` in `useEffect` then closed. The opener could read storage **before** the write finished → `no_result` → silent reset with no error message.

## Fix (PR #196)

Draft PR: [https://github.com/ctxpipe-ai/ctxpipe/pull/196](<https://github.com/ctxpipe-ai/ctxpipe/pull/196>)

| Area | Change |
| -- | -- |
| **Backend** | `resolveGithubInstallationFromList` prefers rows with `installationId`; ambiguous only when multiple *linked* or multiple *unlinked* rows |
| **Popup** | Synchronous `localStorage` write, `sessionStorage` popup marker, short poll before opener registers |
| **UI** | `isGithubInstallationLinked()` — “connected” only when `installationId` is set |

## Tests added

* `apps/backend/src/models/github-installation.resolve.test.ts`
* Updated `githubConnectFlow.test.ts`

## Skeptical note

If someone’s GitHub App **Setup URL** points at the wrong host (not their `AUTH_BASE_URL`), they can still loop — that’s configuration, not this code path. Worth confirming Setup URL matches the deployment origin for self-hosted installs.

Manual check: onboarding → Connect GitHub → finish install in popup → step should advance or show connected, including when a draft connector row already exists from an aborted self-hosted wizard.

**Repository:** `ctxpipe-ai/ctxpipe`
**Branch:** `cursor/fix-github-onboarding-callback-loop-5c76`

[View PR](<https://github.com/ctxpipe-ai/ctxpipe/pull/196>) · [Open in Desktop](<https://cursor.com/background-agent?bcId=bc-1de10d61-deb6-40f4-a2b7-f197609f5c76>) · [Open in Web](<https://www.cursor.com/agents/bc-1de10d61-deb6-40f4-a2b7-f197609f5c76>)

### 2026-05-22T10:38:06.160Z · unknown

This thread is for an agent session with Cursor. [View on Cursor →](https://www.cursor.com/agents/bc-1de10d61-deb6-40f4-a2b7-f197609f5c76)
