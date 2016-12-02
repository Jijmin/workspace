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

```

### 利用MUI这个前端框架来实现移动站点

## 利用HBuilder这个开发工具的云服务将移动站点打包成apk