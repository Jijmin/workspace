## 项目搭建
### 下载好myApp文件
### 欢迎页面
1. index.html主页面
2. app.js是我们入口函数
3. 在我们的app.js中要注入config.js和global.js以及我们的主路由route.js
4. 主页面一样要引入我们的js文件
5. 主路由中注入我们各个分路由，并且对我们的路由规则进行设置
```
var app=angular.module('starter.route',['guidePage.route']);
app.config(function($urlRouterProvider) {
  $urlRouterProvider.otherwise('/guidePage');
});
```
6.  每个分路由管理控制器和模板页
```
var app=angular.module('guidePage.route',[
    'guidePage.controller'
  ]);
  app.config(function ($stateProvider) {
    $stateProvider
      .state('guidePage',{
        url:'/guidePage',
        templateUrl:'modules/guidePage/template.html',
        controller:'guidePage.ctrl'
      })
  })
```
7.  控制器控制我们的功能
```
var app=angular.module('guidePage.controller',[
]);
app.controller('guidePage.ctrl',function ($scope) {
})
```
8. 在服务中我们可以对数据进行操作，讲数据和控制器分离，方便管理