## Node+Express快速搭建电影网站
### 开发框架的介绍
1. 后端(npm安装)
- Express快速搭建WEB应用
- mongoose对数据库进行快速建模的工具
- 模板引擎Jade
- 时间和日期的格式化Moment.js
2. 前端(Bower安装)
- jQuery
- Bootstrap
3. 本地开发环境(GRUNT集成)
- Less文件的编译【less】
- 样式的合并【cssmin】
- 语法的检查【JSHint】
- JS的压缩【UglifyJS】
- 前后端单元测试的实现【mocha】
- 服务的自动重启【nodemon】

### 开发步骤
1. 需求分析
2. 项目依赖初始化
3. 入口文件编码
4. 创建视图
5. 测试前端流程
6. 样式开发，伪造模板数据
7. 设计数据库模型
8. 开发后端逻辑
9. 配置依赖文件，网站开发结束

### 项目结构初始化
1. `npm install express`
2. `npm install jade`
3. `npm install mongoose`
4. `npm install bower -g`
5. `bower install boostrap`

### 编写入口文件编码app.js
```
// 严格模式
'use strict';
// 导入express包
const express=require('express');
// 设置端口
let port=process.env.PORT || 3000;
// 启动一个web服务器
let app=express();
// 设置视图的根目录
app.set('views','./views/');
// 设置默认的模板引擎
app.set('view engine','jade');
// 监听端口
app.listen(port);
// 打印日志信息
console.log('Jijmin started on port '+port);
```

### 测试前端流程
1. `localhost:3000/`
2. `localhost:3000/movie/1`
3. `localhost:3000/admin/movie`
4. `localhost:3000/admin/list`

### 主页模板
```
doctype
html
    head
        meta(charset='utf-8')
        title #{title}
    body
        h1 #{title}
```

### 路由配置
```
// index page
app.get('/',function(req,res){
    res.render('index',{
        title:'Jijmin 首页'
    })
});
// detail page
app.get('/movie/:id',function(req,res){
    res.render('detail',{
        title:'Jijmin 详情页'
    })
});
// admin page
app.get('/admin/movie',function(req,res){
    res.render('admin',{
        title:'Jijmin 后台录入页'
    })
});
// list page
app.get('/admin/list',function(req,res){
    res.render('list',{
        title:'Jijmin 列表页'
    })
});
```

### 解决jade模板更改所有文件缩进的问题
1. jade可以实现继承
2. 公共区块可以提取出来进行调用
3. 调整下目录的层次
4. 将四个页面放入pages文件夹下
5. views下面放一个布局文件，页面的公共部分
6. 引入样式
```
include ./includes/head
```
7. 引入头部区块
```
include ./includes/header
```
8. 更改的是内容部分
```
block content
```

### head.jade
```
.container
    .row
        .page-header
            h1=title
            small 重度科幻迷
```

### header.jade
```
link(href="/boostrap/dist/css/boostrap.min.css",rel="stylesheet")
script(src="/jquery/dist/jquery.min.js")
script(src="/boostrap/dist/js/boostrap.min.js")
```

### 首页的书写
1. 首页继承了我们的layout的页面
```
extends ../layout
```
2. 申明主要的内容
```
block content
```
3. 利用bootstrap中的样式框架进行样式输出
```
link(href="/boostrap/dist/css/boostrap.min.css",rel="stylesheet")
script(src="/jquery/dist/jquery.min.js")
script(src="/boostrap/dist/js/boostrap.min.js")
```
4. 提供一个公共的layout格式
5. 