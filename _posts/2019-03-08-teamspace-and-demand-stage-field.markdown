---
layout: post
title:  "Teamspace and demand, “Stage” field"
date:   2019-03-08 21:05:20 +0100
permalink: teamspace-demand-stage-field
---

We all agree that the field “Stage” on Idea/Demand and Project is appreciated by users, but if we use the [Teamspace feature][teamspace-feature] plugin it’s possible to have some trouble.

In case I create a new state for tsp1_demand, we’ll see only the default image and the table name showed on the stage field will be “Demand”, instead of “Teamspace1 Demand”.

Bad.

![tsp problem stage](/assets/tsp_dmn_stage_00.png)

The object where you can configure the image is “Demand Stage Config”; even if we add correctly a new record for the new state, nothing will change.

![tsp problem stage part 2](/assets/tsp_dmn_stage_01.png)

The problem is in the script include “DemandStageChoices” that simply… ignores the teamspace feature, both of the image and the table name.

To add the teamspace1 demand(tsp1_demand) feature is essential to change the method “getDemandChoice”

{% highlight javascript %}
getDemandChoice: function (demand) {
    var dmnTable = demand.getTableName();
    var table_label = GlideTableDescriptor.get(dmnTable).getLabel();
    if (typeof demand == "undefined" || !demand) {
        return this._createChoice("", table_label, this.defaultImage);
    }
    var nameStageConfigRecord = 'demand';
    if (dmnTable != 'dmn_demand')
        nameStageConfigRecord = dmnTable;
    var flow = this._getStageConfig(nameStageConfigRecord, demand.state);
    var label = demand.getDisplayValue("state");
    var image = flow.image || this.defaultImage;
    return this._createChoice(table_label, label, image);
},
{% endhighlight %}

By adding these lines, the script include will look for the teamspace1 records and everything will work properly.

It’s possible to choose as image any image displayed here: https://YOUR_INSTANCE.service-now.com/image_picker.do



[teamspace-feature]: https://docs.servicenow.com/bundle/kingston-it-business-management/page/product/project-management/reference/r_TeamspaceFeatures.html?cshalt=yes
