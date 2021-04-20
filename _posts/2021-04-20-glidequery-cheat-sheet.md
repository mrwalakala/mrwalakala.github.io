---
layout: post
title:  "GlideQuery Cheat Sheet"
date:   2021-04-20 11:22:29 +0100
category: ["ServiceNow", "GlideQuery"]
permalink: glidequery-cheat-sheet

---

GlideQuery from Quebec is now installed by default on every ServiceNow instance.  
This is a short cheat sheet to make the best use of it. All examples use the tables of the ITBM module.

These are the main sources of information used:


[Jace's post](https://jace.pro/post/2020-05-24-glide-freaking-query-wow)

[Overview of GlideQuery by Samuel Meylan](https://www.snow-adventures.com/blog/overview-of-glidequery/) and his [cheat sheet](https://www.snow-adventures.com/blog/glidequery-cheat-sheet/)

[Official ServiceNow Docs](https://docs.servicenow.com/bundle/paris-application-development/page/app-store/dev_portal/API_reference/GlideQuery/concept/GlideQueryGlobalAPI.html) 

[ServiceNow Developer blog post made by Peter Bell, creator of GlideQuery](https://developer.servicenow.com/blog.do?p=/authors/peter-bell/)

<br/>

Read records
{% highlight javascript %}
var gqProjects = new GlideQuery('pm_project')
.where('project_manager.user_name', 'paul.martin')
.select('short_description', 'number', 'goals$DISPLAY','cost$CURRENCY_DISPLAY')
.forEach(function (project) {
    gs.info('Project ' + project.short_description + '; number ' + project.number + '; Planned cost ' + project.cost$CURRENCY_DISPLAY + '; goals: ' + project.goals$DISPLAY);
});
{% endhighlight %}


Read only one record
{% highlight javascript %}
var gqSingleProject = new GlideQuery('pm_project')
.where('number', 'PRJ0010383')
.selectOne('short_description', 'number', 'goals$DISPLAY', 'cost$CURRENCY_DISPLAY')
.ifPresent(function (project) {
    gs.info('Project ' + project.short_description + '; number ' + project.number + '; Planned cost ' + project.cost$CURRENCY_DISPLAY + '; goals: ' + project.goals$DISPLAY);
});
{% endhighlight %}

Read records with complex where and or conditions
{% highlight javascript %}
var gqActualsByPortfolios = new GlideQuery('pm_project')
.where('active', true)
.where('phase', '!=', 'closing')
.whereNotNull('primary_portfolio')
.where(new GlideQuery()
    .where('duration', '>', '100 00:00:00')
    .orWhere('department.name', 'Engineering')
)
.groupBy('primary_portfolio.name')
.aggregate('sum', 'work_cost')
.having('sum', 'work_cost', '>', 100)
.select()
.forEach(function (actualsByPortfolio) {
    gs.info(JSON.stringify(actualsByPortfolio));
});
{% endhighlight %}


Check if record exist
{% highlight javascript %}
var gqProjectExists = new GlideQuery('pm_project')
.where('number', 'PRJ0010383')
.selectOne()
.isPresent();
gs.info('Project exist? ' + gqProjectExists);
{% endhighlight %}


Create record and return sys_id
{% highlight javascript %}
var projectObject = {
    short_description: 'New Project',
    description: 'Please help!',
};

var gqCreateProject = new GlideQuery('pm_project')
.insert(projectObject)
.get();
gs.info(JSON.stringify(gqCreateProject));
{% endhighlight %}


Update multiple
{% highlight javascript %}
var gqUpdateProjects = new GlideQuery('pm_project')
.where('department.name', 'Facilities')
.updateMultiple({
    priority: 2
});
gs.info(JSON.stringify(gqUpdateProjects));
{% endhighlight %}


Delete
{% highlight javascript %}
var gqDeleteProjects = new GlideQuery('pm_project')
.where('department.name', 'Facilities')
.deleteMultiple();
{% endhighlight %}

Example of filter and result manipulation
{% highlight javascript %}
var gqProjects = new GlideQuery('pm_project')
    .whereNotNull('goals')
    .select('short_description', 'number', 'goals', 'goals$DISPLAY', 'percent_complete')
    .filter(function (project) {
        return project.goals.indexOf(",") != -1;
    })
    .map(function (project) {
        var roundedPercent = Math.round(project.percent_complete)
        var nameUpperCase = project.short_description.toUpperCase();
        return nameUpperCase + '. Percent complete ' + roundedPercent + '%.' + project.goals$DISPLAY;
    })
    .forEach(function (project) {
        gs.info(project);
    });
{% endhighlight %}

Aggregation
{% highlight javascript %}
var projectsEconomics = new GlideQuery('pm_project')
    .aggregate('count')
    .aggregate('max', 'cost')
    .aggregate('sum', 'cost')
    .aggregate('max', 'budget_cost')
    .aggregate('sum', 'budget_cost')
    .groupBy('primary_portfolio.name')
    .groupBy('primary_program.short_description')
    .select()
    .forEach(function (projectEconomics) {
        gs.debug(JSON.stringify(projectEconomics));
    });
{% endhighlight %}