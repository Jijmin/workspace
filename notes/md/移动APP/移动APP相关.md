### 混合开发简介
+ 现在app开发方类型有几种？
    * WebApp:就是在浏览器中运行的web应用
    * NativeApp:用android和Object-C等原生语言开发的应用
    * HybridApp:就是外面是原生的壳，里面是webapp应用，兼具2者的优势
+ 企业如何选择用那种方式开发产品呢？
    * 企业要根据自身的特点进行app开发方式的选择，不要为了混合而混合。
    * 混合app的开发方式是未来的主流趋势。
+ 移动端开发有哪些框架
    * 主流的：Ionic、React Native
    * 非主流：Html5+、AppCan、Jquery Mobile（主要用于做移动Web）

### HTML5+
- [官网地址](http://www.dcloud.io/)
- [Emmet语法](http://docs.emmet.io/cheat-sheet/)
- [案例](http://www.dcloud.io/case/#group-1)
- Dclout4个核心产品
    + HBuilder：是一款web开发工具。它能大幅提升开发效率，对程序员也设计了更人文关怀的UI，它包括最全面的语法库和浏览器兼容性数据。到现在 已经有两年的时间了，到现在大概有几十万的开发者在使用。
    + 5+Runtime：是一个H5的增强引擎。这是一个可以去调用H5的能力和原生操作系统的产品。  
    + MUI框架：是高性能的前端框架，可用开发高性能App，也是目前最接近原生App效果的框架，可以有效解决HTML5原生开发中遇到的部分问题，同时体积也小。
    + 流应用：会让用户感觉一点开就可以用，这也是颠覆现有的应用方式的主要因素，给最终用户去使用应用有更好的体验。
      * 如果手机上安装用360手机市场的话，html5+开发的移动扫一下二维码,只需要一点点流量就能够使用了。
- Html5+的优点
    + 提供了一个自定制的IDE工具，支持非常好，轻量级，甚至不需要安装直接解压缩就可以使用
    + 开发出来的app的性能很好，用户体验很高
    +  丰富的api支持，号称有40万+ 
- Html5+的缺点
    + 打包比较麻烦，需要上传源码，有些公司可以不希望公开源码
    + 从新学习样式

### AppCan
-   [官网地址](http://www.appcan.cn/)
-   首先来认识AppCan是什么？
>AppCan.cn开发平台是基于HTML5技术的跨平台移动应用快速开发一体化解决方案。开发者利用HTML5+CSS3+JavaScript技术可 以快速地开发与本地应用体验相媲美的移动应用。AppCan.cn平台提供了UI快速开发框架、本地功能调用API接口、应用打包系统、IDE集成开发环 境和本地应用调试模拟器，预置数百套界面模板和数十种应用插件，提供多套应用模板。完善的框架接口，人性化的开发环境，丰富的开发资源，强大的服务支持， 使开发者可以快速迈入移动开发领域。

- [项目演示](http://www.appcan.cn/appshow/index.html)

### Jquery Mobile
- 简介
    + jQuery Mobile 是一个用于创建移动端web应用的的前端框架。
- 网站
[官网地址](http://jquerymobile.com/)
[中文网地址](http://jquerymobile.weebly.com/jquerymobile3245220214.html)
[APi学习地址](http://www.w3school.com.cn/jquerymobile/index.asp)

-   Jquery Mobile是做什么的？
jQuery Mobile 是针对触屏智能手机与平板电脑的网页开发框架。
jQuery Mobile 工作与所有主流的智能手机和平板电脑上： 
jQuery Mobile 构建于 jQuery 以及 jQuery UI类库之上，如果您了解 jQuery，您可以很容易的学习 jQuery Mobile。
jQuery Mobile 使用了极少的 HTML5、CSS3、JavaScript 和 AJAX 脚本代码来完成页面的布局渲染。

### Ionic介绍*
= 官网地址
  - [Ionic官网](http://ionicframework.com/)
  - [Ionic中文网](http://www.ionic.wang/)

- 首先要认识Ionic框架？
> IONIC 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。它使用 JavaScript MVVM框架和AngularJS来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。Ionic是一个专注于用WEB开发技术，基于HTML5创建类似于手机平台原生应用的一个开发框架。Ionic框架的目的是从web的角度开发手机应用，基于PhoneGap的编译平台，可以实现编译成各个平台的应用程序。
- ionic框架构成
    + CSS框架 - 提供原生 App 质感的 CSS 样式模拟。 ionic 这部分的实现使用了 ionicons 图标样式库。
    + JavaScript框架 - 提供移动 Web 应用开发框架。ionic 基于 AngularJS 基础框架开发，遵循 AngularJS 的框架约束；此外 ionic 使用 AngularJS UI Router 实现前端路由。
    + 命令行/CLI - 命令行工具集用来简化应用的开发、构造和仿真运行。ionic 命令行工具使用了 Cordova，依赖于平台 SDK（Android & iOS）实现将移动 Web 项目打包成原生 App。
    -  Ionic特点
        + 基于Angular语法，简单易学。
        + 是一个轻量级框架。性能优越，运行速度快。
        + 完美的融合下一代移动框架，支持 Angularjs 的特性， MVC ，代码易维护。
        + 提供了漂亮的设计，通过SASS构建应用程序，它提供了很多UI组件来帮助开发者开发强大的应用。
        + 专注原生，让你看不出混合应用和原生的区别
        + 提供了强大的命令行工具。
    - ionic开发注意点有哪些？
    > 由于 ionic 使用了 HTML5 和 CSS3 的一些新规范，所以要求 iOS7+ / Android4.1+。 在低于这些版本的手机上使用 ionic 开发的应用，有时会发生莫名其妙的问题。
- 现在那些公司再用ionic开发？
  - [查看ionic开发的app的地址](http://www.phonegap100.com/app.html)

### Ionic框架主要技术介绍
- Cordova介绍
   +  [官网地址](https://cordova.apache.org/)
- Cordova是什么？
    + Cordova提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。 
    + Cordova支持如下移动操作系统：iOS, Android,ubuntu phone os, Blackberry, Windows Phone, Palm WebOS, Bada 和 Symbian。 
- Cordova和phonegap的关系？
    + 08年一次ios开发者大会上来自Nitobi软件公司的几个家伙突发奇想，提出一个想法，想做一个工具来弥补web和ios开发之间的不足，并提出 Bridging the gap between the web and the iPhone sdk。一开始的目标并不是很大，但是做到了现在的written once,run everywhere。
      09年他们推出android adk和blackberry sdk，成了移动开发者的福音，就连ibm也加入进来。phonegap继续成长，在2011年10月，整个Nitobi团队被adobe收购，PhoneGap的项目主管Brian LeRoux指出开源PhoneGap的决定在Adobe收购Nitobi之前就做出了，由于Adobe现在拥有PhoneGap商标，他们不得不换个名字，第一个选中的名字是Callback，毫无创意，因此再改一次，产品现在叫Apache Cordova。随后adobe把 phonegap送给了apache软件基金会，接着apache把phonegap改名为cordova，cordova是Nitobi团队当时坐落的街道名称，用此名来纪念Nitobi团队的贡献。
所以Apache Cordova是从PhoneGap中抽出的核心代码，是驱动PhoneGap的核心引擎，。
- Ng-cordova介绍
    + [官网地址](http://ngcordova.com/)
- Ng-cordova是什么？
    + ngCordova是在Cordova Api基础上封装的一系列开源的AngularJs服务和指令扩展，让开发者可以方便的在HybridApp开发中调用设备能力，即可以在AngularJs代码中访问设备能力Api。
  + 在cordova插件的sucess和errorjs回调方法中，是无法使用 angularjs的$scope对象和注入的方法的，只能访问全局的方法和变量，这样会导致很多麻烦，必须使用传统的js方法写很多难看的代码。使用 ngCordova可以解决这个问题。

### React Native 介绍
- [官网](https://facebook.github.io/react-native/)
- [中文网](http://react-native.cn/)
- React native原理图

- React Native核心实现是什么？
  - React Native里面没有webview，这货不是Hybrid app，里面执行JS是用的 JavascriptCore。

  - 把Native code十来个最基本核心的类（RCTDeviceEventEmitter、RCTRenderingPerf等）、或组件（RCTView、RCTTextField、RCTTextView、RCTModalFullscreenView等）封装成二十来个基本组件（Popover、Listview等）

  - 实现了一套类似css的子集，用来解决样式问题，相当复杂和强大，靠这个才能将Native的核心组件组成JS层的基本组件再组成业务端的业务组件

  - 在React Native中，写JS的工程师解决的是「将基本组件拼装成可用的React组件」的问题，写Native Code的工程师解决的是「提供核心组件，提供足够的扩展性、灵活性和性能」的问题。

- React Native的优点有哪些？
  - 不用Webview，彻底摆脱了Webview让人不爽的交互和性能问题。
  - 有较强的扩展性，这是因为Native端提供的是基本控件，JS可以自由组合使用。
  - 可以直接使用Native原生的「牛逼」动画。
  - 可以通过更新远端JS，直接更新app。

- 现在那些公司在用React Native
  - [案例地址](https://facebook.github.io/react-native/showcase.html)

### ES6介绍
- ES6历史
  >1996年11月，JavaScript的创造者Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这种语言能够成 为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript。这个版本就是ECMAScript 1.0版。
 
- ES6是什么？
  +  ECMA-262 的第六次变更，所有以前按照惯例称为es6，ES2015
  + ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。在日常场合，这两个词是可以互换的。
  + 2015年6月，ECMAScript 6正式通过，成为国际标准。
  + Node.js是JavaScript语言的服务器运行环境。它们对ES6的支持度，比浏览器更高。通过它们，可以体验更多ES6的特性。
  + 这个标准的牛逼之处就在于会逐步统一前端，因为新增加的module,异步编程，Generator函数这些东西在angular中和node中都有很好的实现了。

- ES6学习地址
  + [ECMAScript6学习网址](http://es6.ruanyifeng.com/#docs/intro)
/intro)
