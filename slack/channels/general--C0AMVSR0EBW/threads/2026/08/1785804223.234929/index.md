---
source: slack
channel_id: "C0AMVSR0EBW"
channel_name: "general"
is_private: false
thread_ts: "1785804223.234929"
team_id: "T0AMNSG03E1"
message_count: 11
participant_ids: ["U0AMKTFPG1H","U0AP1GU2FS4"]
oldest: "1785804223.234929"
latest: "1786582678.258929"
---

# Thread in #general

### Tom · 2026-08-04T00:43:43.234Z

From Tom: [designsystemdocspec.org](https://designsystemdocspec.org/)

### jakub · 2026-08-04T01:04:09.829Z

interesting, however from very quick look I don't see what is it good for in the age of agents.
it can work as some intermediate data shape during transformations/generations but I don't see why that would be needed now. It was before, but not now.

re their core principles:

#1
> Information is more valuable when it's portable.Tools change. Needs change. Budgets change. A design system's source of truth should survive any rebuild, reorg, or rethink.
The thing is that agents are really good at converting specs from one format to another, so you can use the one that is the most convenient at the time because it's always portable so #1 isn't a valid reason anymore.

#2
> Documentation shouldn't have to pick a side.Humans, parsers, and agents all need the same docs. Teams shouldn't have to write a separate version for each. This standard works as the source of truth for all of them.
Humans can't read json well, it's good for parser, but also expensive/inefficient for agents. So 2 out of 3 parties are underserved by this solution. An alternative MDX is markdown, which means readable by humans, not as easy to parse as json but still doable, and it's very efficient for agents. So mdx serves all 3 not one. Which means their point #2 isn't valid either

#3
> A format that grows with you.Getting started should be easy. A docs standard should also grow as your system's needs grow.
Any structured format that is effectively a DSL is inherently limited, sure you can extend it, but then you're managing extensions. So in practice any structured format has risk of outgrowing it. That's why markdown works, it's just plain unstructured documents where you can add things overtime (headers for structure, frontmatter for metadata, ...). So the point #3 is actually also invalid

### jakub · 2026-08-04T01:07:10.271Z

an example you can take a look on approach that [design.md from google did](https://github.com/google-labs-code/design.md). It's just markdown with frontmatter. The execution there isn't great (primarily lack of composability) but it does capture much better the direction documentations need to move

### Tom · 2026-08-04T01:28:32.735Z

Very good points. Points 1 and 2 def something I picked up from our discussions yesterday

### Tom · 2026-08-04T01:28:40.005Z

From Tom just now:
```I discussed about the complexity of scaling a context layer when coming from different sources and repo, and the manager Alex said that he would be happy to here more about ctxpipe```

### Tom · 2026-08-04T02:15:38.900Z

_(empty)_

- [image.png](https://ctxpipe.slack.com/files/U0BMN61CASK/F0BMV4DTMKK/image.png)

### jakub · 2026-08-04T02:18:05.192Z

that is sensible, I think lightweight LLM wiki is the exact thing they need right now

### jakub · 2026-08-04T02:18:26.429Z

and when they outgrow it (will be quick) we can help them go beyond

### Tom · 2026-08-04T03:08:58.591Z

Yep. Talking to him now

### Tom · 2026-08-04T05:18:49.063Z

So Tom is really keen to get us in, as the longer he looks into it, the more he feels both he and his team are incapable of doing a POC correctly.

However, politics and enterprise process. Of course.

So:
1. Getting us in as advisors isn't easy, as it would go through a different person, and, he mentions us so much (and they know we're mates) - it looks like he is just selling ctxpipe. This may work in the future, but not right now.
2. He needs to do a POC and prove it out and show the scaling issues first. It's inevitable, despite us being a shortcut. 
3. We will stay in touch and offer support. He shares our thoughts and feedback (and links) directly with his team. So we're getting eyeballs


### Tom · 2026-08-13T00:57:58.258Z

@U0BMN61CASK capture this
