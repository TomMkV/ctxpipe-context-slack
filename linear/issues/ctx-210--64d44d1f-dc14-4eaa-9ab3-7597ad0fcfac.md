---
source: linear
type: issue
id: 64d44d1f-dc14-4eaa-9ab3-7597ad0fcfac
identifier: CTX-210
title: Slack notification uses the document path as the message text
url: https://linear.app/ctxpipe/issue/CTX-210/slack-notification-uses-the-document-path-as-the-message-text
state: In Review
priority: Medium
team: Ctxpipe
teamKey: CTX
teamId: 080a5a17-fa66-45af-9009-35d46eed1fc9
project: Linear Test
projectId: effac04a-cdfa-4e7c-98f3-8af1b34cd44d
cycle: null
cycleId: null
assignee: jakub
assigneeId: bf21aa8a-4c82-447c-9af7-94b0e739ec50
creator: tom
creatorId: 4b501e62-2fb1-40f2-b918-10505ad31010
labels:
  - Bug
labelIds:
  - 81bb09c4-a8eb-41b2-ac0a-bd2168a227d4
createdAt: 2026-09-28T12:26:34.173Z
updatedAt: 2026-09-28T12:26:34.173Z
githubReferences: []
attachments: []
---

# CTX-210: Slack notification uses the document path as the message text

The unfurl posts the git path instead of the title, so the channel cannot tell what changed.

## Acceptance

* The message uses the document title.
* The path is in the context line, not the headline.
