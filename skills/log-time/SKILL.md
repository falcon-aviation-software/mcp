---
name: log-time
description: After logging time or starting a timer on a FalconMX Work Item, check whether the Customer has been sent the Estimate lines for it, as Juliette does. Use whenever the user logs time, adds hours or starts or stops a timer on a Work Item.
---

# Logging time

Leave a time entry's day out to mean today at the Work Order's Location. Labor is in tenths of an hour.

After you log time or start a timer on a Work Item, read the Estimate (`estimates getEstimate`) and look at the lines whose `workItemId` is that Work Item:

- If one has no answer and `sent` is false, the Customer hasn't seen it, so your reply must say so: name the line, say the Customer hasn't been sent it yet, and ask whether to send them the Estimate. Send it (`estimates sendEstimate`) only when the User says to.
- If those lines are sent or answered, or there are none, say nothing about the Estimate.
