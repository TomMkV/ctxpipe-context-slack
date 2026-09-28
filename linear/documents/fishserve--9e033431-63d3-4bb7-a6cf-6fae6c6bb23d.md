---
source: linear
type: document
id: 9e033431-63d3-4bb7-a6cf-6fae6c6bb23d
title: Fishserve
url: https://linear.app/ctxpipe/document/fishserve-800c21339e55
projectId: 29b1bae6-2c19-4e9f-b8b0-4a7a2bd28b3e
creatorId: 4b501e62-2fb1-40f2-b918-10505ad31010
createdAt: 2026-03-23T11:15:20.427Z
updatedAt: 2026-03-23T11:20:06.071Z
---

# Fishserve

[**FishServe**](<https://fishserve.co.nz/>) (Commercial Fisheries Services Ltd) is **a private New Zealand company, acting as the official Approved Service Delivery Organisation (ASDO) for the Ministry for Primary Industries since 1999**. They provide essential administrative, registry, and IT services to the commercial fishing industry to support the Quota Management System (QMS) and ensure sustainability

---

**Notes from kick off meeting 23/03/26:**

**System Architecture & Technical Challenges**

* Legacy .NET Framework application (1M lines, 10 years old)
  * 32 microservices in distributed monolith architecture
  * 16 web apps + 16 async service apps with message queues
  * Massive shared code dependencies affecting all services
* 15 months of modernization effort showing progress
  * Yellow nodes in diagram = .NET Core migration (future state)
  * Still significant interconnectedness and “spidey sense” risk areas
* Team composition: 6 people + Mark (partial)
  * 3 engineers, Dan (product manager), 2 BA/Testers
  * Longest-tenured engineer only 1 year (Mark rebuilt team after redundancies)
  * Mark has deep system knowledge, Dan has patchy but valuable perspective

**AI Development & Current Tooling**

* 6 months serious AI adoption (Mark/Dan), 6 weeks team-wide
* Current tools: Code Rabbit for PR reviews, basic repo-pointing agents
* Major concern: “Speed wobbles” from rapid AI-enabled changes
  * Easy to change things quickly but blast radius poorly understood
  * Team lacks tools to prevent “shooting themselves in the foot”
* Documentation gaps: 99% knowledge in heads, 1% in markdown
  * Zero documentation 3 years ago, some Confluence now but feels like “island”
* Repository size: \~40M tokens (too large for single context window)

**Context Pipe Value Proposition Discussion**

* Primary need: “Safety at speed” - move fast while minimizing risk
* Key challenges Context Pipe could address:
  * Surface right knowledge at right time without information overload
  * Connect fragmented knowledge across Confluence, Azure DevOps, code
  * Onboard new team members more effectively
  * Reduce bus factor (Mark/Dan knowledge dependency)
* Interest in graph-based understanding of system interconnections
  * Static compile-time view vs dynamic runtime view mapping
  * Understanding blast radius of changes before making them

**Technical Integration Requirements**

* Hosting: Entirely Azure-based (mix of App Services PaaS + VMs)
  * 100% Terraform infrastructure (recently eliminated click-ops)
  * Willing to spin up VM short-term or use hosted solution
* Data sources priority:
  1. Git repositories (primary value)
  2. Confluence documentation
  3. Future: Azure DevOps work items, Raygun/App Insights monitoring
* Security: Medium confidentiality source code, need info security process understanding

**Next Steps**

* Context Pipe team to provide integration timeline
* Set up regular check-ins (fortnightly/monthly)
* Start with Terraform as potential “baby steps” testing ground
  * Well-structured, newer codebase
  * Team needs upskilling in this area
  * Lower risk than main application code
