### Bower : ENOGIT git is not installed or not in the PATH 解决方法
- 解决方法一：
    + 添加git到window的环境变量中。设置path路径为D:\Program Files (x86)\Git\bin
- 解决方法二：
    + 在不用添加环境变量的情况下，运行下面一个命令，就可以设置当前文件加的环境变量。
    ```
    $ set PATH=%PATH%;C:\Program Files\Git\bin
    ```
    + 运行完了上面的命令，然后你再用bower安装依赖就成功了，不会有Bower : ENOGIT git is not installed or not in the PATH 这个报错了！