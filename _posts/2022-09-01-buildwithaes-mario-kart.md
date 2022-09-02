---
layout: post
title:  "#BuildWithAES and Mario Kart"
date:   2022-09-01 20:04:13 +0100
category: ["ServiceNow", "AES", "Mario Kart"]
permalink: buildwithaes-mario-kart

---

Hey, I'm here for [https://community.servicenow.com/community?id=community_blog&sys_id=8ea8e43f1bb81d10b09633f2cd4bcb8f&view_source=featuredList][#buildwithaes] too!

But... what is AES? 
AES is a development tool for creators, regardless the skills level.

AES can be used to create custom application, and work in the end is not only ITSM, CSM and all the rest.
Sometimes there is also room for recreation!

That's why we occasionally organise company Mario Kart tournaments 😎

But. ServiceNow does not have an OOTB application to support them. Very bad.

It therefore seemed  fair to fix it, thanks to AES!

<img src="/assets/buildwithaes-00.png" alt="" />

Thanks to AES, I started immediately by creating four tables:
- circuit
- participant
- match
- leaderboard

<img src="/assets/buildwithaes-01.png" alt="" />

I have created the relevant application menus, the necessary fields and a dedicate Service Portal (/mk).


To make data entry easier, I also created three catalogue items, again via aes. In five minutes.

<img src="/assets/buildwithaes-02.png" alt="" />

<img src="/assets/buildwithaes-03.png" alt="" />

The catalogue item "Register as participant" has a special feature, it is public! I followed the KB [https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB0681861][KB0681861]

This way, users not in ServiceNow can also join the tournament.

To avoid spam and other problems, there is an approval flow. Also created via aes in five minutes.
Each participant must be approved and, only after the approvation, he can be added in a match!



How it's possible to register a match?
It is possible using the catalogue item mentioned earlier and, an admin user, can enter the result.

Once the match is completed, the result of each participant can be recorded.

All tournament information is displayed on the homepage of the service portal /mk.
This way it could be shown on a large monitor!

All this was done in about two hours using AES (and a couple of configurations on the instance for the public catalogue item).

These are some functions that would be cool to implement:
- Mobile Application and upload photo of the result
- Set-up full tournament setup and score calculation

