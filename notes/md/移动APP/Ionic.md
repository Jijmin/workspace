## 项目搭建
### 下载好myApp文件
### 基本搭建
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
(function (angular){
  var app=angular.module('guidePage.controller',[
    'guidePage.service'
  ]);
  app.controller('guidePage.ctrl', ['$scope','guidePageService',function($scope,guidePageService){
    $scope.name='template';
    $scope.guidePageName=guidePageService.name;
  }]);
})(angular);
```
8. 在服务中我们可以对数据进行操作，讲数据和控制器分离，方便管理
```
(function (angular){
  var app=angular.module('guidePage.service',[]);
  app.factory('guidePageService',function(){
    return {
      name:'guidePageName'
    }
  });
})(angular);
```

### 官方popover的bug
1. 使用popover时不会显示出来的问题
2. 官方在里面加入了一个透明度，显示不出来
3. 将css样式中的透明度去掉就可以

### 欢迎页面
1. guidePage基本结构的搭建
2. route.js、controller.js、service.js、template.html
3. 在主页加入我们的CSS样式
4. 在模板页将欢迎页面的基本结构搭建好
5. 欢迎页里面有一个Swiper结构
6. 控制器中新创建一个Swiper对象
```
var mySwiper=new Swiper('.swiper-container',{
  //...
});
```
7. 添加分页器
```
pagination:'.swiper-pagination'
```
8. 在滑动的过程中第二页没有效果
9. 应该设置当页面加载完成后播放的动画
```
onSlideChangeEnd:function(swiper){
  //当页面加载完成之后播放动画
  var index=swiper.activeIndex+1;
  var slide=document.getElementById('tips-'+index);
  //转换成jQuery对象
  angular.element(slide).removeClass('hidden').addClass('guide-show');
}
```
10. 欢迎页面跳转home页面
11. 找到我们的跳转的按钮，加入点击事件
12. 在控制器中加入一个$state服务，调用go方法进行跳转
```
$scope.goHomePage=function(){
  $state.go('home');
}
```
13. 给localStore添加一个参数，如果参数存在表示guidePage已经浏览过，那么就直接跳转到home页面
```
$scope.goHomePage=function(){
  //给localStore添加一个参数，如果参数存在表示guidePage已经浏览过，那么就直接跳转到home页面
  localStorage.setItem('homeState','true');
  $state.go('home');
}
```
14. 设置路由规则的时候进行一个判断
```
app.config(function($urlRouterProvider) {
  //判断当前页面中的localStorage中是否存在homeState，如果没有值跳转到欢迎页，如果有值跳转到home中
  if(localStorage.getItem('homeState')=='true'){
    $urlRouterProvider.otherwise('/home');
  }else{
    $urlRouterProvider.otherwise('/guidePage');
  }
});
```

### 混合APP存储数据
1. 存在手机文件中xml
2. 发送Ajax向服务器请求数据
3. 存储在localStorage中

### 控制器传递参数
1. `$scope.$emit('接收方的名字,{msg:'发送内容'})`向父级发送信息
2. `$scope.$on('接收方的名字',function(data){})`接收信息
3. `$scope.$broadcast('接收方的名字,{msg:'发送内容'})`父亲将消息广播给儿子

### home
1. 创建页面结构route.js、controller.js、service.js、template.html
2. 主页面有一个tab切换效果，使用我们的抽象路由书写

### tab页面
1. tab选项卡做成抽象路由模式
2. 模板页是tab切换形式
```
<ion-tabs class="tabs-icon-top tabs-color-active-positive">
  <ion-tab title="首页" icon-off="ion-ios-home-outline" icon-on="ion-ios-home" href="#/tab/home">
    <ion-nav-view name="tab-home"></ion-nav-view>
  </ion-tab>
  <ion-tab title="分类" icon-off="ion-ios-paper-outline" icon-on="ion-ios-paper" href="#/tab/category">
    <ion-nav-view name="tab-category">
    </ion-nav-view>
  </ion-tab>
  <ion-tab title="购物车" icon-off="ion-ios-cart-outline" icon-on="ion-ios-cart" href="#/cart" badge="obj_cartCount.count" badge-style="badge-assertive">
    <ion-nav-view name="tab-cart"></ion-nav-view>
  </ion-tab>
  <ion-tab title="我" icon-off="ion-ios-gear-outline" icon-on="ion-ios-gear" href="#/tab/account">
    <ion-nav-view name="tab-account"></ion-nav-view>
  </ion-tab>
</ion-tabs>
```
3. 抽象主路由
```
(function(angular){
  //创建tab的抽象路由模块
  var app=angular.module('tab.route',[]);
  app.config(function($stateProvider){
    $stateProvider.state('tab',{
      url:'tab',
      abstract:true,//表示将当前路由作为抽象路由
      templateUrl:'modules/tab/template.html'
    });
  });
})(angular);
```
4. 抽象子路由，主页是一个抽象子路由
```
app.config(function($stateProvider){
$stateProvider.state('tab.home',{
  url:'/tab/home',
  templateUrl:'modules/home/template.html',
  controller:'home.ctrl'
});
```
5. 匹配不同的view
```
app.config(function($stateProvider){
  $stateProvider.state('tab.home',{
    url:'/tab/home',
    views:{
      'tab-home':{
        templateUrl:'modules/home/template.html',
        controller:'home.ctrl'
      }
    },
  });
});
```
6. 将抽象主路由注入到主路由中
7. 主页对抽象路由进行引用