---
layout: post
title:  "Bookmark Groups"
date:   2019-09-18 19:39:45 +0100
permalink: bookmark-groups

---
In ServiceNow it’s possible to create groups of bookmark

![bookmark group layout](/assets/bookmark_group_00.png)

To do this it is necessary to have the admin role

The required steps are the following:
1. Type “sys_ui_bookmark_group.list” on application menu
2. Create a new record with the desired name and yourself as user

![bookmark group list](/assets/bookmark_group_01.png)

3. Type “sys_ui_bookmark.list” on application menu
4. Show the field “Group” in list
5. Set in the "group" field the record previously created for the bookmarks that you want to group
![bookmark layout](/assets/bookmark_group_02.png)

This functionality was illustrated in [TechNow #68][technow-68]


[technow-68]: https://community.servicenow.com/community?id=community_blog&sys_id=396ef5e71bebbf84fff162c4bd4bcb8e
