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
```
doctype
html
    head
        meta(charset='utf-8')
        title #{title}
        //引入样式文件
        include ./includes/header
    body
        //头部公共部分
        include ./includes/head
        block content
```
5. 主页模板
```
extends ../layout
block content
    .container
        .row
            each item in movies
                .col-md-2
                    .thumbnail
                        a(href="/movie/#{item._id}")
                            img(src="#{item.poster}",alt="#{item.title}")
                        .caption
                            h3 #{item.title}
                            p: a.btn.btn-primary(href="/movie/#{item._id}",role="button")
                                观看预告片
```

### 详情页模板
```
extends ../layout
block content
    .container
        .row
            .col-md-7
                embed(src="#{movie.flash}",allowFullScreen="true",quality="high",width="720",height="600",align="middle",type="application/x-shockwave-flash")
            .col-md-5
                dl.dl-horizontal
                    dt 电影名字
                    dd=movie.title
                    dt 导演
                    dd=movie.director
                    dt 国家
                    dd=movie.country
                    dt 语言
                    dd=movie.language
                    dt 海报地址
                    dd=movie.poster
                    dt 片源地址
                    dd=movie.flash
                    dt 上映年份
                    dd=movie.year
                    dt 简介
                    dd=movie.summary
```

### 后台录入页
```
extends ../layout
block content
    .container
        .row
            form.form-horizontal(method="post",action="/admin/movie/new")
                .form-group
                    label.col-sm-2.control-label(for="inputTitle") 电影名字
                    .col-sm-10
                        input#inputTitle.form-control(type="text",name="movie[title]",value="#{movie.title}")
                .form-group
                    label.col-sm-2.control-label(for="inputDirector") 电影导演
                    .col-sm-10
                        input#inputDirector.form-control(type="text",name="movie[director]",value="#{movie.director}")
                .form-group
                    label.col-sm-2.control-label(for="inputCountry") 国家
                    .col-sm-10
                        input#inputCountry.form-control(type="text",name="movie[country]",value="#{movie.country}")
                .form-group
                    label.col-sm-2.control-label(for="inputLanguage") 语言
                    .col-sm-10
                        input#inputLanguage.form-control(type="text",name="movie[language]",value="#{movie.language}")
                .form-group
                    label.col-sm-2.control-label(for="inputPoster") 海报地址
                    .col-sm-10
                        input#inputPoster.form-control(type="text",name="movie[poster]",value="#{movie.poster}")
                .form-group
                    label.col-sm-2.control-label(for="inputFlash") 片源地址
                    .col-sm-10
                        input#inputFlash.form-control(type="text",name="movie[flash]",value="#{movie.flash}")
                .form-group
                    label.col-sm-2.control-label(for="inputYear") 上映年份
                    .col-sm-10
                        input#inputYear.form-control(type="text",name="movie[year]",value="#{movie.year}")
                .form-group
                    label.col-sm-2.control-label(for="inputSummary") 简介
                    .col-sm-10
                        input#inputSummary.form-control(type="text",name="movie[summary]",value="#{movie.summary}")
                .form-group
                    .col-sm-offset-6.col-sm-6
                    button.btn.btn-default(type='submit') 录入
```

### 列表页
```
extends ../layout
block content
    .container
        .row
            table.table.table-hover.table-bordered
                thead
                    tr
                        th 电影的名字
                        th 导演
                        th 国家
                        th 上映年份
                        th 录入时间
                        th 查看
                        th 更新
                        th 删除
                    tbody
                        each item in movies
                        tr(class="item-id-#{item._id}")
                            td #{item.title}
                            td #{item.director}
                            td #{item.country}
                            td #{item.year}
                            td #{moment(item.meta.createdAt).format('MM/DD/YYYY')}
                            td: a(target="_blank",href="../movie/#{item._id}") 查看
                            td: a(target="_blank",href="../admin/update/#{item._id}") 修改
                            td
                                button.btn.btn-danger.del(type="button",data-id="#{item._id}") 删除

```

### 数据的伪造
```
app.get('/',function(req,res){
    res.render('index',{
        title:'Jijmin 首页',
        movies:[
            {
                title:'机械战警',
                _id:1,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            },
            {
                title:'机械战警',
                _id:2,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            },
            {
                title:'机械战警',
                _id:3,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            },
            {
                title:'机械战警',
                _id:4,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            },
            {
                title:'机械战警',
                _id:5,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            },
            {
                title:'机械战警',
                _id:6,
                poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5'
            }
        ]
    })
});
```
```
app.get('/movie/:id',function(req,res){
    res.render('detail',{
        title:'Jijmin 详情页',
        movie:{
            director:'何塞·帕迪里亚',
            country:'美国',
            title:'机械战警',
            year:2014,
            language:'英语',
            flash:'http://player.youku.com/player.php/sid/XNjA1Njc0NTUy/v.swf',
            summary:'翻拍自1978年同名科幻经典、由《精英部队》导演何塞·帕迪里亚执导的新版《机械战警》发布首款预告片。大热美剧《谋杀》男星乔尔·金纳曼化身新“机械战警”酷黑战甲亮相，加里·奥德曼、塞缪尔·杰克逊、迈克尔·基顿三大戏骨绿叶护航。'
        }
    })
});
```
```
app.get('/admin/movie',function(req,res){
    res.render('admin',{
        title:'Jijmin 后台录入页',
        movie:{
            title:'',
            director:'',
            country:'',
            year:'',
            poster:'',
            flash:'',
            summary:''
            language:'',
        }
    })
});
```
```
app.get('/admin/list',function(req,res){
    res.render('list',{
        title:'Jijmin 列表页',
        movies:[
            {
            title:'机械战警',
            _id:1
            director:'何塞·帕迪里亚',
            country:'美国',
            year:2014,
            poster:'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5',
            language:'英语',
            flash:'http://player.youku.com/player.php/sid/XNjA1Njc0NTUy/v.swf',
            summary:'翻拍自1978年同名科幻经典、由《精英部队》导演何塞·帕迪里亚执导的新版《机械战警》发布首款预告片。大热美剧《谋杀》男星乔尔·金纳曼化身新“机械战警”酷黑战甲亮相，加里·奥德曼、塞缪尔·杰克逊、迈克尔·基顿三大戏骨绿叶护航。'
            }
        ]
    })
});
```

### 对静态资源的加载
1. 导包
```
const path=require('path');
const bodyParser = require('body-parser');
```
2. 更改视图默认目录
```
app.set('views','./views/pages');
```
3. 有提交表单，将表单数据格式化
```
app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());
```
4. 设置静态路由
```
app.use(express.static(path.join(__dirname,'bower_components')));
```

### 静态资源不能加载问题
1. 在加载静态资源的时候文件路径错误
```
Failed to load resource: the server responded with a status of 404 (Not Found)
```
2. 注意静态资源的路径问题，不要书写错误

### jade模板书写格式问题
```
Error: E:\work\nodeMongodb\views\pages\list.jade:25
   23| //- td #{moment(item.meta.createdAt).format('MM/DD/YYYY')} 
   24| td 
 > 25|  //- a(target="_blank",href="../movie/#{item._id}") 查看 
   26|  a(target="_blank",href="../admin/update/#{item._id}") 查看 
   27| td 
   28| a(target="_blank",href="../admin/update/#{item._id}") 修改 

Invalid indentation, you can use tabs or spaces but not both
   at Object.Lexer.indent (E:\work\nodeMongodb\node_modules\jade\lib\lexer.js:790:15)
   at Object.Lexer.next (E:\work\nodeMongodb\node_modules\jade\lib\lexer.js:941:15)
   at Object.Lexer.lookahead (E:\work\nodeMongodb\node_modules\jade\lib\lexer.js:113:46)
   at Parser.lookahead (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:102:23)
   at Parser.peek (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:79:17)
   at Parser.tag (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:773:22)
   at Parser.parseTag (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:759:17)
   at Parser.parseExpr (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:211:21)
   at Parser.block (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:729:25)
   at Parser.tag (E:\work\nodeMongodb\node_modules\jade\lib\parser.js:838:24)
```
解决办法：上下级关系是要注意使用tab键或是换行，只能选一个
```
td: a(target="_blank",href="../movie/#{item._id}") 查看
td: a(target="_blank",href="../admin/update/#{item._id}") 修改
td: button.btn.btn-danger.del(type="button",data-id="#{item._id}") 删除
```

### mongodb模式模型设计及编码
1. Schema模式
在模式中对数据个字段进行定义，可以定义字段的类型。
```
var mongoose=require('mongoose');
var MovieSchema=new mongoose.Schema({
    director:String,
    title:String,
    language:String,
    country:String,
    year:Number,
    summary:String
});
```
2. Model模型
对传入的模式进行编译，会生成构造函数
```
var mongoose=require('mongoose');
var MovieSchema=require('../schemas/movie');
var Movie=mongoose.model(
    'Movie',
    MovieSchema
);
module.exports=Movie;
```
3. Documents文档
- Documents文档实例化
```
var Movie=require('./models/movie');
var movie=new Movie({
    title:'机械战警',
    director:'何塞·帕迪里亚',
    year:2018
});
//将该条数据存储在数据库中去
movie.save(function(err){
    if(err) return handleError(err)
});
```
- 数据库批量查询
```
var Movie=require('./models/movie');
app.get('/',function(req,res){
    Movie.find({})
         .exec(function(err,movies){
             res.render('index',{
                 title:'Jijmin首页',
                 movies:movies
             });
         });
});
```
- 数据库单条查询
```
var Movie=require('./models/movie');
app.get('/',function(req,res){
    Movie.findOne({_id:id})
         .exec(function(err,movies){
             res.render('index',{
                 title:'Jijmin首页',
                 movies:movies
             });
         });
});
```
- 单条数据删除
```
var Movie=require('./models/movie');
app.get('/',function(req,res){
    Movie.remove({_id:id},function(err,movie){
        if(err){
            console.log(err);
        }
    });
});
```

### 声明模式
1. 新建一个模式文件夹movie.js
2. 新建一个模型文件夹movie.js
3. 在模式的文件中引入建模工具mongoose
4. 声明一个模式，放和电影有关的字段级类型
5. 对时间的处理
```
meta:{
    // 录入数据或是更新的时候时间的记录
    createAt:{
        type:Date,
        default:Date.now()
    },
    //更新的时间
    updateAt:{
        type:Date,
        default:Date.now()
    }
  }
```
6. 为模式添加一个方法，每次在存储数据之前调用这个方法
```
MovieSchema.pre('save',function(next){
    // 判断数据是否是新加的
    if(this.isNew){
        // 将创建时间、更新时间设置为当前时间
        this.meta.createAt=this.meta.updateAt=Date.now();
    }else{
        //更新保存
        this.meta.updateAt=Date.now();
    }
    //继续执行存储流程
    next();
});
```
7. 静态方法与数据库进行交互
```
// 进行模型编译进行实例化了才会具有这个方法
MovieSchame.statics={
  //查询所有的数据
  fetch:function(cb){
    return this
      .find({})// 取出数据库中所有的数据
      .sort('meta.updateAt')//按照更新时间排序
      exec(cb);//执行这个回调方法
  },
  //查询单条的数据
  findById:function(id,cb){
    return this
      .find({_id:id})
      exec(cb);
  }
}
```
8. 将模式导出
```
module.exports=MovieSchame;
```
9. 模型文件movie.js
10. 加载mongoose这个工具模块
11. 加载我们写好的模式
12. 编译生成Movie这个模型
```
let Movie=mongoose.model('Movie',MovieSchema);
```
13. 将构造函数导出
```
module.exports=Movie;
```

### 编写数据库交互代码
1. 引入mongoose数据模块
```
const mongoose=require('mongoose');
```
2. 连接本地的数据库
```
mongoose.connect('mongodb://localhost/movie');
```
3. 首页进行一个查询功能
4. 直接调用fetch方法进行查询渲染
```
app.get('/',function(req,res){
    Movie.fetch(function(err,movies){
        if(err){
            console.log(err);
        }
        res.render('index',{
            title:'Jijmin 首页',
            movies:movies
        });
    });
});
```
5. 还要将Movie模块引入
```
const Movie=require('./models/movie');
```
6. 要根据ID值进行查询
```
app.get('/movie/:id',function(req,res){
    let id=req.params.id;
    Movie.findById(id,function(err,movie){
        res.render('detail',{
            title:'Jijmin '+movie.title,
            movie:movie
        });
    });
});
```
7. 调用在模式中定义好的静态方法
8. 我们需要设置一个路由，来解决表单提交过来后数据的存储
9. 因为是有很多字段，我们需要使用post方式拿到后台提交过来的数据
10. 有两种可能，一种是新增的，另一种是更新的
11. 通过判断拿到的数据中有id字段来确定是那种方式
12. 如果不是undefined，我们需要对数据进行更新
13. 先用id查找到对应的数据，要做一个容错处理
14. 用post过来的电影数据，里面更新过来的数据替换掉老的数据
15. 需要用到underscore模块中的extend来替换老的字段
```
const _=require('underscore');
```
16. 处理好后需要对数据进行保存，调用save方法，容错
17. 数据库更新后我们需要将页面重新进行渲染,redirect方法
18. 如果没有id值就是新添加的，就直接调用模型的额构造函数
```
app.post('/admin/movie/new',function(req,res){
    var id=req.body.movie._id;
    let movieObj=req.body.movie;
    let _movie;
    if(id!==undefined){//更新
        Movie.findById(id,function(err,movie){
            if(err){
                console.log(err);
            }
            _movie=_.extend(movie,movieObj);
            _movie.save(function(err,movie){
                if(err){
                    console.log(err);
                }
                // 重定向到详情页
                res.redirect('/movie/'+movie._id);
            });
        });
    }else{
        _movie=new Movie({
            director:movieObj.director,
        title:movieObj.title,
        language:movieObj.language,
        country:movieObj.country,
        year:movieObj.year,
        summary:movieObj.summary,
        flash:movieObj.flash,
        poster:movieObj.poster
        });
        _movie.save(function(err,movie){
            if(err){
                console.log(err);
            }
            // 重定向到详情页
            res.redirect('/movie/'+movie._id);
        });
    }
});
```
19. 对更新页面路由规则设置
20. 在表单域中要加入一个隐藏的id栏
21. 我们添加了moment对象，需要对moment进行调用
22. 海报地址
```
http://r3.ykimg.com/05160000530EEB63675839160D0B79D5
```
23. 片源地址
```
http://player.youku.com/player.php/sid/XNjA1Njc0NTUy/v.swf
```
