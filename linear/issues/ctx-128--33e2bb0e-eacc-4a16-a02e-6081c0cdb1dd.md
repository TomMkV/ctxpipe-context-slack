---
source: linear
type: issue
id: 33e2bb0e-eacc-4a16-a02e-6081c0cdb1dd
identifier: CTX-128
title: Close learning loop
url: https://linear.app/ctxpipe/issue/CTX-128/close-learning-loop
state: Todo
priority: Urgent
team: Ctxpipe
teamKey: CTX
teamId: 080a5a17-fa66-45af-9009-35d46eed1fc9
project: Product development
projectId: 29b1bae6-2c19-4e9f-b8b0-4a7a2bd28b3e
cycle: null
cycleId: null
assignee: jakub
assigneeId: bf21aa8a-4c82-447c-9af7-94b0e739ec50
creator: tom
creatorId: 4b501e62-2fb1-40f2-b918-10505ad31010
labels:
  - Improvement
labelIds:
  - 66a2e21f-a34f-4439-af5b-39e2a103e3f5
createdAt: 2026-06-10T02:38:29.740Z
updatedAt: 2026-06-10T02:38:30.975Z
githubReferences: []
attachments: []
---

# CTX-128: Close learning loop

We need to enable the trickling down to agent learnings to other agents.

@jakub this needs better definition - I don't fully understand where the loop gaps are.

1. Agents pull from the graph via the ctx_advisor MCP
2. Agent can create memories via the local memory skill and hooks
3. These memories are committed and in the PR
4. Agents pull these down and benefit from it? 

What am I missing?
