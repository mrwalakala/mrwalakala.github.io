---
layout: post
title:  "Connect ServiceNow and Power BI"
date:   2020-07-23 17:04:05 +0100
category: ["ServiceNow", "Power BI"]
permalink: connect-servicenow-and-powerbi

---
In this guide we will see how to connect ServiceNow and Power BI via REST API to build reports and publish them on Power BI Service.

In this way we will be able to share ServiceNow information with our colleagues on Microsoft Teams, access from mobile and use all the opportunities Power BI provides us.

## Build the REST call

Firstly, you need to build the REST call that will be made by Power BI to get the data in ServiceNow.

To do this we can use the [Rest API Explorer][rest-api-explorer] tool, present inside ServiceNow to help us.


It is necessary that our user has the "rest_api_explorer" role. If he does not have it, you can still design the call to an administrator user or with the correct role.

From here we must select the ServiceNow table of which we are interested in the data (for example the incident table) and press "Sent".

<img src="/assets/connect-servicenow-and-powerbi-00.png" alt="servicenow rest api explorer" width="75%"/>

After the loading ServiceNow will show us the call (highlighted in blue in the screenshot) to provide to Power BI

<img src="/assets/connect-servicenow-and-powerbi-01.png" alt="servicenow rest call" width="75%"/>

And a preview of the answer

<img src="/assets/connect-servicenow-and-powerbi-02.png" alt="servicenow rest response" width="75%"/>


*In-depth information*:

At this step you can apply filters, choose the fields that will be shown in the body and many other options.

In particular, by choosing the fields you can use dot-walking to get information.

For example, in case you want to get the incident table records but only have the number, opened_by and short_description fields this will be the call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_fields=number%2Copened_by%2Cshort_description
{% endhighlight %}


With this result:

{% highlight json %}
{
  "result": [
    {
      "number": "INC0000060",
      "short_description": "Unable to connect to email",
      "opened_by": {
        "link": "https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/sys_user/681ccaf9c0a8016400b98a06818d57c7",
        "value": "681ccaf9c0a8016400b98a06818d57c7"
      }
    }
  ]
}
{% endhighlight %}


The "opened_by" field is a reference to the users table, so to show the value shown to users you need to add the "sysparm_display_value" parameter to the call by setting it to true

This will be the call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_display_value=true&sysparm_fields=number%2Copened_by%2Cshort_description
{% endhighlight %}

With the following result:

{% highlight json %}
{
  "result": [
    {
      "number": "INC0000060",
      "short_description": "Unable to connect to email",
      "opened_by": {
        "display_value": "Joe Employee",
        "link": "https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/sys_user/681ccaf9c0a8016400b98a06818d57c7"
      }
    }
  ]
}
{% endhighlight %}


To avoid that opened_by is then seen in Power BI as a record we can use the parameter sysparm_exclude_reference_link, setting it to true, to make ServiceNow directly return the display value.
Call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&sysparm_fields=number%2Copened_by%2Cshort_description 
{% endhighlight %}


Result:
{% highlight json %}
{
  "result": [
    {
      "number": "INC0000060",
      "short_description": "Unable to connect to email",
      "opened_by": "Joe Employee"
    }
  ]
}
{% endhighlight %}

You can also use dot walking to get information from reference fields, such as opened_by.

In case we want to get the email address of the user who opened the case it will be enough to modify the part of the call where we select the fields to show.


It's just gonna have to be modified from this.

*sysparm_fields=number,opened_by,short_description*
To this:

*sysparm_fields=number,opened_by,**opened_by.email**,short_description*


And made the call

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&sysparm_fields=number%2Copened_by%2Copened_by.email%2Cshort_description 
{% endhighlight %}


Response:

{% highlight json %}
{
  "result": [
    {
      "number": "INC0000060",
      "short_description": "Unable to connect to email",
      "opened_by.email": "employee@example.com",
      "opened_by": "Joe Employee"
    }
  ]
}
{% endhighlight %}


## Connection to Power BI

Once the call is ready, we can switch to Power BI Desktop to enter the data source and create the report.

First you will need to create a new Data Source, Web type

<img src="/assets/connect-servicenow-and-powerbi-03.png" alt="power bi data source web" width="50%"/>

Then enter the previous call created in ServiceNow

<img src="/assets/connect-servicenow-and-powerbi-04.png" alt="power bi data source web url" width="75%"/>

Important: remove the final parameter (&sysparm_limit=1) to avoid a maximum limit of results

At the first connection you will need to specify credentials, you can use your local ServiceNow credentials

<img src="/assets/connect-servicenow-and-powerbi-05.png" alt="power bi data source basic auth" width="75%"/>


The data that will be shown will respect the user's security (ACL and any business rule before query), so be careful and, if necessary, create a dedicated reporting user.

To make the data usable you need to:

- Convert data into tables

<img src="/assets/connect-servicenow-and-powerbi-06.png" alt="power bi convert data into table" width="75%"/>


- Expand the rows

<img src="/assets/connect-servicenow-and-powerbi-07.png" alt="power bi convert expand the rows" width="75%"/>

- Expand the columns

<img src="/assets/connect-servicenow-and-powerbi-08.png" alt="power bi convert expand the columns" width="75%"/>


- Press “Close and Apply”

<img src="/assets/connect-servicenow-and-powerbi-09.png" alt="power bi convert close apply" width="75%"/>



Now Power BI Desktop will take us back to the work screen and, on the right, will appear table and fields imported from ServiceNow

<img src="/assets/connect-servicenow-and-powerbi-10.png" alt="power bi field" width="75%"/>

Now we can create the report, like this one that I attach:

[ServiceNow ITSM Incident Dashboard Power BI](/assets/servicenow_itsm_incident_dashboard_power_bi.pbit)

Once finished you can share it on PowerBI Service through the path "File" "Publish" "Publish to Power BI".
Sharing it on "My Workspace" will be accessible via web and mobile.
So, you can share it with other colleagues and via Microsoft Teams.


[rest-api-explorer]: https://community.servicenow.com/community?id=community_blog&sys_id=396ef5e71bebbf84fff162c4bd4bcb8e