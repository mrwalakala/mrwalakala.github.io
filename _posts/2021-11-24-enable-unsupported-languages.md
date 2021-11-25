---
layout: post
title:  "Make ServiceNow international (for real): Enable unsupported languages"
date:   2021-11-24 19:12:29 +0100
category: ["ServiceNow", "Translate"]
permalink: enable-unsupported-languages

---

Let's start with an important distinction: if the language you are interested is [one of this in the list][i18n-plugin-language], all you have to do is install the dedicated plugin and you won't need to do anything else.



If it is not listed (as in the case of Luxembourgish 🇱🇺, for example), a few additional steps are required to make it selectable for users.


To begin with, install the plugin "I18N: Internationalization" (id com.glide.i18n)



<img src="/assets/enable-unsupported-languages-00.png" alt="plugin-install" width="75%"/>

After installation navigate to "System Localization" --> "Languages" and create a new record.


Use as "id" the two-character language code. Here the full list: [https://www.loc.gov/standards/iso639-2/php/code_list.php][lang-code-list]

<img src="/assets/enable-unsupported-languages-02.png" alt="" width="55%"/>


Then create a new choice with the followings value:

Table --> <em>sys_user</em>

Element --> <em>preferred_language</em>

Language --> <em>en</em>

Label --> <em>Name of the language set in the previous step</em>

Value --> <em>two-character language code</em>

<img src="/assets/enable-unsupported-languages-01.png" alt="" width="25%"/>


In this way, the new language can be chosen at login or set for the individual user.


<img src="/assets/enable-unsupported-languages-03.png" alt="" width="25%"/>


Users will now be able to choose the new language, but it is necessary to enter the translations in the new language. If there are no translations, they will be shown in English.

<br>
The texts to be translated are listed in these tables:

Translated Name / Field --> <em>sys_translated</em>

Message --> <em>sys_ui_message</em>

Field label --> <em>sys_documentation</em>


Choice --> <em>sys_choice</em>

Translated Text --> <em>sys_translated_text</em>

<br>
It is advisable to export the texts from these tables so that professional translators can work on the translations.

Once the translation is complete, it can be reinserted into the system via import set.



[i18n-plugin-language]: https://docs.servicenow.com/bundle/rome-platform-administration/page/administer/localization/task/t_ActivateALanguage.html
[lang-code-list]: https://www.loc.gov/standards/iso639-2/php/code_list.php