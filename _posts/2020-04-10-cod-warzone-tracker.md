---
layout: post
title:  "Call Of Duty Warzone Tracker"
date:   2020-04-10 18:52:02 +0100
category: ["ServiceNow", "Custom App", "Call Of Duty"]
permalink: cod-warzone-tracker

---

[cod.tracker.gg] is a good service to see your call of duty warzone statistics and, luckily, it provides us rest api.

Starting from these I made an application to show some statistics of the players, for now killing, victory and k/d rate.

Before we get started, all players need to have the correct privacy settings
[https://cod.tracker.gg/modern-warfare/articles/how-to-public-stats]

Users must be inserted in the Warzone Users table (x_alli2_trackerwar_warzone_user ), and the statistics will be saved on the table itself. 

The Integration is based on a flow, which checks the statistics daily.

Here you can get the source code:
[https://github.com/mrwalakala/WarzoneTracker]

[https://github.com/mrwalakala/WarzoneTracker]: https://github.com/mrwalakala/WarzoneTracker
[https://cod.tracker.gg/modern-warfare/articles/how-to-public-stats]: https://cod.tracker.gg/modern-warfare/articles/how-to-public-stats
[cod.tracker.gg]: https://cod.tracker.gg/