# 移动App第7天ReactJS
## 复习
1.npm install -g webpack
2.配置webpack.config.js文件，文件的配置类似于gulp
3.通过配置文件中的entry获取到文件，output将文件输出
4.webpack通过loader（加载器）和plugin（插件）处理文件

## React简介
### 组件化的特点
    1.高内聚，低耦合
### 1.1. React是什么
  >React是Facebook开源的一个用于构建用户界面的Javascript库，已经 应用于Facebook及旗下Instagram。和庞大的AngularJS不同，React专注于MVC架构中的V，即视图。这使得React很容易和开发者已有的开发栈进行融合。React顺应了Web开发组件化的趋势。应用React时，你总是应该从UI出发抽象出不同 的组件，然后像搭积木一样把它们拼装起来
 
  >这个项目本身也越滚越大，从最早的UI引擎变成了一整套前后端通吃的 Web App 解决方案。衍生的 React Native 项目，目标更是宏伟，希望用写 Web App 的方式去写  Native App。如果能够实现，整个互联网行业都会被颠覆，因为同一组人只需要写一次 UI ，就能同时运行在服务器、浏览器和手机
  
### 1.2. 文档地址
  - [官网](http://facebook.github.io/react)
  - [中文网](http://reactjs.cn/react)
  - [React中文社区](http://react-china.org/)


### 1.3. 在线编辑工具
  - [使用js语法书写](https://jsfiddle.net/reactjs/5vjqabv3/)
  - [使用jsx语法书写](https://jsfiddle.net/reactjs/69z2wepo/)

### 1.4. 为什么要使用React

  > 我们创造 React 是为了解决一个问题：构建随着时间数据不断变化的大规模应用程序。为了达到这个目标，React 采用下面两个主要的思想。

  - 1、简单 
    + 仅仅只要表达出你的应用程序在任一个时间点应该长的样子，然后当底层的数据变了，React 会自动处理所有用户界面的更新。
  - 2、声明式 (Declarative) 
    + 数据变化后，React 概念上与点击“刷新”按钮类似，但仅会更新变化的部分。
  - 3、构建可组合的组件 
    + React 都是关于构建可复用的组件。事实上，通过 React 你唯一要做的事情就是构建组件。得益于其良好的封装性，组件使代码复用、测试和关注分离（separation of concerns）更加简单。
 
  - [更多原因](http://facebook.github.io/react/blog/2013/06/05/why-react.html)
  - 模块化和组件化的区别
      + 模块化中的模块一般指的是 Javascript 模块，比如一个用来格式化时间的模块。

组件则包含了 template、style 和 script，而它的 Script 可以由各种模块组成。比如一个显示时间的组件会调用上面的那个格式化时间的模块。
### 1.5. react使用说明

  >希望以上内容让你明白了如何思考用 React 去构造组件和应用。虽然可能比你之前要输入更多的代码，记住，读代码的时间远比写代码的时间多，并且阅读这种模块化的清晰的代码是相当容易的。当你开始构 建大型的组件库的时候，你将会非常感激这种清晰性和模块化，并且随着代码的复用，整个项目代码量就开始变少了。

### 1.6. 真实案例
  - http://info.smartstudy.com/
  - http://www.kongkonghu.com/choice
### 1.7 建议在React中使用ES6语法
```
function Person() {
        this.name='person';
    }
    function Beijin() {
        this.name='bj'
    }
    Beijin.prototype=new Person();
    var person=new Beijin();
    console.log(person.name);
    class PersonES6{
         constructor(){
             this.name='Person';
         }
     }
     class BeijinES6 extends PersonES6{
         constructor(){
             super();
             this.name='Beijin';
         }
     }
```
## 2 React中4个主要概念

### 2.1. VirtualDOM
#### 2.1.1 虚拟DOM是React的基石

  >之所以引入虚拟DOM，一方面是性能的考虑。Web应用和网站不同，一个Web应用中通常会在单页内有大量的DOM操作，而这些DOM操作很慢.在React中，应用程序在虚拟DOM上操作，这让React有了优化的机会。简单说，React在每次需要渲染时，会先比较当前DOM内容和待渲染内容的差异，然后再决定如何最优地更新DOM。这个过程被称为reconciliation。
  >除了性能的考虑，React引入虚拟DOM更重要的意义是提供了一种一致的开发方 式来开发服务端应用、Web应用和手机端应用
  ![virtualDOM](reademeImg/virtualImg.png)

#### 2.1.2. Virtual DOM速度快的说明
  >在Web开发中，我们总需要将变化的数据实时反应到UI上，这时就需要对DOM进行操作。而复杂或频繁的DOM操作通常是性能瓶颈产生的原因（如何 进行高性能的复杂DOM操作通常是衡量一个前端开发人员技能的重要指标）。React为此引入了虚拟DOM（Virtual DOM）的机制：在浏览器端用Javascript实现了一套DOM API。基于React进行开发时所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后React将当前 整个DOM树和上一次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。而且React能够批处理虚拟 DOM的刷新，在一个事件循环（Event Loop）内的两次数据变化会被合并，例如你连续的先将节点内容从A变成B，然后又从B变成A，React会认为UI不发生任何变化，而如果通过手动控 制，这种逻辑通常是极其复杂的。尽管每一次都需要构造完整的虚拟DOM树，但是因为虚拟DOM是内存数据，性能是极高的，而对实际DOM进行操作的仅仅是 Diff部分，因而能达到提高性能的目的。这样，在保证性能的同时，开发者将不再需要关注某个数据的变化如何更新到一个或多个具体的DOM元素，而只需要 关心在任意一个数据状态下，整个界面是如何Render的。

### 2.2. React组件

#### 2.2.1 概念
- 组件化概念
  >虚拟DOM(virtual-dom)不仅带来了简单的UI开发逻辑，同时也带来了组件化开发的思想，所谓组件，即封装起来的具有独立功能的UI部 件。React推荐以组件的方式去重新思考UI构成，将UI上每一个功能相对独立的模块定义成组件，然后将小的组件通过组合或者嵌套的方式构成大的组件， 最终完成整体UI的构建。例如，Facebook的instagram.com整站都采用了React来开发，整个页面就是一个大的组件，其中包含了嵌套 的大量其它组件，大家有兴趣可以看下它背后的代码。

  >如果说MVC的思想让你做到视图-数据-控制器的分离，那么组件化的思考方式则是带来了UI功能模块之间的分离。我们通过一个典型的Blog评论界面来看MVC和组件化开发思路的区别。

  >对于MVC开发模式来说，开发者将三者定义成不同的类，实现了表现，数据，控制的分离。开发者更多的是从技术的角度来对UI进行拆分，实现松耦合。
  >对于React而言，则完全是一个新的思路，开发者从功能的角度出发，将UI分成不同的组件，每个组件都独立封装。

  >在React中，你按照界面模块自然划分的方式来组织和编写你的代码，对于评论界面而言，整个UI是一个通过小组件构成的大组件，每个组件只关心自己部分的逻辑，彼此独立。
  ![图](reademeImg/zujian.jpg)
-  组件化开发特性
React认为一个组件应该具有如下特征：
（1）可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，一个复杂的UI可以拆分成多个简单的UI组件；
（2）可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个UI场景；
（3）可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护；
（4）可测试（Testable）：因为每个组件都是独立的，那么对于各个组件分别测试显然要比对于整个UI进行测试容易的多。


#### 2.2.2. React定义一个组件

```
  es5定义一个组件
    var Hello=  React.createClass({
        render:functoin(){
           return <div>哈哈只<div>
        }
      })
```

```
  把html渲染到页面上去
    ReactDOM.render(
      <Hello />,
      document.getElementById('example')
    );
```

### 2.3. JSX语法
  - jsx -> js xml

#### 2.3.1 什么是JSX  
  >在用React写组件的时候，通常会用到JSX语法，粗看上去，像是在Javascript代码里直接写起了XML标签，实质上这只是一个语法糖，每一个 XML标签都会被JSX转换工具转换成纯Javascript代码.
  在解析jsx语法时，遇到<就当作html解析，遇到{}就当js解析

  > 当然你想直接使用纯Javascript代码写也是可以的，只是利用JSX，组件的结 构和组件之间的关系看上去更加清晰


#### 2.3.2 JSX语法使用

  > HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 JSX 的语法，它允许 HTML 与 JavaScript 的混写。

  ```
    var names = ['Alice', 'Emily', 'Kate'];

      ReactDOM.render(
        <div>
        {
          names.map(function (name) {
            return <div>Hello, {name}!</div>
          })
        }

  ```
  >上面代码体现了 JSX 的基本语法规则：遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析。

  - JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员

  ```
      var arr = [
      <h1>Hello world!</h1>,
      <h2>React is awesome</h2>,
    ];
      ReactDOM.render(
        <div>{arr}</div>,
        document.getElementById('example')
      );

  ```

### 2.4 Data Flow(单向数据流)
  - 没有双向，如果想实现双向数据流,就需要我们自己去写代码.


## 3. React快速开始
### 3.1. 安装
  - `npm install react --save-dev` // 负责创建组件
  - `npm install react-dom --save-dev` // 渲染组件到页面
  - 如果要使用webpack开发那么还需要下载webpack工具
   + 1. ` npm install babel-loader --save-dev `
   + 2. ` npm install babel-core --save-dev `
   + 3. ` npm install  babel-cli babel-preset-react  --save-dev`
### 3.2. 开发工具
  - react-devtools 谷歌插件
  - [在谷歌应用商店的链接](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
### 3.3 使用步骤
- 1.直接渲染页面
```
    //1.引入js文件
    <script src="node_modules/browser.min.js"></script>
    <script src="node_modules/react/react.js"></script>
    <script src="node_modules/react-dom/dist/react-dom.js"></script>
    //2创建script时候需要添加type="text/babel"
    <script type="text/babel">
        //3 渲染页面
        ReactDOM.render(
            <h1>Hello React</h1>,
            document.getElementById('example')
        );
    </script>
```
- 2.通过创建组件渲染(标签必须要大写并且是闭合标签)
```
<!--es5写法-->
<script type="text/babel">
    var Hello=React.createClass({
        render:function () {
            return (<div><h1>hello React Class</h1></div>)
        }
    });
    ReactDOM.render(
           <Hello></Hello>,
            document.getElementById('box')
    );
</script>
```
```
<script type="text/babel">
    //使用es6语法需要继承React.Component
    class Hello extends React.Component{
        constructor(){
            super();
        }
        render(){
            return (
                    <h1>
                     {
                            //注释
                        }
                        es6
                    </h1>
            )
        }
    }
    ReactDOM.render(
            <Hello/>,
            document.getElementById('box')
    )
</script>
```
### 4.2. jsx语法详解
### 4.2.1 HTML 转义（了解）

  >React 会将所有要显示到 DOM 的字符串转义，防止 XSS。所以如果 JSX 中含有转义后的实体字符比如 &copy; (©) 最后显示到 DOM 中不会正确显示，因为 React 自动把 &copy; 中的特殊字符转义了。有几种解决办法：
```
    1 直接使用 UTF-8 字符 ©
    2 使用对应字符的 Unicode 编码
    3 使用数组组装 <div>{['cc ', <span>&copy;</span>, ' 2015']}</div>
    4 直接插入原始的 HTML
    <div dangerouslySetInnerHTML={{__html: 'cc &copy; 2015'}} />
    dangerouslySetInnerHTML参考文档
    http://reactjs.cn/react/tips/dangerously-set-inner-html.html
```
#### 4.2.2 自定义 HTML 属性（了解）
  - 如果在 JSX 中使用的属性不存在于 HTML 的规范中，这个属性会被忽略。如果要使用自定义属性，可以用 data- 前缀。
  可访问性属性的前缀 aria- 也是支持的。
  与dom的区别文档：http://reactjs.cn/react/docs/dom-differences.html

#### 4.2.3 支持的标签和属性
  - 如果你要使用的某些标签或属性不在这些支持列表里面就可能被 React 忽略，必须要使用的话可以用前面提到的 dangerouslySetInnerHTML。
  - 所有的属性都是驼峰命名的，class 属性和 for 属性分别改为 className 和 htmlFor，来符合 DOM API 规范。
  - 支持列表：http://reactjs.cn/react/docs/tags-and-attributes.html

#### 4.2.4 属性扩散
  - 有时候你需要给组件设置多个属性，你不想一个个写下这些属性，或者有时候你甚至不知道这些属性的名称，这时候 spread attributes 的功能就很有用了。
  ```
   var obj={
    namea:'1',
    nameb:'2',
    namec:'3',
    }
    ReactDOM.render(
        <MyHello name1={obj.namea} name2="bbb" name3="ccc"/>,
        document.getElementById('box')
    )
  ```

#### 4.2.5 自闭合标签
  - 在ReactJS中所有标签必需闭合
  - [官方说明](http://reactjs.cn/react/tips/self-closing-tag.html)

#### 4.2.6 注释
  - 在 JSX 里使用注释也很简单，就是沿用 JavaScript，唯一要注意的是在一个组件的子元素位置使用注释要用 {} 包起来 

### 4.3. ReactJS顶级API
  - http://facebook.github.io/react/docs/top-level-api.html


### 4.4.组件生命周期(重要)
  - 组件的生命周期，另外的名字是状态回调，和上面讲的状态的唯一差别，上面的状态是它里面的元素，而组件的生命周期是它自己

#### 4.4.1. 组件的生命周期分成三个状态
  - Mount 已插入真实DOM
  - Update 正在被重新渲染
  - Unmouting 已移出真实DOM

#### 4.4.2. 状态处理函数
  - React 为每个状态都提供了两种处理函数，will函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。
    + componentWillMount() //加载之前
    + componentDidMount()  //已经再dom中
    + componentWillUpdate(object nextProps, object nextState) //将要更新
    + componentDidUpdate(object prevProps, object prevState)  //已经更新
    + componentWillUnMount() //已经加载完成
  - 此外，React 还提供两种特殊状态的处理函数。
    + componentWillReceiveProps(object nextProps)
      * 已加载组件收到新的参数时调用
    + shouldComponentUpdate(object nextProps, object nextState)
      * 组件判断是否重新渲染时调用，不会修改后立马被监视到，因为如果有多次渲染他会选最后一次渲染结果

#### 修改初始值
  ```
  //建议在构造函数内使用去初始值
  this.state={
            name:"123"
        }
  //如果要对数据进行修改，在componentWillMount中对数据进行修改
  componentWillMount(){
        this.setState({name:'444'})
    }
  ```
#### props使用
 - 可以通过使用props将组件内部的参数进行修改
 ```
  class MyHello extends React.Component{
    constructor(){
        super(); 
    } 
    render(){
        return <div> 
                    <button>{this.props.name}</button>
                </div>
    }
}
ReactDOM.render(
    <MyHello name="btn"/>,
    document.getElementById('box')
)
    //使用
 ```
#### 代码使用

  ```es5
    var Hello = React.createClass({
    getInitialState() {
        return { liked: false };
    },
    render: function() {
        console.log(this.state.liked);
        return(
            <div>
                <h1 style={style}>Hello world</h1>
                <br/>
                <image/>
            </div>
        )
    }
  });
  ```
  
  ```es6
    export default class Hello extends Component {
    constructor(props) {
        super(props);

        this.state = { count: 'es6'};
    }
    render() {
        return (
            <div>
                <h1 style={style}>Hello world{this.state.count}</h1>
                <br/>
                <image/>
            </div>
        )
    }
  }
  ```

#### 相关资料
  - http://www.cnblogs.com/CHONGCHONG2008/p/5099483.html
  - http://pinggod.com/2015/React-%E7%BB%84%E4%BB%B6%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F/
http://reactjs.cn/react/docs/component-specs.html

### 4.5. React与ReactNative中 es5与es6语法对照
  - [React的](http://www.tuicool.com/articles/equ2my)
  - [ReactNative的](http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8/2)
### 书的人
angular 大漠穷秋 雪狼 孤狼
react es6 阮一峰
nodejs 朴灵


 
