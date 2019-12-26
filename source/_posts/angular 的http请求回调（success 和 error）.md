---
title: angular 的http请求回调（success 和 error）
date: 2017-11-27 16:59:13
categories: Angular
tags: Angular
keywords: http, angular

---
前面有文章写过angular的$http请求的简单书写和使用。
回顾一下：

```
var Server = angular.module("Server", []);

Server.service("Api", ["$http", function ($http) {
		return {
			getTest : function (params, callback) {
		            $http({
		                url: 'www.baidu.com?name=xxx&passwd=xxx',
		                method: 'GET'
		            }).success(callback);
		        },
		        postTest : function (data, callback) {
		            $http({
		                url: 'www.baidu.com',
		                data: data,
		                method: 'POST'
		            }).success(callback);
		}
}]);

var App = angular.module("App", [ "Server"]);
App.controller('Ctrl', ['$scope', 'Api',
    function($scope, Api) {
    		Api.getTest("?name=xxx&passwd=xxx", function(res){
    			//res为返回值	
				alert("请求成功！")
			});
			Api.postTest({name: xxx, passwd: xxx}, function(res){
    			//res为返回值	
				alert("请求成功！")
			});
    }]);
```
<!-- more -->
这里以GET和POST请求为例。首先我们需要在APP中注入Server这个模块，才能使用里面的service服务Api，如上代码所示，注入Server之后我们就可以在控制器Ctl里面注入Api服务，然后使用我们之前定义好的接口。

 1. GET请求中params 代表了？之后的参数，即 params == '?name=xxx&passwd=xxx',这样就可以通过传参的方式把参数加入到地址上，callback是请求成功后的回调，是个function，我们可以在里面打印出请求成功的返回值。
2. POST请求中data代表请求参数，传入之后直接请求即可，其他跟get请求类似。

## error

还有一点需要说的是http请求有时候会失败，当我们http请求失败的时候，将不会再调用success回调函数，而是会进入error回调，下面我就写一下http请求的success和error同时存在的写法。

```

var Server = angular.module("Server", []);

Server.service("Api", ["$http", function ($http) {
		return {
				getTest : function (params, successCallback, errorCallback) {
		            $http({
		                url: 'www.baidu.com?name=xxx&passwd=xxx',
		                method: 'GET'
		            }).then(successCallback, errorCallback);
		        }
}]);

var App = angular.module("App", [ "Server"]);
App.controller('Ctrl', ['$scope', 'Api',
    function($scope, Api) {
    		Api.getTest("?name=xxx&passwd=xxx", function(res){
    			//res为返回值	
				alert("请求成功！")
			}, function(res){
    			//res为返回值	
				alert("请求失败！")
			});
    }]);
```
用法几乎一样，只不过把success(callback)改成了then(callback1, callback2).然后在调用的时候再加入一个function即可。

最简单的get写法

```
$http({
          url: 'www.baidu.com?name=xxx&passwd=xxx',
          method: 'GET'
      }).success(callback);
```
