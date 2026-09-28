---
source: linear
type: issue
id: 732dfceb-e244-46fa-a80e-2cd20ecc6a8b
identifier: CTX-56
title: Design and implement first-pass pricing page + Polar billing model
url: https://linear.app/ctxpipe/issue/CTX-56/design-and-implement-first-pass-pricing-page-polar-billing-model
state: In Progress
priority: Urgent
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
  - Research
  - Feature
labelIds:
  - 3fdd57cf-22f9-4a23-a316-f0f963e4b9ab
  - d41a0b47-f5a4-4521-aeb7-6b7b4ef7e559
createdAt: 2026-04-15T15:43:44.105Z
updatedAt: 2026-06-10T02:39:52.733Z
githubReferences: []
attachments: []
---

# CTX-56: Design and implement first-pass pricing page + Polar billing model

**Summary**
We need a first-pass pricing model and pricing page for **ctxpipe.ai** that reflects the product as **organisation-level engineering context infrastructure**, not a simple seat-based SaaS tool.

The commercial model should be:

* **base subscription per organisation**
* **included usage allowance**
* **usage-based expansion**
* **enterprise/custom deployment tier**

This aligns better with how ctxpipe creates value and incurs cost: ingestion, indexing, extraction, retrieval, chat/advisor usage, and enterprise deployment/governance requirements.

It also mirrors how adjacent products package value:

* Cursor combines plan access with usage concepts and enterprise packaging.
* LangSmith has paid plans that scale with usage volume rather than only seats.
* Polar supports fixed subscription products plus metered pricing and usage billing, which fits our desired model well.

---

## Context

From our current stack and privacy model, ctxpipe cost/value is driven by:

* ingestion from Git and Git-connected sources
* normalisation into a common ingestion pipeline
* indexing and graph extraction
* storage and sync/re-sync
* retrieval and advisor/chat flows
* LLM routing via OpenRouter
* enterprise trust requirements around hosting, retention, governance, and org isolation

This means **pure seat pricing is probably the wrong primary model**.

Instead, the external pricing model should likely be based on:

* **connected sources**
* **included monthly context operations**
* optional **enterprise deployment/governance add-ons**

“Context operations” is the blended public meter that hides internal complexity like retrieval, extraction, graph expansion, and model calls.

---

## Proposed pricing model

### Public pricing shape

| Plan | Who it’s for | Monthly price (USD) | Included sources | Included context ops / month | Hosting | Notes |
| -- | -- | -- | -- | -- | -- | -- |
| Sandbox | solo builders / eval | Free | 2 | 2,000 | Hosted | low-friction trial |
| Starter | small teams | 299 | 10 | 25,000 | Hosted | first real team plan |
| Team | growing eng orgs | 999 | 30 | 100,000 | Hosted | shared team usage |
| Scale | deeper adoption | 2,500 | 100 | 300,000 | Hosted / private cloud | better controls + volume |
| Enterprise | large / secure orgs | Custom | Custom | Custom | VPC / self-hosted / hosted | security, governance, rollout |

### Suggested overages / add-ons

| Meter | Suggested pricing |
| -- | -- |
| Additional connected source | $15/source/month |
| Additional 10,000 context ops | $40 |
| Private cloud add-on | from $1,000/month |
| Self-hosted minimum | from $30k–$75k ARR |

---

## Product/pricing principles

1. **Do not anchor pricing around seats**
   * seats can exist in packaging, but should not be the primary value metric
2. **Do not expose raw tokens as the commercial model**
   * customers want to buy reliable context infrastructure, not model bills
3. **Primary external metric should be simple**
   * recommended: **connected sources + monthly context ops**
4. **Enterprise tier is essential**
   * this is where private deployment, SSO, governance, retention controls, support, and custom integrations live

---

## Polar implementation direction

Use **Polar subscriptions + metered pricing**.

Recommended structure:

### Product structure

* Create one subscription product per plan:
  * Sandbox
  * Starter
  * Team
  * Scale
  * Enterprise (likely sales-led / manual)

### Fixed recurring component

* Each paid tier has a monthly recurring subscription price

### Metered component

* Add a metered price for **context operations**
* Optionally add a second meter later for **connected sources beyond included allowance**

Polar supports:

* subscription products
* metered prices on subscription products
* ingestion of usage events
* monthly or yearly usage aggregation/invoicing
* credits/prepaid usage patterns if we later want included allowances to behave as credits

### Recommended billing logic

* include a monthly allowance of context ops per plan
* record usage events from app/backend
* invoice overages monthly
* keep the public model simple even if internal cost attribution is more complex

---

## What needs to be built

### 1\. Pricing page

Create a pricing page for ctxpipe with:

* plan cards
* monthly pricing
* source allowances
* context ops allowances
* enterprise CTA
* short explanation of what a “context op” means
* FAQ section explaining why pricing is based on org infrastructure rather than seats

### 2\. Polar billing integration

Implement billing model in Polar:

* recurring subscription products
* metered billing for context ops
* checkout / subscription flow
* webhook handling for subscription state changes
* webhook/event ingestion for usage events
* customer billing visibility in app if practical

### 3\. Internal meter definition

Define exactly what counts as a billable **context op**.
Initial recommendation:

* retrieval/query against indexed knowledge
* chat/advisor request with retrieval
* graph expansion or context assembly step
* bounded ingestion extraction pass

This can be blended internally as one meter even if multiple backend operations contribute.

---

## Deliverables

* pricing page implemented
* Polar products/plans configured
* metered billing pipeline connected
* usage event schema defined
* first-pass FAQ and pricing copy added
* docs/comments in code explaining pricing assumptions

---

## Acceptance criteria

* user can view pricing page with the 5-tier structure above
* user can subscribe to a paid plan through Polar
* paid plans have recurring billing configured
* backend can send usage events for context ops
* usage over included allowance is metered
* enterprise plan has contact-sales path
* pricing copy clearly positions ctxpipe as an **organisation-level context layer for engineering agents**

---

## Notes for implementation

* keep first release simple; avoid too many public meters
* it is okay for the public model to expose only:
  * connected sources
  * context ops
* internally we can still map costs to:
  * ingestion
  * extraction
  * retrieval
  * storage
  * model routing
  * observability

---

## References

### Pricing/packaging references

* Cursor team pricing / models and pricing / enterprise packaging:
  [https://cursor.com/docs/account/teams/pricing](<https://cursor.com/docs/account/teams/pricing?utm_source=chatgpt.com>)
  [https://cursor.com/docs/models-and-pricing](<https://cursor.com/docs/models-and-pricing?utm_source=chatgpt.com>)
  [https://cursor.com/enterprise](<https://cursor.com/enterprise?utm_source=chatgpt.com>)
* LangSmith pricing / usage-oriented billing / billing controls:
  [https://www.langchain.com/pricing](<https://www.langchain.com/pricing?utm_source=chatgpt.com>)
  [https://www.langchain.com/langsmith/observability](<https://www.langchain.com/langsmith/observability?utm_source=chatgpt.com>)
  [https://docs.langchain.com/langsmith/billing](<https://docs.langchain.com/langsmith/billing?utm_source=chatgpt.com>)

### Polar implementation references

* Usage-based billing intro:
  [https://polar.sh/docs/features/usage-based-billing/introduction](<https://polar.sh/docs/features/usage-based-billing/introduction?utm_source=chatgpt.com>)
* Billing behaviour:
  [https://polar.sh/docs/features/usage-based-billing/billing](<https://polar.sh/docs/features/usage-based-billing/billing?utm_source=chatgpt.com>)
* Credits/prepaid usage:
  [https://polar.sh/docs/features/usage-based-billing/credits](<https://polar.sh/docs/features/usage-based-billing/credits?utm_source=chatgpt.com>)
* Subscription products / tiered plans:
  [https://polar.sh/features/products](<https://polar.sh/features/products?utm_source=chatgpt.com>)

## Comments

### 2026-06-10T02:39:52.772Z · tom

@jakub / @vietanhtran.dev [https://github.com/ctxpipe-ai/payments](<https://github.com/ctxpipe-ai/payments>)

No work done on the app-side to hook into the payments system. Need some guidance here on direction.

### 2026-04-15T15:44:31.674Z · tom

Hey @jakub  - this is one big-ass summary of research I've been doing. Let's discuss it and see where we land. I'd like to use Polar, but open to anything.
