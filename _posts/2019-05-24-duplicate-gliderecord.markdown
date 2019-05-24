---
layout: post
title:  "Duplicate GlideRecord"
date:   2019-05-24 19:01:00 +0100
permalink: duplicate-gliderecord
---

A simple way for creating a new gliderecord from another one.

Please, look at the number 5 if you use the [autonumbering feature][sn-autonumbering]

{% highlight javascript %}
function duplicateGlideRecord(tableName, sysID, fieldsToExclude) {
    var gr = new GlideRecord(tableName);
    gr.initialize();
    gr.get(sysID);
    gr.setValue('number', new NumberManager(tableName).getNextObjNumberPadded());
    if (typeof fieldsToExclude !== 'undefined')
        for (var i = 0; i < fieldsToExclude.length; i++)
            gr.setValue(fieldsToExclude[i], "NULL");
    return gr;
}
{% endhighlight %}

Here some examples:
The first with only two parameters (tablename and sys_id of source gliderecord)
{% highlight javascript %}
@example
var table = 'incident';
var sysID = 'ed92e8d173d023002728660c4cf6a7bc';
var clonedGR = duplicateGlideRecord(table, sysID);
clonedGR.insert();
{% endhighlight %}


The second add an array of fields that need to be excluded from duplication
{% highlight javascript %}
var table = 'incident';
var sysID = 'ed92e8d173d023002728660c4cf6a7bc';
var fieldsToExclude = ['subcategory', 'contact_type'];
var clonedGR = duplicateGlideRecord(table, sysID, fieldsToExclude);
clonedGR.insert();
{% endhighlight %}

[sn-autonumbering]: https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/field-administration/task/t_AutoNumberingRecordsInATable.html