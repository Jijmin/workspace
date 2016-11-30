# 视频播放项目
## 项目文件夹搭建
### project文件夹
1. 新建一个src文件夹，用来存放我们的测试的文件
2. 新建一个dist文件夹，用来存放我们处理后上线的文件
3. 建立一个index.js文件是我们的整个项目的入口点
4. 因为我们安装了node环境，用shell自动生成一个配置文件
5. `npm init -y`如果当前文件夹有中文，只能手动配置

### package.json文件设置
1. package.json中的scripts属性是配置文件中的配置文件
2. 对scripts进行配置，设置两个key，到时候只要执行`npm run 配置的键`就可以运行
3. "dev"：只要执行npm run dev 就会去执行src下面的代码
4. "dist"：只要执行npm run dist 就会去执行dist下面的代码
5. dist文件夹下的东西是对src的压缩和混淆，其他基本一样，相当于拷贝一份
6. 所以我们创建的服务器的端口是要不一样的，一般测试的是单数，上线的是双数端口
```
"dev":" cross-env PORT=8876 NODE_EVN=dev nodemon ./index.js" ,
//在shell中可以直接运行npm run dev就会执行这里面的代码
//cross-env是一个可以跨平台设置的环境变量的包，需要全局安装
//nodemon包是可以自启动服务器，也是需要全局安装
"dist":" cross-env PORT=8878 NODE_EVN=dist nodemon ./index.js" 
//在dist中我们将端口号设置为双数
```
7. 我们需要在全局环境下安装cross-env和nodemon包

### index.js
1. index.js的职责是这个项目的统一入口
2. 获取通过环境变量设置到NODE_EVN的值
```
"use strict";
let env=process.env.NODE_EVN.trim();
```
3. 判断当前执行的是开发环境还是工厂环境
```
if(env=='dev'){
    //开发环境
    require('./src/app.js');
}else{
    //生产环境
    require('./dist/app.js');
}
```

### src文件夹
1. app.js的职责是搭建开发环境的服务器
2. 我们是用express来搭建框架的，因此我们需要安装express`npm i express --save`
3. 导入express`let express=require('express');`
4. 实例化一个application
5. 初始化orm模型数据，orm是一个将数据库中的数据取出，转换成一个数组，里面是一个个对象，[{'uid':'1','uname':'admin':'upwd':'123456'},{......}]
```
const orm=require('orm');
app.use(orm.express('mysql://root:root@127.0.0.1:3306/nodedb',{
    define:function(db,models,next){
        models.userinfo=db.define('userinfo',{
            uid:{type:'serial',key:true},
            uname:String,
            upwd:String,
            ustatus:Number
        });
        models.videoinfo = db.define('videinfo',{
            vid:{type:'serial',key:true},
            vtitle:String,
            vsortno:Number,
            vvideid:String,
            vsummary:String,
            vremark:String,
            vimg:String
        });
        next();
    }
}));
```
6. 将网站的statics这个文件夹当做系统的静态资源文件夹
```
let phyPath=__dirname+'/statics';
app.use(express.static(phyPath));
```
7. 我们需要在src目录下设置静态资源存放区域
8. 设定路由规则
```
const accountRoute=require('./routes/accountRoute.js');
app.use('./account',accountRoute);
```
9. 监听端口
```
app.listen(port,()=>{
    console.log('express 服务器启动'+port);
});
```
10. 到时候我们需要将src中的文件拷贝到dist中，端口只能设置为变量
```
let port=process.env.PORT || 8877;
```
11. 服务器开启完毕，我们需要对具体的路由进行设置
```
"use strict";
const express=require('express');
let route=express.Router();
let accCtrl=require('../controller/accountCtrl.js');
route.get('/test',accCtrl.test);
module.exports=route;
```
12. 路由可以对页面进行跳转，在上面我们提供给了accCtrl一个test方法
```
"use strict";
exports.test=(req,res)=>{
    //...
}
```
13. 将查询到的信息用模板显示在页面上
```
"use strict";
const xtpl=require('xtpl');
const path=require('path');
exports.test=(req,res)=>{
    //设置请求头
    res.setHeader('Content-Type','text/html;charset=utf-8');
    req.models.userinfo.find({},{},(err,datas)=>{
        //读模板，需要读取的路径是当前文件
        xtpl.renderFile(path.join(__dirname,'../view/test.html'),{arr:datas},(err,html)=>{
            if(err){
                res.end(err.message);//将错误消息的摘要响应到浏览器 (aa is not function)
                console.log(err);//详细的错误堆栈信息打应到cmd面板上
                return;
            }
            res.end(html);
        });
    });
}
```

### 问题
- 总的来说程序没有报错，但是数据取不出来，推测应该是orm包没有下载好。
- 解决办法：今天回过头去看看，发现其实是数据库中建立的表是user表，但是取的时候是userinfo，一直就取不出来数据。

### 用gulp管理(src拷贝到dist)
1. 在项目的根目录下建立一个gulpfile.js的文件
2. 因为要用gulp管理，因此我们需要安装gulp`npm i gulp --save-dev`
3. 在gulpfile.js文件中引入gulp包
```
'use strict';
const gulp=require('gulp');
```
4. gulp管理后的提示
```
gulp.task('default',[],()=>{
    console.log('任务完成');
});
```
5. gulp进行JS压缩以及拷贝
```
//JS压缩
const minjs = require('gulp-uglify');
const es6toes5 = require('gulp-babel');
gulp.task('minjs',()=>{
    //将js文件压缩，将src目录结构拷贝到dist中
    gulp.src(['./src/controller/*.js','./src/routes/*.js','./src/model/*.js'],{base:'src'})
    .pipe(es6toes5({presets: ['es2015']}))//表示将 js文件由es6转换成es5
    .pipe(minjs())//压缩
    .pipe(gulp.dest('dist'));
    console.log('压缩完毕');
});
```

### 用git将项目管理起来
1. 在项目文件夹下的package.json中添加依赖项
2. 通过`git bash`中的`git init`初始化仓库
3. `git add .`将文件提交到暂缓区
4. `git commit -m 'add'`提交
5. 在githup上新创建一个仓库，中间的`.gitignore`设置为`node`
6. 将设置好的`.gitignore`文件内容拷贝一份到本地的`.gitignore`中
7. `git remote add origin master git仓库地址`取一个别名

### 维护一致的编码风格
在项目根目录中添加.editorconfig文件在其中固定所有的代码风格，那么所有的编辑器就会自动读取这个文件
```
# EditorConfig is awesome: <a onclick="javascript:pageTracker._trackPageview('/outgoing/EditorConfig.org');" href="http://EditorConfig.org">http://EditorConfig.org</a>

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# Matches multiple files with brace expansion notation
# Set default charset
[*.{js,html,css}]
charset = utf-8

# Tab indentation (no size specified)
[*.js]
indent_style = tab
tab_width = 4
```

## 后台管理页面级功能
### 列表静态页面设计
1. 后台几乎所有的页面都有一个公共的头部和左侧菜单栏
2. 只有中间的内容不一样，我们可以在改变的地方放置一个占位符
3. 另外新开页面写上我们的改变的内容
4. 新的页面采用继承模板的形式
5. 在静态资源区建立一个index.html文件
6. 因为采用的是Bootstrap技术，我们引入Bootstrap的公共模板
7. 应该先将页面进行分析，在进行布局

### 用git不能提交上去的问题
- 问题描述：将项目提交到githup上面进行托管的时候仓库拒绝访问
- 解决办法：因为在githup上面的仓库里面是存在文件的，直接让自己的仓库和远程的连接，出问题。
- 可以将仓库变成一个空仓库，README和忽略文件都不要有，再将自己的仓库和远程的连接，这样才能连接。
- 还可以先将远程仓库克隆下来，再将自己的文件拷贝到克隆下来的仓库，在进行提交，一样可以连接。

### 主页模块分离
1. 我们登录进来后看到的页面都是一块一块平凑而来
2. 我们要将中间部分分离成一个单独的模块
3. 新建一个页面layout作为公共的模板
4. 将要改变的部分提取出来，放上一个占位符
```
{{{block('body')}}}
```
5. 使用xtpl替换模板
6. 在list页面里面先将layout页面中公共的部分继承过来
```
{{extend('./layout.html')}}
{{#block('body')}}
//我们要替换的部分
{{/block}}
```
7. 现在我们将页面拆分开来，但是将来访问的时候是在一个页面上访问
8. 我们通过路由进行设置
```
127.0.0.1:8877/admin/list
```
9. 我们添加一个新的路由文件，搭建好主体
```
'use strict';
const express=require('express');
let adminRoute=express.Router();
module.exports=adminRoute;
```
10. 设计路由规则，首先我们要将数据显示出来
11. 查询显示页面设计一条get的路由规则，来实现列表数据的获取
```
const adminCtrl=require('./controller/adminCtrl.js');
adminRoute.get('/admin/list',adminCtrl.getlist);
```
12. 新增模块有两条路由规则
13. 首先要向服务器发送一个get请求获取到新增页面
```
adminRoute.get('/admin/list',adminCtrl.getlist);
```
14. 将用户填写好的表单数据post提交给服务器，服务器将数据插入到数据库的表中
```
adminRoute.post('/admin/add',adminCtrl.postadd);
```
15. 编辑操作中我们需要传参数，要确定当前是哪条数据需要修改
```
adminRoute.get('/admin/edit/:vid',adminCtrl.getedit);
adminRoute.post('/admin/edit',adminCtrl.postedit);
```
16. 删除，我么需要通过参数删除特定的数据
```
adminRoute.get('/admin/delete/:vid',adminCtrl.getdelete);
```
17. 改造我们的list.html，将需要产生动态数据的地方写上占位符
```
{{#each(list)}}
<tr>
    <td>{{this.vid}}</td>
    <td>{{this.vtitle}}</td>
    <td>{{this.vsortno}}</td>
    <td>{{this.vvideid}}</td>
    <td>
        //利用a标签的跳转向服务器发起请求
        <a href="/admin/edit/{{this.vid}}"  class="btn btn-success">编辑</a>
        <input type="button" class="btn btn-danger" onclick="del{{this.vid}}" value="删除">
    </td>
</tr>
{{/each}}
```
18. 查询的getlist函数中我们需要将数据库中的数据渲染到页面上
19. 先去数据库中查找videoinfo这个表中的数据
20. 调用xtpl.renderFile()将页面渲染
```
exports.getlist=(req,res)=>{
    //查询
    res.models.videoinfo.find({},{},(err,datas)=>{
        let obj={list:datas};
        xtpl.renderFile(path.join(__dirname,'../view/list.html'),obj,(err,html)=>{
            if(err){
                res.end(err.message);
                return;
            }
            res.end(html);
        });
    });
}
```
21. 将路由规则引入app.js中
```
const adminRoute = require('./routes/adminRoute.js');
app.use('/',adminRoute);
```
22. app.js里面的videoinfo表写错了单词
23. layout.html中路径问题
```
<link href="/node_modules/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">
```

### params、query的区别
1. 如果url参数是通过?传递给服务器的话，使用req.query.id1可以获取的到值
2. 如果url参数是通过:传递给服务器的话，使用req.params.id2可以获取的到值，如果有了这个种格式，url地址栏就只要输入要传入参数的值就行，不需要带上键

### 新增页面
1. 在按钮上面增加一个点击事件进行跳转
```
<input type="button" class="btn btn-info" onclick="window.location='/admin/add';" value="新增">
```
2. 增加add.html页面，同样也要继承layout.html
3. 用Bootstrap写一个新增页面，里面有一个富文本编辑器，需要我们用js动态生成
4. 在控制器中将页面渲染出去
```
exports.getadd = (req, res) => {
  xtpl.renderFile(path.join(__dirname, '../view/add.html'),{}, (err, html) => {
    if (err) {
      res.end(err.message);
      return;
    }
    res.end(html);
  });
}
```
5. 解决乱码问题
```
app.all('admin/*',(req,res,next)=>{
    res.setHeader('Content-Type','text/html;charset=utf-8');
    next();
});
```

### 富文本编辑器
1. 引入JS文件
```
<script type="text/javascript" charset="utf-8" src="/ueditor/ueditor.config.js"></script>
<script type="text/javascript" charset="utf-8" src="/ueditor/ueditor.all.min.js"> </script>
<script type="text/javascript" charset="utf-8" src="/ueditor/lang/zh-cn/zh-cn.js"> </script>
```
2. 将ueditor文件夹拷贝到静态资源目录下
3. 创建一个富文本编辑器
```
<script id="editor" type="text/plain" style="width:680px;height:500px;"></script>
<script>
UE.getEditor('editor');
</script>
```
4. 我们拷贝过来代码的时候要注意，重定向跳转的时候路径有问题，需要更改
```
res.redirect('/ueditor/nodejs/config.json');
```
5. 拷贝代码中用到了path，我们需要将path导入，以及我们的路径名需要更改
```
app.use("/ueditor/ue", ueditor(path.join(__dirname, 'statics'), function(req, res, next) {
    //...
});
```
6. 将bodyParser这个包加载到express中使用
```
app.use(bodyParser());
```
7. 设定上传图片的路由规则代码
```
app.use("/ueditor/ue", ueditor(path.join(__dirname, 'statics'), function(req, res, next) {
  // ueditor 客户发起上传图片请求
  if(req.query.action === 'uploadimage'){
    var foo = req.ueditor;
    var imgname = req.ueditor.filename;
    var img_url = '/images/ueditor/';
    res.ue_up(img_url); //你只要输入要保存的地址 。保存操作交给ueditor来做
  }
  //  客户端发起图片列表请求
  else if (req.query.action === 'listimage'){
    var dir_url = '/images/ueditor/';
    res.ue_list(dir_url);  // 客户端会列出 dir_url 目录下的所有图片
  }
  // 客户端发起其它请求
  else {
    res.setHeader('Content-Type', 'application/json');
    res.redirect('/ueditor/nodejs/config.json');
}}));
```
8. 我们上传到服务器的图片保存地址
```
statics/images/ueditor/用户上传的图片
```
9. 添加了ueditor之后会报错//TODO

### 获取post请求数据
1. 在路由规则里面的请求报文对象接受一个data事件，可以获得提交数据
```
route.post('/admin/add',(req,res)=>{
    req.on('data',(data)=>{
        console.log(data);
    });
})
```
2. 通过bodyParser这个第三方包去获取
- 在app.js中使用app.use(require('body-parser'))
- 通过req.body.vtitle获取到请求报文体中的vtitle的值

### 新增的功能实现
1. 获取到请求报文体中的值req.body.vname
```
exports.postadd=(req,res)=>{
    let vtitle=req.body.vtitle;
    let vsortno=req.body.vsortno;
    let vvideid=req.body.vvideid;
    let vsummary=req.body.vsummary;
    let vimg=req.body.vimg;
    let vremark=req.body.editorValue;
}
```
2. 交给orm将数据插入到mysql表中的videoinfo中
```
let videoData={
    vtitle:vtitle,
    vsortno:vsortno,
    vvideid:vvideid,
    vsummary:vsummary,
    vremark:vremark,
    vimg:vimg
};
req.models.videoinfo.create(videoData,(err,item)=>{
    if(err){
        res.end(err.message);
        return;
    }
    //...
});
```
3. 提示新增成功并跳转到列表页面
```
res.end('<script>alert("新增成功");window.location="/admin/list";</script>');
```
4. *Cannot read property 'vsortno' of undefined*错误
- 解决办法：app.use(bodyParser());中间件的使用在路由规则设定之前

### 视频上传的功能
一般一些小网站是不支持视频上传的功能的，因为存储视频需要很大的空间，一般是将视频图片存储在云上，直接使用提供给我们的接口看来实现视频的上传，如果大量的视频在自己的服务器上面，会增加流量和内存的消耗。而且存在自己的服务器上会被下载下来，不方便管理。

### 编辑功能的实现
1. 点击编辑按钮，浏览器会进行相应的跳转，跳转到特定的路由规则
2. 我们需要对特定条数的数据进行编辑，因此我们的路由规则中是有参数的
3. 我们点击编辑按钮会将特定的vid传给浏览器的参数
4. 向服务器发送请求，服务器或根据路由规则实现功能
5. 首先我们需要从数据库中取出特定的那条数据
6. 将取出的数据渲染到页面上，将特定数据填充到form表单中的所有元素的value中
7. 用户进行修改后，我们需要将所有的数据重新提交到服务器存入数据库中
8. 但是我们在form表单中提交的路由规则中的参数不是固定的，应该用vid替换
9. 在模板页中我们需要将value用占位符替换
10. 但是最大的问题是我们富文本中的内容没有办法用value显示出来
11. 撇开富文本的问题，我们设置路由规则
12. 首先从url中获取到vid的值
```
let vid=req.params.vid;
```
13. 根据vid的值从mysql数据库中查找视频数据
```
//通过orm去数据库查找返回的结果是一个数组
```
14. 拿到数据后渲染edit.html页面
```
exports.getedit=(req,res)=>{
    let vid=req.params.vid;
    req.models.videoinfo.find({vid:vid},{},(err,data)=>{
        if(data.length<=0){
            res.end('id值非法，没有对应数据');
            return;
        }
        xtpl.renderFile(path.join(__dirname, '../view/edit.html'),data[0], (err, html) => {
        if (err) {
          res.end(err.message);
          return;
        }
        res.end(html);
    });
    });
}
```

### 解决富文本显示我们存在数据库中数据的问题
1. 查看官方文档的时候发现可以通过设置内容来实现富文本框显示内容
```
var ue=UE.getEditor('editor');
ue.setContent('欢迎使用',false);
```
2. 但是刷新页面后发现富文本的值不能使用
3. 当我们的浏览器从上往下执行，执行到`var ue=UE.getEditor('editor');`时，页面渲染是需要时间的，但是我们马上就要对它设值，所以不能用
4. 可以设置一个延迟，但是我们不知道要延迟多久
```
var content='{{vremark}}';
setTimeout(function(){
  ue.setContent(content,false);
}, 1000);
```
5. 但是我们输出的是一段被解析成HTML标签的字符串
```
<p><img src="/images/ueditor/802142432136400896.JPG" title="" alt="IMG_0635.JPG"/></p>
```
6. 需要将`&lt`;自动变成小于号
```
content=$('<span />').html(content).text();
```
7. 由于我们的jQuery是在页面底部加载的，因此我们需要添加onload事件。
8. 但是我们的富文本编辑器是不成功的，原生的富文本编辑器已经不能满足我们的需求，需改进下富文本编辑器。
9. 假设我们创建完编辑器之后然后执行一个回调函数，回调函数里面再去执行我们的设置值的事情。
10. 在我们的ueditor.all.js中我们找到getEditor函数主体。
11. 由上面推测可知我们需要传入一个回调函数，但是在return之前来执行这个回调函数显然是不合理的，因为我们的JS代码执行是非常快，而富文本编辑器渲染是需要时间的。
12. 我们可以在全局设置一个回调函数变量，在getEditor函数主体对全局变量赋值
13. 回调函数会在我们函数执行完毕之后执行，这样我们的富文本就能很好的渲染
```
var ve=UE.getEditor('editor',{},function(){
    ue.setContent(content,false);
});
```

### ittun使用
1. 启动服务器
2. 将startup-api接口里面的配置项改变，要填上子域名和端口号，一定是本地启动的端口号

### 修改后保存的处理
1. 获取主键
```
exports.postedit=(req,res)=>{
    let vid=req.params.vid;
    //...
}
```
2. 利用orm将数据更新到数据库中
```
req.models.videoinfo.get(vid,(err,data)=>{
    //将页面上所有的数据同步给data中相应的属性
    data.vtitle=req.body.vtitle;
    data.vsortno=req.body.vsortno;
    data.vvideid=req.body.vvideid;
    data.vsummary=req.body.vsummary;
    data.vremark=req.body.vremark;
    data.vimg=req.body.vimg;
    data.save();
});
```
3. 在form表单中的路由规则写错了，直接复制过来的
```
<form action="/admin/edit/{{vid}}" method="post" class="form-horizontal">
```

### 删除模块
1. 我们一样也可以采用上面的模式来，但是为更好的用户体验
2. 应该采用ajax来实现页面无刷新直接删除
3. 我们在写代码的时候最好少嵌套
4. jQuery中的ajax
- $.get(url,参数对象，成功的回调函数，服务器响应回来的数据格式)
- $.post(url,参数对象，成功的回调函数，服务器响应回来的数据格式)
- $.getJSON(url,参数对象，成功的回调函数)
- $.ajax()最全的
5. 我们需要根据后端返回的格式来使用JSON或XML，我们这里传回来的参数是JSON
6. 我们不需要对服务器传递参数，因为我们已经通过：形式来传参，因此data是null
```
var url='/admin/delete/{{vid}}';
var data=null;
$.getJSON(url,data,function(jsObj){
    //...
});
```
7. 在这个ajax中，服务器响应回来的json对象会转换成一个JS对象
8. 这种删除在服务器可能会报错，为了更好的容错性，我们需要将错误响应到回调函数中
9. 在回调函数中我们需要根据vid的值删除数据
10. 然后将处理的结果响应回去，我们调用end方法的时候，都是将消息返回给浏览器
11. 但是中间是通过我们的异步对象xhr中的onreadystatechange
12. onreadystatechange这个回调函数的方法体其实是这个$.getJSON()中的回调函数
13. 这个函数就会将你返回的的数据转换成一个JSON对象
14. 如果你的数据删除错误，那么应该返回一个错误信息，成功就有成功的处理
15. 在回调函数中一定要判断服务器响应给我们的数据到底是成功还是失败的
16. 那么如果我们返回的JS对象中有一个状态，告诉我们是成功还是失败，以及还要见错误信息提示出来
```
function del(vid){
  if(!confirm('您是否删除这条数据')){
    return;
  }
  var url='/admin/delete/{{vid}}';
  var data=null;
  $.getJSON(url,data,function(jsObj){
    if(jsobj.status==1){
      alert(jsObj.message);
    }else{//成功
      alert(jsObj.message);
      // window.location.reload();//会出现页面翻白
      window.location=window.location;
    }
  });
}
```
17. 因为已经商量好的响应回去的是JSON数据{"status":1,"message":"提示语"}
18. 
```
exports.getdelete=(req,res)=>{
    let vid=req.params.vid;
    //返回的是一个JSON对象
    let resObj={status:0,message:'删除成功'};
    //一般在程序开发过程中，我们采用这种方式来判断是否是成功
    let success=0;
    let fail=1;
    req.models.videoinfo.get(vid,(err,data)=>{
        if(err){
                resObj.status=fail;
                resObj.message=err.message;
                res.end(JSON.stringify(resObj));
                return;
            }
        data.remove((err)=>{
            if(err){
                resObj.status=fail;
                resObj.message=err.message;
                res.end(JSON.stringify(resObj));
                return;
            }
            res.end(JSON.stringify(resObj));
        });
    });
}
```

### get()和find()区别
1. get()返回的是唯一的一条数据
2. find()返回的是一个数组

### 关于API的文档如何去看
1. 写静态(里面的数据是通过ajax去访问后台程序员开发出来的api接口进行操作的)
2. 一般会得到一些由后台人员书写的API接口规范
3. 我们需要对页面进行怎么样的操作，一般都是看什么路由规则
4. 商定好请求方式是get还是post，传递了哪些参数，返回的格式以及具体的格式样例

### 登录页面思路
1. 确定登录的路由规则 
2. 通过get请求拿到我们的登录页面
3. 当用户填写完数据之后点击提交按钮，会发送一个我们的post请求
4. 验证码是图片，也要向服务器发送一次请求
5. 我们要将用户名密码验证码发送给服务器
6. 服务器获取到浏览器发送过来的用户填写的验证码字符串和服务器生成的验证码字符串比较
7. 验证码的作用就是防止暴力破解
8. 我们先在服务器创建好一个验证码字符串，并保存起来
9. 将创建好的验证码以图片的形式显示在浏览器
10. 服务器拿到浏览器发送过来的验证码和自己本身生成的验证码进行比较
11. 我们要先是登录才能进入我们的页面中访问我们的功能
12. 我们使用express-session保存我们的验证码
13. 每个人访问我们的网站，验证码是不一样的
14. 浏览器在发送请求的时候要将自己的浏览器的id一起发送过去
15. 浏览器发送请求，服务器会通过set-cooike随机字符串产生一个浏览器的身份ID
16. 当浏览器获取到set-cookie指令以后，就会将身份ID保存到浏览器的内存或者磁盘上
17. 当下一次再去发送这个网站的其他页面请求的时候，就会同时带上这个身份ID给服务器

### session和cooike
- session：是一种服务端的技术，一般是结合cookie来进行服务器的状态维持，本质上是在服务器的内存中开辟了一块空间
- cookie是客户端的一种能够存储少量(4k)文本的技术，类似于sessionStorage和localStorge这种技术
- 一般就是用在登录以后的状态的维持中的浏览器身份id的保存(express-session)就是用了它

### 登录页面的渲染
1. 登录页面的bootstrap实现
2. 我们每次点击图片的时候就会进行一次请求
3. 要将图片的src设置为跳转的路由
```
$('#vcodeimg').click=function(){
    //在IE浏览器中不能实现刷新
    // var url='/account/vcode';
    var url='/account/vcode'+Math.random();
    //这样页面每次刷新的时候的url都不一样，就可以刷新
    // this.src=url;//原生的
    $(this).attr('src',url);
}
```
4. 设置路由规则
```
route.get('/account/login',accCtrl.getlogin);
route.post('/account/login',accCtrl.postlogin);
route.get('/account/vcode',accCtrl.getvcode);
```
5. 第一次发送请求渲染页面
```
exports.getlogin = (req,res)=>{
    xtpl.renderFile(path.join(__dirname,'../view/login.html'),{},(err,html)=>{
        if(err)
        {
            res.end(err.message); 
            console.log(err); 
            return; 
        }
        res.end(html);
    });
}
```

### 验证码实现
1. 产生一个验证码字符串
```
let vcode=parseInt(Math.random()*9000+1000);
```
2. 将验证码字符串变成一个图片响应回去
```
const captchapng = require('captchapng');
var p = new captchapng(80,30,vcode);
p.color(0, 0, 0, 0);
p.color(80, 80, 80, 255);
var img = p.getBase64();
var imgbase64 = new Buffer(img,'base64');
res.writeHead(200, {
  'Content-Type': 'image/png'
});
res.end(imgbase64);
```
3. 当浏览器第一次请求网页的时候就将浏览器向浏览器写一个身份标识到他的cookie中
```
const session=require('express-session');
app.use(session({
  secret: 'keyboard cat',//加密的秘钥
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}));
```
4. 将验证码字符串保存在session中，就是将验证码存在服务器
```
req.session.vcode=vcode;
```
5. 点击图片不能刷新问题的解决
```
$('#vcodeimg').click(function(){//jQuery不熟悉，语法上的错误
  var url='/account/vcode?vid='+Math.random();
  $(this).attr('src',url);
});
```

### 登录post验证过程
1. 获取浏览器发送过来的请求报文体中的数据
2. 获取服务器中属于这个浏览器的验证码字符串与浏览器提交过来的验证码对比
3. 验证用户名和密码的正确性
4. 跳转到/admin/list
```
exports.postlogin = (req,res)=>{
    let uname=req.body.uname;
    let upwd=req.body.upwd;
    let vcode=req.body.vcode;
    let vodeFormSession=req.session.vcode;
    if(vodeFormSession!=vcode){
      res.end('<script>alert("验证码输入错误");</script>');
      return;
    }
    req.models.user.find({uname:uname,upwd:upwd},{},(err,datas)=>{
      if(err){
          res.end('<script>alert("用户名和密码错误");</script>');
          return;
      }
      res.end('<script>alert("登录成功");window.location="/admin/list"</  script>');
    });
}
```
5. 验证码输入总是错误
在app.js中初始化数据库模型的时候，将models上的属性写错，写成userinfo，这边用的是user
6. 我们登录时要将我们的用户名记录，显示在list页面中
7. 在跳转之前将我们的验证码存储在req.session中
```
req.session.uname=uname;
```
8. 在模板页面中显示我们的用户
```
<p class="navbar-text navbar-right">欢迎【{{loginname}}】登陆<a href="#" class="navbar-link">退出</a></p>
```
9. 我们加载list页面时将用户也一起输出
```
let obj={list:datas,loginname:req.session.uname};
```

### 统一登录之后才能进入后台
1. 进入到我们的admin中所有的路由规则都需要进行验证
2. 只有我们是登录成功的才能进行访问
3. 只要我们session中有值，那么说明我们登录成功
```
app.all('/admin/*',(req,res,next)=>{
    if(req.session.uname==null){
        res.end('<script>alert("用户未登录");window.location="/account/login";</script>');
    }
    next();
});
```

### 退出功能的实现
1. 退出是一个超链接，超链接可以跳转，发送请求
```
<a href="/account/logout" class="navbar-link">退出</a>
```
2. 我们需要给退出提供一个路由规则
```
route.get('/account/logout',accCtrl.getlogout);
```
3. 将req.session.uname这个登录标识的值设置为null
```
req.session.uname=null;
```
4. 跳转到登录页面
```
res.end('<script>alert("退出成功");window.location="/account/login"</script>');
```

### 用户登录不成功时优化
```
res.end('<script>alert("验证码输入错误");window.location="/account/login"</script>');
res.end('<script>alert("用户名和密码错误");window.location="/account/login"</script>');
```

### 密码的加密(md5)
- 在数据库中存在的是明文的话，将数据库破解后，就会很危险，要将数据库的密码存储为密文。
- 通常加密方式
    + SHA1
    + SHA128
    + SHA256
    + MD5
- 对称加密：可逆
- 非对称加密
    + 公钥负责加密，通常在互联网上流通的
    + 私钥负责解密，一般是存储在服务器上
    + 应用：https，通过加密以后将数据在互联网上流通的
- 行业规则：MD5加密的密码不可逆
```
const crypto=require('crypto');
const secret='adcdefg';
const hash=crypto.createHmac('md5',secret)
                 .update('I love cupcakes')
                 .digest('hex');
```
1. 在项目中设置一个工具类的文件夹
2. 工具类的文件夹中新建一个md5entry.js专门负责我们的加密
3. 在我们nodejs的官网上面我们可以找到一个加密模块crypto
4. 将MD5加密过程封装成一个模块，并将模块暴露出去
```
'use strict';
const crypto=require('crypto');
const secret='adcdefg';
module.exports=function(str){
    return crypto.createHmac('md5',secret)
                 .update(str)
                 .digest('hex');
};

```
5. 在我们登录提交时将密码加密
```
upwd.=require('../tools/md5entry.js')(upwd);
```

### 分页功能
1. 查询出真正属于这页的数据
- offset：跳过多少条数据
- limit：取得多少条数据
2. 确定好这个分页的总页数
- 分页的总页数=表中数据的总条数/页容量
- orm.models.videoinfi.count();
3. 页面和页容量都要传递服务器
4. 获取到数据表中的数据总条数
```
req.models.videoinfo.count({},(err,count)=>{
    //...
}
```
5. 计算出总页数，产生一个数组
```
let pages=[];
for (var i = 1; i < count; i++) {
    pages.push(i);
}
//在数据库中查找后传给我们的浏览器
let obj={list:datas,loginname:req.session.uname,pages:pages};
```
6. 因为是动态生成，我们需要更改视图 
```
{{#each(pages)}}
    <li><a href="/admin/list?pageindex={{this}}&pagesize=1">{{this}}</a></li>
{{/each}}
```
7. 动态计算出跳过的总条数
```
let pageIndex=parseInt(req.query.pageindex);
let pageSize=parseInt(req.query.pagesize);
let skipCount=(pageIndex-1)*pageSize;
req.models.videoinfo.find({},{offset:skipCount,limit:pageSize},(err,datas)=>{
    //...
}
```
8. 让刚开始进来list的时候有一个默认值
```
let pageIndex=parseInt(req.query.pageindex||1);
let pageSize=parseInt(req.query.pagesize||1);
```
9. 一般的情况下一页显示10条数据
```
for (var i = 1; i < Math.ceil(count/10)+1; i++) {
    pages.push(i);
}   
```

### 查找
1. 模糊查询
2. 在计算总条数的时候有条件
```
req.models.videoinfo.count({vtitle:orm.like('%node%')},(err,count)=>{
    //...
}
```
3. 将数据动态化，点查询的时候提交
4. 也有分页
5. 给查询绑定点击事件
```
<input type="button" class="btn btn-success" onclick="search()" value="查询">
```
6. 获取文本框的值
```
var searchValue=$('#title').val();
```
7. 发送请求到/admin/list中
```
window.location='/admin/list?searchValue='+searchValue;
```
8. 在服务器获取到我们输入框的值
```
let searchValue=req.query.searchValue;
```
9. 更改我们的模板
```
{vtitle:orm.like('%'+searchValue+'%')}
```
10. 我们用到了我们的orm包，将orm设置为全局
```
global.orm=orm;
```
11. 我们应该将我们输入的查询信息存储起来
12. 在我们的url中有我们的查询信息
13. 在我们的input标签中我们需要添加一个value值
```
<input type="text" class="form-control" placeholder="请输入视频名称" aria-describedby="basic-addon1" id="title" value="{{searchValue}}">
```
14. 将来searchValue可以通过渲染回去
```
let obj={list:datas,loginname:req.session.uname,pages:pages,searchValue:searchValue};
```
15. 但是有一个小问题就是我们的放在页码上的时候我们会发现并没有searchValue值，我们需要对代码进行修改
```
<li><a href="/admin/list?pageindex={{this}}&pagesize=10?searchValue={{searchValue}}">{{this}}</a></li>
```

### 使用gulp进行JS和CSS的压缩
1. 使用gulp-uglify对JS进行压缩
```
const minjs = require('gulp-uglify');
```
2. 因为我们使用了ES6语法，我们需要将其全部转换
```
const es6toes5 = require('gulp-babel');
```
3. 对JS进行压缩
```
gulp.task('minjs',()=>{
    //将js文件压缩，将src目录结构拷贝到dist中
    gulp.src(['./src/controller/*.js','./src/routes/*.js','./src/model/*.js','./src/tools/*.js','./src/statics/js/*.js'],{base:'src'})
    .pipe(es6toes5({presets: ['es2015']}))//表示将 js文件由es6转换成es5
    .pipe(minjs())//压缩
    .pipe(gulp.dest('dist'));
    console.log('压缩完毕');
});
```
4. CSS压缩
```
const mincss=require('gulp-clean-css');
gulp.task('mincss',()=>{
    gulp.src(['./src/statics/css/*.css'],{base:'src'})
    .pipe(mincss({compatibility: 'ie8'}))
    .pipe(gulp.dest('dist'));
    console.log('CSS压缩完毕');
});
```

### gulp-rev进行css的文件名添加MD5
- 我们的项目上线了，那个用户端的CSS文件可能存储在用户端的浏览器中，我们更新了CSS样式之后，如果用户不强制刷新的话有可能看不到更新
- 我们使用gulp-rev进行css的文件名添加MD5
```
const rev=require('gulp-rev');
gulp.task('cssmd5',()=>{
    gulp.src(['./src/statics/css/*.css'],{base:'src'})
    .pipe(rev())//通过rev()在CSS中加上一个md5的字符串的后缀
    .pipe(gulp.dest('dist'));
    console.log('CSS压缩完毕');
});
```
- 我们需要将我们的md5文件和html中的一致
- 不仅仅是CSS可以md5化，我们的JS图片这些静态资源都可以MD5化
- 将html文件中引用到的css文件的名称替换成MD5
```
const revCollector=require('gulp-rev-collector');
```
- 自动生成一个manifest.json文件文件中将原来的site.css和新的有md5建立key-vlue关系
```
gulp.task('cssmd5',()=>{
    gulp.src(['./src/statics/css/*.css'],{base:'src'})
    .pipe(rev.manifest())
    .pipe(gulp.dest('./src/rev/'));//拷贝到这个目录下
});
```

### 利用gulp-rev-collector进行文件路径替换
```
const revCollector=require('gulp-rev-collector');
gulp.task('cssmd5replace',()=>{
    gulp.src(['./src/rev/**/*.json','./src/view/*.html'],{base:'src'})
    .pipe(revCollector())//通过rev()在CSS中加上一个md5的字符串的后缀
    //.pipe(htmlmin({collapseWhitespace: true}))
    .pipe(gulp.dest('dist'));
    console.log('CSS压缩完毕');
});
```
但是我们文件的路径还是没有更改，我们需要手动设置rev-mainfest.json文件里面的文件路径
```
{
  "/css/site.css": "/css/site-20a5d09f15.css"
}
```
占时还没有找到将上面路径解决的办法，只能自己手动更改

### 用一条指令直接将用gulp进行打包
```
gulp.task('default',['minjs','mincss','cssmd5','cssmd5replace'],()=>{
    console.log('任务完成');
});
```

### 利用gulp-htmlmin和gulp-imagemin完成html和图片的压缩
```
const htmlmin = require('gulp-htmlmin');
gulp.src(['./src/rev/**/*.json','./src/view/*.html'],{base:'src'})
.pipe(htmlmin({collapseWhitespace: true}));
```