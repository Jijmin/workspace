## SQL语句的语法
1. 增加
-> 语法：insert into 表名称(字段1，字段2,...) values (字段1的值，字段2的值,...)
-> 举例：`insert into userinfo(uname,upwd,ustatus) values('test1','123',0)`
2. 删除
-> 语法： delete from 表名称（表示将表中的所有数据删除,这是很危险的）
-> 举例：`delete from userinfo where uname='zy'`(推荐写法)
3. 修改
-> 语法： update 表名称 set 字段1=更新的值，字段2=更新的值,...（更新所有数据的这些字段）
-> 举例：`update userinfo set upwd='123' where uname='zy'`
4. 查询
-> 语法： 
- select * from 表名称   -- 查询表中的所有字段
- select 字段名称,字段2的名称,..... from 表名称    --查询表中的指定字段
- select * from 表名称 limit 跳过多少条数据,拿到多少条数据  ->分页的sql语句写法
- select * from 表名称 where 字段的值 = '条件值'   -> 带条件查找 (相等)
- select * from 表名称 where 字段的值 like '%条件值%'   -> 带条件查找 - （只要字段的值的任何位置包含有条件之即可查出）
- select * from 表名称 where 字段的值 like '条件值%'   -> 带条件查找 - （模糊查询,表示条件值是字段值的前缀）
- select * from 表名称 where 字段的值 like '%条件值'   -> 带条件查找 - （模糊查询,表示条件值是字段值的后缀）
- 满足大于，小于，大于等于，小于等于某个条件值的写法
- select * from 表名称 where 字段的值 > 条件值
- select * from 表名称 where 字段的值 >= 条件值
- select * from 表名称 where 字段的值 < 条件值
- select * from 表名称 where 字段的值 <= 条件值
- 想要查找一个表中主键为1或者为2的值 (包含)
- select * from 表名称 where 主键值 in (1,2)
- select * from 表名称 where 主键值 =1 or 主键值 =2
- 想要找到一个表中同时有两个字段满足要求的数据
- select * from 表名称 where 字段值1='条件值' and 字段值2 = '条件值2' and .....
5. 其他
- `use abc` :表示在执行sql语句时候切换到abc数据库
- `select count(*) from 表名称`统计一个表中的数据总条数
6. 连表查询
- 内联
  select t1.* from 表名称1 as t1 ,表名称2 as t2 where t1.外键值 = t2.主键值
- 左链接
  select * from 表名称1 as t1
  left join 表名称2 as t2 
  on (t1.外键 = t2.主键)
- 右连接
 select * from 表名称1 as t1
  right join 表名称2 as t2 
  on (t1.外键 = t2.主键)
7. 删除一个表
drop table 表名称  (危险做法，不要去做)

## orm+MySQL
```
在orm中可以利用：
db.driver.execQuery("带有业务的sql语句", function (err, data) { 
  // data:参数的值：
  //1.0 如果是select语句则这个data就是查询回来的数据数组对象
  //2.0 如果是非select语句，那么这个data就是数据库的提示信息对象

})
```

## 实现移动站点播放视频
### 利用nodejs开启后台的API
1. 用户手机上输入一个网址的时候，先访问的是我们的一个移动站点
2. 移动站点是我们实现视频的播放功能的，是运行在服务器上的
3. 我们现在是用nodejs的express来托管的
4. 上面放置的是我们的一些纯静态的页面
5. 在项目中我们应该做到前后端分离，方便维护
6. 我们在移动站点上应该通过ajax请求去后端人员帮我们开发的API服务器上调用接口
7. 在我们的API服务器上存储了很多的对数据的操作，通过我们的orm+mysql之类的形式调用数据库的数据
8. 很大一部分得到情况下，我们是值用负责移动站点的开发，我们只需要用ajax调用我们后端人员的API
9. 我们需要和后端人员定义好API接口的相关参数，这个是后端开发人员定的
10. 这其中有很多规范，需要我们相互协商好
- API地址是什么
- API的请求方式是get还是post还是两者兼有
- API的参数有哪些，分别代表什么意义
- 响应回来的数据格式是什么json还是xml
- 返回来的数据格式中的每个字段是什么意义
- 在附录中也要记录我们的定义好的成功和失败的表示形式
11. 如果是我们自己开发后台的功能，我们需要根据一个原型图来设计
12. 如果没有原型图我们需要有一个模板来开发

### API模板
1. 地址
2. 作用描述
3. 传入API的参数形式
4. 返回的数据格式
5. 返回数据格式样例

### orm+mysql实现API服务器
1. 创建一个服务器
```
'use strict';
const express=require('express');
let app=express();
app.listen(8899,'127.0.0.1',()=>{
  console.log('API服务已启动,127.0.0.1:8899');
});
```
2. 引入我们的orm包去操作我们的数据库里面的数据
```
const orm=require('orm');
app.use(orm.express('mysql://root:root@127.0.0.1:3306/nodedb',{
  define:function(db,models,next){
    next();
  }
}));
```
3. 开启我们的路由，将我们路由暴露出去
```
'use strict';
const express=require('express');
const apiCtrl=require('../controllers/apiCtrl');
let route=express.Router();
module.exports=route;
```
4. 设定首页的ajax请求的路由规则
```
const apiRoute=require('./routes/apiRoute.js');
app.use('/',apiRoute);
```
5. 设定视频播放页面的ajax请求的路由规则/api/getlist/
6. 获取首页的数据，数据中的参数，一个是我们的状态值，还有一个是消息值
```
"use strict";
let successState=0;
let fialState=1;
```
7. 定义一个接收响应回来的数据的状态值，一般我们定义一个变量存储我们表示成功的状态值(0)，还要定义一个表示失败的状态值(1)，就可以用这个值来代替我们队状态的标识
```
let resObj={status:successState,message:''};
```
8. 我们获取首页的数据要实现分页
9. 定义好我们的SQL语句
10. 先要获取传入的pageIndex的值，就是页码
11. 确定页容量
12. 计算要跳过的条数
13. 利用orm发送sql语句查询出来分页的数据即可
14. 我们需要判断是否有异常，如果有就要改变状态值，以及还要有错误提示消息
```
exports.getlist=(req,res)=>{
  //先要获取传入的pageIndex参数
  let pageIndex=req.query.PageIndex || 1;
  let pageSize=10;
  //计算要跳过的条数
  let skipRow=(pageIndex-1)*pageSize;
  //查询出分页数据
  let sql='select vid,vtitle,vsummary,vimg form videoinfo limit '+skipRow+','+pageSize;
  req.db.dirver.execQuery(sql,(err,datas)=>{
    if(err){
      resObj.status=fialState;
      resObj.message=err.message;
      res.end(JSON.stringify(resObj));
      return;
    }
    //获取数据成功
    resObj.message=err.message;
    res.end(JSON.stringify(resObj));
  });
}
```
15. `{"status":1,"message":"ER_PARSE_ERROR: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'videoinfo limit 0,10' at line 1"}`解决办？
16. SQL语句写错了，是from，不是form，获取数据也写错了，直接将获取到的datas数据传给message`resObj.message=datas;`
17. 设定视频播放页面的ajax请求的路由规则
```
route.get('/api/getvideo/',apiCtrl.getvideo);
```
18. 获取指定vid的视频详情和视频播放id
```
exports.getvideo=(req,res)=>{
  let resObj={status:successState,message:''};
  let vid=req.query.vid;
  let sql='select vvideid,vremark from videoinfo where vid='+vid; 
  req.db.driver.execQuery(sql,(err,datas)=>{
    //判断是否异常
    if(err){
      resObj.status=fialState;
      resObj.message=err.message;
      res.end(JSON.stringify(resObj));
      return;
    }
    //获取数据成功
    resObj.message=datas;
    res.end(JSON.stringify(resObj));
  });
}
```

### 利用MUI这个前端框架来实现移动站点
1. 下载HBuilder
2. 创建一个MUIAPP文件夹，这个就是我们将来在手机上运行的东西
3. 新建一个移动APP
4. 选择MUI项目
5. 可以对其进行一些系统项的配置
6.  MUI开发规则
- 除了固定栏以外，一切内容都要包裹在mui_content中
- 页面初始化必须执行mui.init方法
7. 头部固定栏我们直接在hello MUI 上拷贝过来
8. 后面的内容放入我们的mui-content
9. 搜索框以及图文列表
10. 我们图片是从后台获取的，为了减少服务器的压力，我们将图片存放在七牛上
11. 将我们的图片的外链地址更改在我们的图片位置
12. 我们可以使用模板将我们的视频列表输出
13. 使用underscore.js模板工具
```
<script type="text/template" id="tpl">
    <% for(var i = 0; i < message.length; i++) { %> 
    <% var item = message[i] %>     
    <li class="mui-table-view-cell mui-media">
        <a class="mui-navigate-right">
            <img class="mui-media-object mui-pull-left" src="<%=item.vimg%>">
            <div class="mui-media-body">
                <%=item.vtitle%>
                <p class="mui-ellipsis"><%=item.vsummary%></p>
            </div>
        </a>
    </li>
    <% } %>
</script>
```
14. 利用MUI的ajax方法去API服务器请求数据结合模板生成数据即可
```
function getlist(){
    var url='http://127.0.0.1:8899/api/getlist?pageIndex=1';
    //接收JSON数据，已经传了参数，第二个参数是null，第三个参数是一个成功的回调函数
    mui.getJSON(url,null,function(jsObj){
        if(jsObj.status!=0){
            alert(jsObj.message);
            return;
        }
        //结合模板进行数据渲染
        //1.0获取到模板字符串的对象
        var tplStr=document.querySelector('#tpl').innerHTML;
        //2.0调用underscore中的template实例化模板对象，解析模板，返回内容，将结果填充页面中
        document.querySelector('#list').innerHTML=_.template(tplStr, jsObj);
    });
}
getlist();
```
15. 在我们APP开发中，若要使用HTML5+扩展API，必须等到plusready事件发生后才能正常使用，否则可能会报错，但是这个方法在浏览器是不支持的。
```
mui.plusReady(function(){
    getlist();
});
```
16. 出现了一个跨域问题
```
XMLHttpRequest cannot load http://127.0.0.1:8899/api/getlist?pageIndex=1. Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://127.0.0.1:8020' is therefore not allowed access.
```
17. 解决办法
- jsonp方式去访问
- 只需要在NodeJS服务器中添加几个响应报文即可完成
```
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});
```
18. 没有报错，显示字符串信息
```
function (n){return o.call(this,n,m)}
```
19. 通过这种方式我们发送了两次请求
20. 浏览器对服务器的跨域设置是有一些不兼容性的，如果使用mui中的getJSON()方法对于res.header("Access-Control-Allow-Headers", "X-Requested-With");是不兼容的，会发出两次请求
21. 我们只能更改使用的模板，使用jquery.tmpl.js模板
```
<script src="js/jquery.min.js"></script>
<script src="js/jquery.tmpl.min.js"></script>
```
```
<script type="text/x-jquery-tmpl" id="tpl">
    {{each(i,item) message}}     
    <li class="mui-table-view-cell mui-media">
        <a class="mui-navigate-right">
            <img class="mui-media-object mui-pull-left" src="${item.vimg}">
            <div class="mui-media-body">
                ${item.vtitle}
                <p class="mui-ellipsis">${item.vsummary}</p>
            </div>
        </a>
    </li>
    {{/each}}
</script>
```
```
function getlist(){
    var url='http://127.0.0.1:8899/api/getlist?pageIndex=1';
    mui.getJSON(url,null,function(jsObj){
        if(jsObj.status!=0){
            alert(jsObj.message);
            return;
        }
        var resHtml=$('#tpl').tmpl(jsObj);
        $('#list').html(resHtml);
    });
}
$(function(){
    getlist();
});
```
22. 我们点击页面上的列表我们就需要跳转到播放页面，在我们的HBuilder中新建一个含有mui的html
23. 头部是一个有返回主页面的头部，中间视频简介区域我们需要自己手动设置
24. 视频播放区域我们只需要留有一个div的位置，到时候通过腾讯的JS插件生成出来
25. 下面的是一个tab选项卡，课程介绍和章节管理
26. 因为我们每点击一次列表就会进入到我们对应的视频播放区域
```
<a href="/videoApp/play.html?vid=${item.vid}" class="mui-navigate-right">
```
27. 实现了我们从视频列表页跳转到详情页，那么我们还需要将视频显示出来，以及对应的详情信息
28. 出现问题
- 问题描述：jquery.min.js:4 GET http://127.0.0.1:8899/api/getvideo/vid=16 404 (Not Found)
- 解决办法：$.getJSON('http://127.0.0.1:8899/api/getvideo?vid='+vid,null,function(data){
- 将参数前面的?写错了
- 但是没有数据显示出来
- 我们的message是一个数组，加上索引
- $("#item1").html(data.message[0].vremark);

### 后台管理系统所有的静态资源功能不能实现
将我们设置静态资源放在设置路由规则之前

### 视频添加
1. 将我们在腾讯视频上上传的视频打开
2. url地址栏就有我们的id编号
3. 复制过来保存到我们的数据库
4. [腾讯视频播放插件](http://www.cnblogs.com/zgzy/p/4396366.html)
5. 将腾讯提供给我们的tvp.player.js下载下来，放到我们的文件夹下
6. 引入腾讯视频播放器组件
```
<script src="js/tvp.player.js"></script>
```
7. 对我们的插件的参数进行设置
```
function setVideo(vid){
  //定义视频对象
  var video = new tvp.VideoInfo();
  //向视频对象传入视频vid
  video.setVid(vid);//替换成自己的视频id
  //定义播放器对象
  var player = new tvp.Player('100%', 240);
  //设置播放器初始化时加载的视频
  player.setCurVideo(video);
  //设置精简皮肤，仅点播有效
  //player.addParam("flashskin", "http://imgcache.qq.com/minivideo_v1/vd/res/skins/TencentPlayerMiniSkin.swf");
  //输出播放器,参数就是上面div的id，希望输出到哪个HTML元素里，就写哪个元素的id
  player.addParam("autoplay","1");
  player.addParam("wmode","transparent");
  //player.addParam("pic","http://img1.gtimg.com/ent/pics/hv1/75/182/1238/80547435.jpg");
  player.write("video");//将视频输出到id=video的div中
}
```
8. 设置当前视频的id并进行播放
```
setVideo(data.message[0].vvideid);
```

### 打包之前的设置
1. 这个手机端播放视频我们也是需要通过gulp进行管理
2. 也需要通过一个app.js文件使用express托管
```
'use strict';
const express=require('express');
let app=express();
app.listen(8888,()=>{
  console.log('video site started 8888');
});

```
3. 我们需要将我们这个App进行托管，都是我们的静态资源
4. 将当前目录变成我们express的静态资源目录即可
```
app.use(express.static(__dirname));
```

### 移动站点和移动APP
1. 移动站点：
- 一般是类似于m.jd.com m.taobao.com m.itcast.cn
- 在浏览器中打开，那么这种网站一定要在服务器上进行托管允许(目前使用的是express)
2. 移动APP：
- 这种就是类似于原生的APP，直接打包安装到手机上，将来手机就会自动利用HBuilder的基座打开一个内置的浏览器，自动找到名称为index.html的文件运行即可

## 利用HBuilder这个开发工具的云服务将移动站点打包成apk
1. 在HBuilder中我们提供了一个云，可以发行为原生的安装包，一定要选中我们要发行的安装包
2. 进行选择的打包，以及选择是否去配置我们需要额模块与权限

### 打包注意点
1. 要将我们的url地址改为外网地址，不是我们测试的本地

### 解决移动端搜索框用不了的办法
1. 将后面的span标签去掉
2. 以及span里面的data-input-clear="1" data-input-search="1"一起去掉

## 学习微信的开发
1. 注册号微信公众平台以后，利用它的后台直接发布消息(不需要开发经验)
2. 学习微信的API的使用
- 订阅后自动发送消息到手机上
- 当用户发送一条消息后我们的服务器主动回复

### 苹果投影
1. 先装iTunes
2. 再装iTools
3. 在iTools中选择我们苹果录屏大师，选择操作系统
4. 第一次使用选择连接助手，要使用屏幕投影助手扫描二维码
5. 要在手机的商店中下载屏幕投影助手
6. 扫描后会自动在手机上装一个iTools
7. 如果有连接上的话就是AirPlay
8. 选择iTools并将镜像打开

### 订阅后自动发送消息到手机上
- 在我们的微信公众平台上有一个自动回复选项，在里面输入我们文字。
- 关注了微信公众号后，自动收到一条欢迎消息
- 我们微信订阅号订阅成功后会建立起一条通道
- 微信服务器就会通过自动回复中的被添加自动回复功能主动给你发送一条消息
- 当我们的用户发送一条消息的时候是发送给微信服务器的
- 微信服务器要回复一条消息，微信公众平台中我们需要添加关键词自动回复
- 用户输入规则名可以获取到我们关键词自动回复的消息

### 企业自己的系统服务器
1. 开启一个web服务器用来接收请求
2. 当用户发送信息的时候不是通过微信发送
3. 根据系统自己的逻辑做一些响应回去
4. 我们的服务器响应给微信服务器
5. 微信再响应回用户界面
6. 在微信公众平台上有很多接口

### 自己开发一个服务器来接管我们的微信公众号
1. 关闭微信服务器本身的消息响应功能
2. 还可以在【开发】里面的【基本配置】成为开发者
3. 【服务器配置】【启用】就会自动关闭
4. 当所有的资料补全以后进入到【基本配置菜单】【修改配置】
5. 【URL】代表我们自己开发的微信服务器的对外的地址
- 作用：用来给微信服务器想我们自己开发的web服务器上进行消息推送的
- 这个地址要想填写成功，必须要写代码验证
6. 在开发过程中使用明文，方便调试
7. 用express搭建我们的服务器
8. 要将我们的服务器变成一个URL，借助于ittun
9. 在【startup-api接口.bat】中更改我们的域名以及端口号
10. 定一个与微信服务器进行交互的路由规则
11. 将申请服务器以及搭建服务写好之后回到基本配置中的URL中
12. 将我们自己开发好的url填充到微信公众平台的修改配置中的url以后，微信服务器就会自动发送一条请求到达我们的服务器
13. 这时由于我们的服务器没有给微信服务器响应一个它要求的合法的字符串，所以微信服务器此时认为我们填写的url是一个不合法的url拒绝通信
14. 我们应该在我们自己的服务器中按照微信的通讯标准来进行响应合法的字符串
15. signature这个参数的值就是echostr+timestamp+nonce进行加密以后的密文
16. echostr微信服务器向我们自己开发的服务器推送的一个随机字符串，将来只需要在我们的服务器中将这个字符串原样响应给微信服务器即可这时微信服务器只要获取到这个字符串与它内部生产的比较如果相等就表示我们的服务器url验证成功
```
let echostr=req.query.echostr;
res.end(echostr);
```
17. 我们在做验证的时候，只需要在我们的服务器中将微信服务器发送过来的echostr的值响应即可完成
18. 在我们的服务器中只需要将echostr返回即可，因为在url的参数形式是？要使用query获取
19. 验证成功以后，将来微信服务器和我们的服务器的通讯地址就是url

### 开发功能
1. 关注/取消关注事件
```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>//开发者微信号
<FromUserName><![CDATA[FromUser]]></FromUserName>//发送方帐号（一个OpenID）
<CreateTime>123456789</CreateTime>//消息创建时间 （整型）
<MsgType><![CDATA[event]]></MsgType>//消息类型，event
<Event><![CDATA[subscribe]]></Event>//事件类型，subscribe(订阅)、unsubscribe(取消订阅)
</xml>
```
- 解析xml(使用xml2.js)
- 分析里面的MsType是什么类型event
- event节点中一定是subscribe
- 才表示用户是关注这个公众号的事件

### 关注/取消关注事件开发
1. 当用户关注了公众号的时候，我们要响应回去
2. 解析微信推送给我们的服务器的xml数据(post)
3. 获取到post提交过来的请求报文体中的数据
```
req.on('data',(requestBody)=>{
  console.log(requestBody.toString());
});
```
4. 将所有发送过来的xml数据转换成JS对象
```
var parseString = require('xml2js').parseString;
var xml = "<root>Hello xml2js!</root>"
parseString(xml, function (err, result) {
    console.dir(result);
});
```
5. 再根据JS对象中的相关的字段去判断到底是关注还是其他
```
req.on('data',(requestBody)=>{
    var parseString = require('xml2js').parseString;
    var xml = requestBody.toString();
    parseString(xml, function (err, result) {
      if(result.xml.MsgType[0]=='event'&&result.xml.Event[0]=='subscribe'){
        //关注事件
        console.log('enter...');
      }else{

      }
    });
  });
```
6. 将欢迎消息响应回微信服务器
```
let resXML=`<xml>
            <ToUserName><![CDATA[${result.xml.FromUserName}]]></ToUserName>
            <FromUserName><![CDATA[${result.xml.ToUserName}]]></FromUserName>
            <CreateTime>12345678</CreateTime>
            <MsgType><![CDATA[text]]></MsgType>
            <Content><![CDATA[你好，欢迎关注！！]]></Content>
            </xml>`;
res.end(resXML);
```
7. 对微信客户端和服务器端进行通讯处理

###

### xml2.js使用
1. 将xml转换成对象
```
var parseString = require('xml2js').parseString;
var xml = "<root>Hello xml2js!</root>"
parseString(xml, function (err, result) {
    console.dir(result);
});
```

### 被动回复用户消息
#### 用户关注回发送消息
```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>//接收方帐号（收到的OpenID）
<FromUserName><![CDATA[fromUser]]></FromUserName>//开发者微信号
<CreateTime>12345678</CreateTime>//消息创建时间 （整型）
<MsgType><![CDATA[text]]></MsgType>//text
<Content><![CDATA[你好!!!]]></Content>//回复的消息内容（换行：在content中能够换行，微信客户端就支持换行显示）
</xml>
```
#### 用户随便输入回复消息
```
if(result.xml.MsgType[0]=='text'){
  let resXML=`<xml>
              <ToUserName><![CDATA[${result.xml.FromUserName}]]></ToUserName>
              <FromUserName><![CDATA[${result.xml.ToUserName}]]></FromUserName>
              <CreateTime>12345678</CreateTime>
              <MsgType><![CDATA[text]]></MsgType>
              <Content><![CDATA[你好]]></Content>
              </xml>`;
  res.end(resXML);
}
```
#### 回复图文消息
```
let resXML=`<xml>
            <ToUserName><![CDATA[${result.xml.FromUserName}]]></ToUserName>
            <FromUserName><![CDATA[${result.xml.ToUserName}]]></FromUserName>
            <CreateTime>12345678</CreateTime>
            <MsgType><![CDATA[news]]></MsgType>
            <ArticleCount>1</ArticleCount>
            <Articles>
            <item>
            <Title><![CDATA[欢迎学习nodejs课程]]></Title> 
            <Description><![CDATA[这次学习的是NodeJS项目]]></Description>
            <PicUrl><![CDATA[https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png]]></PicUrl>
            <Url><![CDATA[https://www.baidu.com/]]></Url>
            </item>
            </Articles>
            </xml>`;
res.end(resXML);
```

### 这个流程的使用
1. 使用ittun创建3个外网连接的地址
- api接口
- 移动站点MUI
- 运营者服务器
2. 因为我们的app是托管到MUI上面，我们要将移动站点MUI的ittun配置成mui的端口
3. 开启我们的移动站点MUI的服务器，就是配置文件
4. 要取到数据需要我们的APIService启动，就是运行app.js
5. 通过MUI将我们的手机端页面打开就可以看到数据
6. 将我们映射到外网的app地址填入我们对微信关注公众号处理中
7. 在我们的运营者服务器处理关注事件中调用我们对xml文件的处理函数
8. 开启我们的运营者服务器
9. 我们需要对关键字进行开发
10. 获取到搜索的关键字
11. 去表中查询数据
12. 包装成图文消息的xml格式
13. 响应回去
14. 需要对我们响应回去的xml进行连接数据库的操作
15. 获取到数据的总条数
16. 遍历我们定义好的item格式生成一个长串的xml格式
17. 将我们的后台数据填充到xml文件中
18. 最后响应回去

### 整个项目的展示图
![架构设计](../images/03 - 系统回顾和微信开发的架构设计.png)

