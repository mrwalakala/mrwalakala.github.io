---
layout: post
title:  "Planning console in London and Madrid"
date:   2019-04-03 21:34:20 +0100
permalink: planning-console-client-setup
---

From the London version is available the new release of planning console, entirely client side.

**How to enable it**
It can be done by using the object “Planning Console” (pm_console) from the menù "Project Administration" => "Settings" => "Planning Console" and by setting the field “Enable Client Side Planning” to true.

The record that identifies the planning console has the entity field set to “Project” and the context field set to “default”

![planning console enable](/assets/planning_console_client_setup_00.png)

NB: It’s necessary to check the property "com.snc.project.fire_brs_from_planning_console". 
If the property is set to true you’ll never able to use planning console client side


**Trigger business rule**
The new planning console doesn’t trigger the business rule immediately, but it’s possible to enable the business rule field by field.

Referring to the table “Planning Console Display Column” (you can reach it by using the related list form “Planning Console” object), it’s possible to see all the available fields and, by changing the value of the field "Fire BR on Save", trigger the business rule.

In the fields where “Fire BR on Save” is set as true, the planning console will save immediately the entire project in case of field change.

Still concerning the object “Planning Console Display Column”, it’s possible to change some settings of fields visualization, for example:

Allow the user to hide a field  => "Can Hide"
Allow the user to sort results by field value => "Sortable" (not always available)
Trigger the duration of the project recalculation => "Trigger recalculation"


**Useful link**

[Planning Console Admin Guide (HI)][planning-console-admin-guide]

[Planning Console Client docs.servicenow.com][planning-console-docs-sn]

[planning-console-admin-guide]: https://hi.service-now.com/kb_view.do?sys_kb_id=bc2122addb49ab08feb1a851ca961980&sysparm_rank=4&sysparm_tsqueryId=15e7fda4db956b805ed4a851ca96190f
[planning-console-docs-sn]: https://docs.servicenow.com/bundle/madrid-it-business-management/page/product/project-management/concept/client-side-planning-console.html