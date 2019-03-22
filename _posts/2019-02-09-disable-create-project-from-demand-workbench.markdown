---
layout: post
title:  "Disable Create Project from Demand Workbench"
date:   2019-02-09 15:24:20 +0100
---
Officially there isn’t a way to customize Demand Workbench; this can be an issue if you need to stop the creation of artifacts from the context menù (like project or enhancement).

![Demand workbench screen create project context menù](/assets/dmn_workbench_create_project.png)

Anyway there’s a way to block this feature.

When the button “Create Project” is pressed, Demand Workbench invokes the processor “workbenchAjaxProcessor”.

In the script of the processor, the variable g_request contains referer, so we can check if the processor is invoked from Demand Workbench

{% highlight javascript %}
var refererURL = g_request.getHeader('referer');
{% endhighlight %}

And check on the referURL to find Demand Workbench

{% highlight javascript %}
var inDemandWorkbench = refererURL.includes('$demand_workbench.do');
{% endhighlight %}

And use it to stop the creation

It’s possible to show an error message through the variable “response”.

{% highlight javascript %}
if (method == 'createProject' && inDemandWorkbench) {
        response = {error: 'Cannot create project from demand workbench'};
        response = (new JSON()).encode(response);
}
{% endhighlight %}