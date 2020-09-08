---
layout: post
title:  "Connect ServiceNow and Power BI"
date:   2020-07-23 17:04:05 +0100
category: ["ServiceNow", "Power BI"]
permalink: connect-servicenow-and-powerbi

---
This guide is about how to connect ServiceNow and PowerBI via REST API, in order to build reports and publish them on PowerBI Service.

This way, we will be able to share ServiceNow information with our colleagues on Microsoft Teams, access from mobile and use all the opportunities PowerBI provides us.

## Building the REST call

First, you need to build the REST call that will be made by PowerBI to get the data in ServiceNow.

To do this, we can use the [Rest API Explorer][rest-api-explorer] tool to help us, which is already included in ServiceNow.


Furthermore, our user must have the "rest_api_explorer" role. If he does not, you can still design the call to an administrator user or with the correct role.

Then, we must select the ServiceNow table containing the data we are interested in (for example the incident table) and press "Sent".

<img src="/assets/connect-servicenow-and-powerbi-00.png" alt="servicenow rest api explorer" width="75%"/>

After loading, ServiceNow will show us the call (highlighted in blue in the screenshot) to provide to PowerBI

<img src="/assets/connect-servicenow-and-powerbi-01.png" alt="servicenow rest call" width="75%"/>

Here is a preview of the answer

<img src="/assets/connect-servicenow-and-powerbi-02.png" alt="servicenow rest response" width="35%"/>


*In-depth information*:

At this step, you can apply filters, choose the fields to be shown in the body and many other options.

In particular, by choosing the fields you can use dot-walking to get information.

For example, in case you want to get the incident table records having only "number", "opened_by" and "short_description" fields this will be the call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_fields=number%2Copened_by%2Cshort_description
{% endhighlight %}


And this the result:

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


The "opened_by" field is a reference to the users table, therefore to display the value shown to users you need to add the "sysparm_display_value" parameter to the call by setting it to true

This will be the call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_display_value=true&sysparm_fields=number%2Copened_by%2Cshort_description
{% endhighlight %}

And the following result:

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


To avoid "opened_by" to be seen in PowerBI as a record, we can use the parameter "sysparm_exclude_reference_link", by setting it to true, to make ServiceNow directly return the display value.
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

You can also use dot walking to get information from reference fields, as shown with "opened_by".

In case we want to get the email address of the user who opened the case, it will be enough to edit the part of the call where we select the fields to show.


It’s just going to have to be edited from this:

*sysparm_fields=number,opened_by,short_description*
To this:

*sysparm_fields=number,opened_by,**opened_by.email**,short_description*


And once made the call:

{% highlight http %}
https://YOUR-SERVICENOW-INSTANCE.service-now.com/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&sysparm_fields=number%2Copened_by%2Copened_by.email%2Cshort_description 
{% endhighlight %}


This will be the response:

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


## Connection to PowerBI

Once the call is ready, we can switch to PowerBI Desktop to enter the data source and create the report.

First you will need to create a new Data Source, Web type

<img src="/assets/connect-servicenow-and-powerbi-03.png" alt="powerbi data source web" width="50%"/>

Then enter the previous call created in ServiceNow

<img src="/assets/connect-servicenow-and-powerbi-04.png" alt="powerbi data source web url" width="75%"/>

Important: remove the final parameter (&sysparm_limit=1) to avoid having a maximum limit of results

The first connection will require some credentials: you can use your local ServiceNow credentials

<img src="/assets/connect-servicenow-and-powerbi-05.png" alt="powerbi data source basic auth" width="75%"/>


The resulting data will respect the user's security (ACL and any business rule before query), so be careful and, if necessary, create a dedicated reporting user.

In order to make the data usable you need to:

- Convert the data into tables

<img src="/assets/connect-servicenow-and-powerbi-06.png" alt="powerbi convert data into table" width="75%"/>


- Expand the rows

<img src="/assets/connect-servicenow-and-powerbi-07.png" alt="powerbi convert expand the rows" width="75%"/>

- Expand the columns

<img src="/assets/connect-servicenow-and-powerbi-08.png" alt="powerbi convert expand the columns" width="75%"/>


- Press “Close and Apply”

<img src="/assets/connect-servicenow-and-powerbi-09.png" alt="powerbi convert close apply" width="75%"/>



Now PowerBI Desktop will have taken us back to the work screen and, on the right, the table and fields we imported from ServiceNow will appear

<img src="/assets/connect-servicenow-and-powerbi-10.png" alt="powerbi field" width="75%"/>

Now we can create the report, like the one here attached:

[ServiceNow ITSM Incident Dashboard PowerBI](/assets/servicenow_itsm_incident_dashboard_power_bi.pbit)

Once finished you can share it on PowerBI Service through the path "File" "Publish" "Publish to PowerBI".
If shared on "My Workspace", it will be accessible via web and mobile.
This way, you can share it with other colleagues and also via Microsoft Teams.


[rest-api-explorer]: https://community.servicenow.com/community?id=community_blog&sys_id=396ef5e71bebbf84fff162c4bd4bcb8e