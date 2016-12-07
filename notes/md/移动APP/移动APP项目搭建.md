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
8. 在服务中我们可以对数据进行操作，将数据和控制器分离，方便管理
9. 书写好我们的欢迎页模板
10. 在主页引入我们的Swiper.js插件
11. 还要对插件进行初始化才能使用
```
app.controller('guidePage.ctrl',function ($scope) {
  var mySwiper = new Swiper('.swiper-container', {
    pagination:'.swiper-pagination'
  });
})
```
12. 当页面加载完毕后完成对下一页的动画触发
```
onSlideChangeEnd:function(swiper){
  var index=swiper.activeIndex+1;
  var slide=document.getElementById('tips-'+index);
  angular.element(slide).removeClass('hidden').addClass('guide-show');
}
```

### 主页
