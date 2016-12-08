### ngRoute
- ng-view       //ui-view
- $stateProvider.state      //$routeProvider.when()
- $urlRouterProvider.otherwise      //$routeProvider.otherwise

### 重新整理结构
- 为了让结构更加清晰，方便后期维护
- 在我们的启动模块中将对路由规则的设置放置到一个大的路由中去

### 主模块文件抽取(核心模块)
1. 控制项目的启动文件app.js(项目的入口文件)
2. 控制路由的跳转route.js(项目的总路由)
3. 控制全局变量global.js(一些系统的参数，网址，版本)
4. 控制不同平台的兼容性的config.js
5. route，global，config三个模块都需要在app模块中引用

### 其他功能模块划分
1. controller.js//业务逻辑
2. service.js//数据请求访问，和服务器进行操作
3. router.js//子功能路由js，控制我们的按页面跳转
4. template.html//模板

### 改APP名字
config.xml中更改name
