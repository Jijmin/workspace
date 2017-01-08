### 本机host文件
【C盘】【Windows】【System32】【drivers】【etc】【hosts】

### GitLab配置
1. 在hosts文件中配置一行
```
192.168.141.95    www.study.com
```
2. 如果受权限影响，应该配置一个权限
【hosts】【右键】【属性】【安全】【编辑】【找到自己的用户】【完全控制】勾上
3. 还有一种更简单粗暴的方式，直接复制出来，更改再复制回去
4. gitlab配置
```
192.168.141.95    gitlab.study.com
```

### 塌陷问题的解决办法
1. 将margin换成padding
2. 给border值加上透明度

### 解决文字和小图标没有对齐的问题
1. 文字有默认的行高
2. 将文字的行高设置为1

### 设置文字居中
1. 给文字设置一个字体大小
2. 将文字的行高设置为1
3. 设置padding的上下值

### 解决CSS样式冲突的办法
1. 在文件下面写一个.content，每个人规定一个命名空间，再在对应的命名空间下写样式
2. 每个人写单独的样式文件，将公共的import进来就行

### Express的安装
1. `npm install -y`
2. `npm install express --save`
3. node-modules文件夹要在项目目录下，--save才能更新到配置文件中

# 管理系统部署
## 前端页面搭建
## 后台框架搭建
### 后台文件夹
- studyit
    + controllers
        * index.js
    + models
    + node_modules
    + public
        * images
        * js
        * less
    + uploads
    + views
        * adverts广告
        * courses课程
        * dashboard仪表盘
        * teachers教师
        * users用户
        * layout布局
            - base.xtpl
    + app.js
    + package.json

## 测试用例
### app.js
1. 引入express包
2. 实例一个express对象
3. 监听端口
4. 我们需要有很多模块来完成功能
5. 需要搭建路由
6. 在controllers新添一个控制器index.js
7. 将我们index.js引入，建立连接
8. 对路由规则进行使用
9. 设置模板路径
```
app.set('views',__dirname+'/views');
```
10. 设置模板引擎
```
app.set('view engine','xtpl');
```

### index.js
1. 导入express包
2. 设置一个路由对象
3. 配置路由规则
4. 将路由对象暴露出去
5. 当我们使用send或是end显示内容有限
6. 要使用render来对页面进行渲染
7. 要使用模板引擎来实现xtpl
8. 进行渲染的时候可以通过{}进行参数设置
```
res.render('index',{name:'Jijmin'});
```
9. 在模板这种要使用参数时用{{}}进行渲染

## 主页路由设置
1. 更改测试用例中的路由的路径，找到仪表盘中的首页
2. 将原来的文件的.html文件更改为.xtpl文件，如果没有高亮显示，可以将语法格式转变为html的格式，就会有高亮
3. 页面加载成功，但是样式没有加载成功
4. 设置静态目录
```
app.use('/',express.static('public'));
app.use('/',express.static('uploads'));
```
5. 更改样式路径地址
6. 静态资源的库放在assets文件夹下
7. 先将公共部分抽离，再对其他部分进行部署
8. 新建一个模板布局，将公共的写入进去
9. 可以将主页除了公共部分的写在一个文件下，再将公共部分集成过来
10. 个人设置时，在a标签中加入跳转目录
11. 再进行路由设置

### 包管理
1. express
2. xtpl
3. xtemplate