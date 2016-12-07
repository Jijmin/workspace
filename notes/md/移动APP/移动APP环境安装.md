### Node安装

### Git安装

### JDK安装
1. 安装JDK
2. 配置环境
3. 在环境变量中添加【JAVA_HOME】->【安装jdk的路径】
4. 【CLASSPATH】->【.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;】
5. 【PATH】->【;%JAVA_HOME%\bin;】
6. 在cmd中运行【javac】和【java -version】

### Android SDK
1. D:\Android\AndroidStudio
2. D:\Android\sdk
3. TOOLS->23.0.0.1
4. Android SDK Manager-->Android 6.0 ->Documentation.../SDK Platform/sources for Andriod SDK
5. 【ANDROID_HOME】->【自己的SDK路径】
6. 【PATH】->【;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools】
7. 【adb devices】【开发者选项】【USB调试】终端出现设备号就可以

### Python
1. C:\Python27\
2. Add python.exe to Path
3. Will be install....
4. 有可能不会将路径添加进环境中去，再想办法
5. 在cmd中【echo %path%】
6. 没有C:\Python27\;就要将路径加入
7. 【python --version】出现在版本号就是对的

### Visual Studio 2015
1. 如果不能打开的话转换成zip格式
2. 点击【vs_enterprise.exe】
3. 选择自定义
4. 选择功能中所有都不选
5. Visual C++勾选
6. 如果是Windows8需要进行更新才能安装

### 安装Ionic
- [Inoic](http://www.ionic.wang/start-index.html)
- 安装ionic/Install Ionic
```
$ npm install -g cordova ionic
```
- 【cordova -v】【ionic -v】
- 在myApp文件夹下有ionic.config.json
- 通过Ionic创建一个项目
```
$ ionic start myApp tabs
//一定要在你想要创建的地方输入这个命令
```
- 运行我们刚才创建的Ionic项目
```
$ cd myApp
$ ionic platform add android//会多一个platform文件夹，安卓所依赖的东西
$ ionic build android//将www目录打包到platform
//很可能丢文件，删掉重来
```
- 【adb devices】出现了版本号就可以【$ ionic run android】可以直接安装在手机上
### bower下载包在lib文件下
在GitHub上下载东西，可以修改路径
```
npm i -g bower
bower install jquery
```
[bower](https://bower.io)

### 简历更改
1. 排版改成统一的
2. 版心要改小点
3. 不重要的信息往后放
4. 第一个项目要写详细点，然后就是要写一些较好的知识点上去，单页面应用什么的

