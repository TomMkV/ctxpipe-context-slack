---
source: slack
channel_id: "C0AMVSR0EBW"
channel_name: "general"
is_private: false
thread_ts: "1787148733.040509"
team_id: "T0AMNSG03E1"
permalink: "https://ctxpipe.slack.com/archives/C0AMVSR0EBW/p1787148733040509?thread_ts=1787148733.040509&cid=C0AMVSR0EBW"
captured_at: "2026-08-20T02:14:26.970Z"
captured_by:
  handle: "tom"
  name: "Tom"
message_count: 7
participant_ids: ["U0AMKTFPG1H","U0AP1GU2FS4","U0BMN61CASK"]
oldest: "1787148733.040509"
latest: "1787191994.934499"
---

# Thread in #general

### Tom (@tom) · 2026-08-19T14:12:13.040Z

I'm noticing (in Cursor) more memory entries, which I assume is from the recent skill changes - however some of it looks buggy?

- [CleanShot 2026-08-20 at 00.11.26.png](https://ctxpipe.slack.com/files/U0BMN61CASK/F0BS41MMQE4/cleanshot_2026-08-20_at_00.11.26.png)
- [CleanShot 2026-08-20 at 00.11.32.png](https://ctxpipe.slack.com/files/U0BMN61CASK/F0BR3DBAC75/cleanshot_2026-08-20_at_00.11.32.png)

### jakub (@jakub) · 2026-08-19T14:14:37.748Z

Yes a bug, fixed in main. It was caused probably by changes how cursor fires hooks

### Tom (@tom) · 2026-08-19T14:14:56.762Z

I'll rerun npx ctxpipe init

### jakub (@jakub) · 2026-08-19T14:28:18.096Z

No need, updating from main should solve that. There may be some in flight captures that show up few times but should clear up 

### Tom (@tom) · 2026-08-19T14:55:15.862Z

@U0BMN61CASK capture this

### ctxpipe-dev (@ctxpipedev) · 2026-08-19T14:55:21.254Z

Engineering context capture failed. error:1E08010C:DECODER routines::unsupported

### Tom (@tom) · 2026-08-20T02:13:14.934Z

@U0BMN61CASK capture this
