---
layout: post
title:  "How to paginate element on Service Portal in async way"
date:   2020-06-10 20:00:55 +0100
category: ["ServiceNow", "Service Portal"]
permalink: paginate-element-serviceportal-async

---
I just had to paginate a tr element asynchronously, reading data from an external webservice.

The [Pagination Directive component] [pagination-directive] really saved me a lot of time.


This is a simple example of implementation on a widget that shows users on ServiceNow in groups of 10 asynchronously.

At the bottom of the page you will find the update set ready to download.


Firstly html part:

{% highlight html %}
<div>
  <table border="1">
    <tr>
    <th>Username</th>
    <th>Email</th>
  </tr>
   <tr dir-paginate="user in users | itemsPerPage: usersPerPage" total-items="totalUsers" current-page="pagination.current" pagination-id="tr-users">
        <td>
          {{user.user_name}}
        </td>
     	<td> {{user.email}}
        </td> 
    </tr>
    </table>
    <dir-pagination-controls pagination-id="tr-users" on-page-change="pageChanged(newPageNumber)"></dir-pagination-controls>
</div>
{% endhighlight %}

you need to add the lines 7 and 15 to implement the pagination.


Then client side scripting:

{% highlight javascript %}
api.controller = function ($scope, $http) {
    /* widget controller */
    var c = this;
    $scope.users = [];
    $http.get('api/now/stats/sys_user?sysparm_count=true').then(function (responseUserCount) {
        if (responseUserCount.status == 200) {
            $scope.totalUsers = responseUserCount.data.result.stats.count;
        }
    });
    $scope.usersPerPage = 10;
    getResultsPage(1);

    $scope.pagination = {
        current: 1
    };

    $scope.pageChanged = function (newPage) {
        getResultsPage(newPage);
    };

    function getResultsPage(pageNumber) {
        var offset = ($scope.usersPerPage * pageNumber) - $scope.usersPerPage;
        $http.get('api/now/table/sys_user?sysparm_limit=' + $scope.usersPerPage + '&sysparm_offset=' + offset).then(function (responseUsers) {
            if (responseUsers.status == 200) {
                $scope.users = responseUsers.data.result;
            }
        });
    }
};
{% endhighlight %}


Here I use two apis:
To get the total number of records I use [Aggregate API] [aggregate-api], while to get the results i use TableAPI, with limit and offset parameters.

The work on the widget is done, to complete it you have to add the Pagination Directive component as Widget Dependency


To save time you can download the entire update set with the widget ready. The widget has id "ngrepeat-pagination_async" and the dependency.

[Update Set](/assets/update_set_widget_async_pagination.xml)


[aggregate-api]: https://developer.servicenow.com/dev.do#!/reference/api/orlando/rest/c_AggregateAPI

[pagination-directive]: https://github.com/michaelbromley/angularUtils/tree/master/src/directives/pagination 