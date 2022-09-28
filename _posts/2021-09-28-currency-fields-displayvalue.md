---
layout: post
title:  "Make ServiceNow international (for real): Display currency fields in a proper format"
date:   2022-09-29 19:12:29 +0100
category: ["ServiceNow", "Translate"]
permalink: currency-fields-displayvalue

---


Managing the different ways of displaying the information contained in currency fields can be really annoying.
And potentially harmful if done too late. But let's go step by step.


ServiceNow OOTB displays economic amounts in U.S. format.
So 25480 € and 55 cents are shown as 25,480.55

<img src="/assets/currency-fields-displayvalue-00.png" width="25%"/>

Odd format for a European.
An Italian would expect the amount shown as 25,480.55

<img src="/assets/currency-fields-displayvalue-01.png" width="25%"/>

A Frenchman as 25 480.55

<img src="/assets/currency-fields-displayvalue-02.png" width="25%"/>


How can this be handled?

ServiceNow allows location settings to be configure at two different levels.


## System Level
The glide.system.locale property allows you to set the appropriate locale code.
Where do we find the various locale codes?

Here ( [https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB0695326][sn-support-combination] ) is a list of the most commons combinations.
Format is [language code].[country code]


So if you want to use the Luxembourg display as your default locale, you can use de.LU (or if you prefer the francophone one fr.LU)


**BE AWARE**
Do not change the system locale after currency values have been entered into the instance. When you change the system locale, the reference currency values are not adjusted. There is no rate conversion. This persistence results in invalid aggregations and filtering.

[docs reference][sn-docs-reference-update]


## User Level
The same settings can be applied at the individual user level using the Country and Language fields.

Both are normal choice fields, so you can add new countries/languages

<img src="/assets/currency-fields-displayvalue-03.png" width="40%"/>

Keep in mind that user-level settings are prioritized over system-level settings




[sn-support-combination]: https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB0695326
[sn-docs-reference-update]: https://docs.servicenow.com/bundle/tokyo-platform-administration/page/administer/currency/concept/locales.html 