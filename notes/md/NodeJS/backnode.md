### 文件目录
- 配置文件【config】
    + db.js
- 控制器【controllers】
    + index.js
- 模型【models】
    + teacher.js
- 包管理【node_modules】
- 静态资源【public】
    + assets
        * bootstrap
        * echarts
        * font-awesome
        * jquery
        * nprogress
    + images
    + js
    + less
    + libs
- 上传图片【uploads】
- 视图【views】
    + adverts
    + courses
    + dashboard 
        * index.xtpl
    + layout
        * base.xtpl
    + login
    + teachers
    + users
- 入口文件【app.js】
- 【package.json】

### 项目搭建
1. 将依赖关系记录好`npm init`
2. 将Express框架存储起来`npm install express --save`
3. 将express实例化搭建一个服务器，并监听端口
4. 控制器中要引入一个路由模块，设置测试路由
5. 将路由暴露给入口文件使用
6. 设置模板引擎
7. 基础的目录文件搭建好
8. 将静态资源挪过来(images、js、less)
9. 上传文件的目录设置为upload
10. 将设置好的页面放置到我们模板的views下，要将后缀改变成模板的格式
11. 对页面进行分模块(users、dashboard、teachers、adverts、courses)
12. 根目录下加载一个主页的视图，但是样式没有加载好
13. 设置静态路由，将静态资源挂载到我们的根目录下
14. 将公共部分抽离出来，做一个模板布局
15. 在views目录下新建一个layout文件，base.xtpl
16. 将公共模板继承过来`{{extend ('../layout/base')}}`
17. 公共部分做一个标记，让继承过来后知道是在那个地方进行新增`{{{block ('content')}}}`
18. 公共部分对应的接口`{{#block ('content')}}...{{/block}}`
19. 在公共部分的【个人中心】加上路由
20. 当我们的功能强大的时候，将路由都挂载在我们主路由上是不合适的
21. 我们应该对我们的模块进行路由的分化
22. 在我们的今天资源中有【assats】放置我们所要用的一些静态资源的包
23. 将导航栏的连接设置好路由规范
24. 添加二级菜单展开JS功能
25. 取到a元素，进行判断，如果属性不是javascript:;就直接返回
26. 然后对其进行一个切换功能
27. 根据url地址栏的参数设置选中状态`location.pathname`
28. 将用户模块的路由挂载到主路由下，然后对页面进行渲染
29. 通过地址栏的参数设置导航栏对应的class值
30. 用seajs改造我们的项目，在我们的public目录中加一个libs文件夹
31. 先将模块加载器拿进来，我们需要将业务逻辑放在scripts
32. 中间要引入jquery，我们希望路径不是那么的长
33. 可以使用seajs提供的config函数进行设置
```
seajs.config({
    base:'/assets',
    alias:{
        jquery:'jquery/jquery'
    }
});
```
34. css命名空间(避免样式冲突)
35. 对讲师模块进行渲染
36. 登录页面也会使用到公共样式
37. 我们可以定义一个中间的路由进行设置，因为会用到头部
38. 我们需要将base.xtpl再次进行分离，将侧边栏和内容部分分离
39. 添加讲师的时候左边没有对应的显示选中的处理
40. 我们判断地址的时候是用的全等于，我们只要在地址栏的pathname中有要跳转的大块路由的字段就行
41. 先将斜杠用slice去掉，用lastindexOf
42. 设计好数据库，下载mysql包，进行连接
43. 可以将连接数据库公共出来，我们需要对数据库进行配置
44. 创建一个新的文件夹config，里面有对数据库的配置文件db.js
```
var mysql=require('mysql');
var connection=mysql.createConnection({
    host:'localhost',
    user:'root',
    password:'root',
    database:'test'
});
module.exports=connection;
```
45. 需要添加一个专门用来对数据进行处理的模块models中的teacher.js
46. 我们需要处理teacher数据表的数据，引入数据库的配置文件
47. 在我们提交的时候要添加一个.gitignore文件忽略我们的node_modules目录
48. 在开发功能的时候要开发一个分支
49. 在添加讲师的时候，点击添加我们需要将数据提交过去
50. 首先将添加变成一个input的按钮，设置一个submit事件
51. 在base.xtpl下放置一个script的空位
52. 在我们的添加教师的模板中写我们的script，可以对数据进行提交
53. 通过add.js模块；哎添加一个讲师模块，引入我们设置好路径的jquery
54. 在添加讲师模块中使用我们定义好的模块
55. 提交的时候需要考虑将数据提交到哪个接口？以何种方式提交(get/post)，以及传递哪些参数
56. 后台将表单的字段name和数据库的对应起来
57. 前台通过ajax来获取数据
58. 提交事件中的this对象就是取回来的数据，我们要对其进行序列化serialize()
59. 要取消默认的提交行为，return false;
60. 提交的url接口我们要在后台设置好路由
61. serialize会忽略没有name属性的表单
62. 在路由中我们需要对ajax取回来的数据进行操作，在node中是用body-parser中间件
63. 在我们进行表单提交的时候，是用application/x-www-form-urlencoded进行提交的
64. 我们需要对其进行解析`app.use(bodyParser.urlencoded({ extended: false }))`
65. 如果是json字符串我们需要使用`app.use(bodyParser.json())`
66. 取回数据后我们需要调用model进行数据存储
67. 在模型中我们暴露出去方法，控制器中引入，就可以调用我们暴露出来的方法
68. 将通过body-parser取回来的数据放入到我们的数据库中去
```
exports.add=function(body){
    db.query('INSERT INTO `teacher` SET ?',body,function(err,rows){
        //...
    });
}
```
69. 提交中要对我们提交的数据的value值进行默认的设置，符合我们建立的数据库中的表
70. 当我们操作成功后用回调函数传回

### 模块化
1. 实现模块化需要解依赖的关系
2. 浏览器端JS是天然不能实现模块
3. 有些库弥补了浏览器端的JS的一些缺陷，实现了模块化并解决了依赖关系将这种库称为模块加载器，requirejs seajs
4. 这些模块加载器定义了自己的规范，必须遵照这些规范才能正常工作
5. 在seajs中，通过define()方法来定义模块
```
define(function(require,exporys,module){
    var sayHello=function(name){
        alert('Hello '+name);
    }
    exports.sayHello=sayHello;
});
```
6. 通过use()方法来加载/执行模块
```
seajs.use('./scripts/a',function(a){//a就是exports对象
    a.sayHello('Tom');
});
```
7. 通过require()引入模块
```
var a=require('./a');
```
8. 通过exports/module.exports暴露模块功能
9. 第三方插件不一定是按照这种方式书写
10. 比如我们要引入jquery模块，我们需要在jquery的源代码的最外层包裹一个define
11. jquery是默认支持模块化的，只不过支持的是AMD的requirejs，不是我们的seajs
```
if(typeof define === 'function' && define.cmd){
    defien(function(require,exports,module){
        module.exports=jQuery;
    });
}
```

### 包管理
1. `npm install express --save`
2. `npm install xtpl xtemplate --save` 
3. `npm install mysql --save` 
4. `npm install body-parser --save`

### 前后端分离
1. V+C前端 M=后端
2. C靠JS、CSS、HTML是不能实现的
3. 为了实现C前端团队需要依赖于一个后端语言Nodejs、PHP、Python
4. C可以实现加载任意的V，在V里通过XMLHttpRequest发送请求索取数据
5. 前后端分离可以实现前后端完全解耦，使得后端数据更加稳定统一
6. 可能会引起跨域的问题，JSONP解决
