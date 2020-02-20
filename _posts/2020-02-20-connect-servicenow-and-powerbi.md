---
layout: post
title:  "Connect ServiceNow and Power BI"
date:   2020-02-20 18:58:02 +0100
permalink: connect-servicenow-and-powerbi

---
It is possible to connect ServiceNow and Power BI in order to have a different kind of reporting from the one available on Performance Analytics or on the report module.

It can be done in several ways: the easiest one is using rest api and making a get call.
With rest api, it is also possible to access the data of tables or database view

TIPS: While building the soon-to-be-made rest call you can use [Rest API Explorer][rest-api-explorer]  to apply filters on data extraction, to show fields via dotwalking and much more.

![servicenow rest api explorer](/assets/connect-servicenow-and-powerbi-00.png)

Once you get the get call, you need to install [Power BI Desktop][powerbi-desktop] on your PC.
Once started, in order to read the data you need to press "Get Data" -> "From Web".

![powerbi web source](/assets/connect-servicenow-and-powerbi-01.png)

Then you have to enter the call you previously created.
Local ServiceNow credentials can be used as credentials.

Now the data will be uploaded and, if you want,  you can manipulate it or you can complete the upload directly by pressing "Close & Apply".

That's it! Now you can start building dashboards as you wish.


[rest-api-explorer]: https://docs.servicenow.com/bundle/istanbul-application-development/page/integrate/inbound-rest/task/t_GetStartedAccessExplorer.html?cshalt=yes

[powerbi-desktop]: https://www.microsoft.com/en-us/p/power-bi-desktop/9ntxr16hnw1t?activetab=pivot:overviewtab