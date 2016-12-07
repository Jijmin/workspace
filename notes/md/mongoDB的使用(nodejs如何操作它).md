### mongoDB的使用(nodejs如何操作它)
#### 关系型数据：mysql，oracle，sqlServer
#### 非关系型数据： mongoDB(文档型数据库)，redis(缓存)
#### 组成mongoDB的几种软件
1. mongoDB的服务软件
- mongodb-win32-i386-2.4.3
- 在bin目录下我们有一个批处理文件
- mongod --dbpath data
- 上面的是一个相对目录，在我们批处理的文件夹下有一个data文件夹
- 里面有【_tmp】【local.0】【local.ns】【mongod.lock】
- 运行我们的批处理操作，如果出现我们的cmd一闪而过，将上面的mongod.lock文件删除，再次打开就可以
- mongodb的服务器监听端口是：27017
2. mongoDB的客户端软件
- MongodbVUE -Installer-1.5.3
- 安装Installer.msi
- 添加一个连接
- 可以新建数据库，但是没有新建表
- Collections下面的startup_log才是一个表
- 直接通过模型就可以建表
3. nodejs中是通过mongoose这个包进行数据库的访问
- 安装`npm install mongoose --save`
- 根据文档去操作数据库[mongoose](https://www.npmjs.com/package/mongoose)
- 进行代码编写
  + 导包
  ```
  const mongoose=require('mongoose');
  ```
  + 定义一个数据库连接字符串`mongodb://数据库的服务器ip/你的数据库名称`
  ```
  let dbConstr='mongodb://127.0.0.1:27017/test'
  ```
  + 通过connect()连接
  ```
  mongoose.connect(dbConstr);
  ```
  + 定义表结构`mongoose.Schema()`
  ```
  var Schema=mongoose.Schema,
      ObjectId=Schema.ObjectId;//代表自动生成的主键
  var userinfoSchema=new Schema({//表结构的构建
    uid:ObjectId,
    uname:String,
    upwd:String
  });
  ```
  + 通过`mongoose.model()`定义模型
  ```
  let userInfoModel=mongoose.model('userinfo',userinfoSchema);//表名称，表结构
  ```
  + 通过模型中的方法操作数据库
    * find()//查询
    * create()//增加
    * update()//更新
    * remove()//删除
  ```
  userInfoModel.create({uid:'1',uname:'admin',upwd:'123'},(err,item)=>{
    console.log('插入成功');
  });//表结构的JS对象，回调函数
  ```
