### webpack
1. `npm install -g webpack`
2. 配置webpack.config.js文件，文件的配置类似于gulp
3. 通过配置文件中的entry获取到文件，output将文件输出
4. webpack通过loader（加载器）和plugin（插件）处理文件

### 文档地址
  - [官网](http://facebook.github.io/react)
  - [中文网](http://reactjs.cn/react)
  - [React中文社区](http://react-china.org/)

### 在线编辑工具
  - [使用js语法书写](https://jsfiddle.net/reactjs/5vjqabv3/)
  - [使用jsx语法书写](https://jsfiddle.net/reactjs/69z2wepo/)

### 真实案例
  - http://info.smartstudy.com/
  - http://www.kongkonghu.com/choice

### React中4个主要概念
1. VirtualDOM
2. React组件
3. JSX语法
4. Data Flow(单向数据流)

### React快速开始
1. 安装
- `npm install react --save-dev` // 负责创建组件
- `npm install react-dom --save-dev` // 渲染组件到页面
- 如果要使用webpack开发那么还需要下载webpack工具
    + 1. ` npm install babel-loader --save-dev `
    + 2. ` npm install babel-core --save-dev `
    + 3. ` npm install  babel-cli babel-preset-react  --save-dev`
2. 开发工具
- [在谷歌应用商店的链接](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)

### 开始编写react代码
1. 确定react渲染的位置
```
<div id="myDiv"></div>
```
2. 引用reactdom
```
import ReactDOM from 'react-dom'
```
3. 将写好的组件渲染到界面上
- 参数：1组件的内容，2需要将你的组件渲染的位置
```
ReactDOM.render(
    <h1>hello react</h1>,
    document.getElementById('myDiv')
);
```
4. 利用webpack将JSX转换成浏览器可以识别的代码
```
module.exports={
    entry:'./main.jsx',
    output:{
        path:__dirname,
        filename:'bundle.js'
    },
    module:{
        loaders:[
            {
                test:/\.jsx?$/,
                loader:'babel-loader',
                query:{
                    presets:['react','es2015']
                }
            }
        ]
    }
}
```
5. 如果用webpack打包不成功，可以使用一个翻译工具browser.js
6. 创建hello React组件
```
//类名一定要大写
class Hello extends React.Component{
    //当前组件所需要渲染的内容
    render(){
        //return后面一定要紧跟标签，如果要换行就一定要用小括号将标签包裹起来
        return (
            <div>
                {/*注释*/}
                <h1>hello react</h1>
                }
            </div>
        )
    }
}
ReactDOM.render(
    <Hello></Hello>,
    document.getElementById('myDiv');
);
```
7. state创建对应的参数
```
class Hello extends React.Component{
    constructor(){
        super();
        //初始化参数通过state来创建对应的参数
        this.state={
            number:100,
            name:'这是通过state方式创建的参数'
        }
    }
    render(){
        //如果想要修改对应的参数，不推荐
        this.setState(
            number:200
        );
        return (
            <div>
                <h1>hello react</h1>
                <h2>{this.state.name}</h2>
                <h3>{this.state.number}</h3>
                {/*
                    在react中的input标签如果需要修改value中的参数，需要使用defaultValue来将默认参数显示出来
                */}
                <input type="text" defaultValue={this.state.name} />
            </div>
        )
    }
}
ReactDOM.render(
    <Hello></Hello>,
    document.getElementById('myDiv');
);
```
8. 初始化参数通过state创建时，里面有html标签，会原样输出
9. 将要转义的前面加上
```
<div dangerousliSetInnerHTML={{__html:this.state.myh1}}>
</div>
```
10. 添加样式，在state里面添加
```
//react样式通过style方式设置或是通过className设置样式
myStyle:{
    'background-color':'red'
}
//在渲染的时候直接将样式绑定在标签上
<p style="{this.state.myStyle}">red</p>
//TODO
```
11. props的意思从外部传递过来内容通过props接收然后输出
```
<p style={this.state.myStyle}>{this.props.name}</p>
//TODO
```
12. 事件调用
```
click(){
    console.log('触发');
}
//在渲染函数中直接绑定点击事件，在事件调用的方法后面不需要加小括号
<input type="button" value="ClickMe" onClick={this.clickMe}>
```
13. 双向数据绑定
```
inputValueChange(){
    //input修改后的值this.refs.inputEdit.value
    //当前的name值this.state.name
    //修改当前name
    this.setState({
        //name:this.state.name+'?'
        name:this.refs.inputEdit.value
    });
}
//bind绑定当前this的指向
<input refs='inputEdit' type="text" defaultValue={this.state.name} onChange={this.inputValueChange.bind(this)}>
{this.state.name}
```

### 直接渲染页面
```
<div id="myDiv"></div>
```
```
<script src="js/react.js"></script>
<script src="js/react-dom.js"></script>
<!--翻译工具-->
<script src="js/browser.min.js"></script>
<!-- 创建script时候需要添加type="text/babel" -->
<script type="text/babel">
  ReactDOM.render(
        <h1>hello react</h1>,
        document.getElementById('myDiv')
    );
</script>
```

### 通过创建组件渲染
```
<!-- es5写法 -->
var Hello=React.createClass({
    render:function(){
        return (<div><h1>hello react</h1></div>)
    }
});
ReactDOM.render(
    <Hello></Hello>,
    document.getElementById('myDiv')
);
```
下面这种方式通过翻译工具browser.min.js不能解析，在jsx中通过webpack转义后可显示
```
class Hello extends React.Component{
  constructor(){
     super();
  }
  render(){
    return (<h1>es6</h1>)
  }
}
ReactDOM.render(
  <Hello></Hello>,
  document.getElementById('myDiv')
)
```

### 组件生命周期*
1. 组件的生命周期分成三个状态
- Mount 已插入真实DOM
- Update 正在被重新渲染
- Unmouting 已移出真实DOM
2. 状态处理函数

- React 为每个状态都提供了两种处理函数，will函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。
    + componentWillMount() //加载之前
    + componentDidMount() //已经在dom中
    + componentWillUpdate(object nextProps, object nextState) //将要更新
    + componentDidUpdate(object prevProps, object prevState) //已经更新
    + componentWillUnMount() //已经加载完成
    
- 此外，React 还提供两种特殊状态的处理函数。
    + componentWillReceiveProps(object nextProps)//已加载组件收到新的参数时调用
    + shouldComponentUpdate(object nextProps, object nextState)//组件判断是否重新渲染时调用，不会修改后立马被监视到，因为如果有多次渲染他会选最后一次渲染结果

![函数调用顺序图](../../images/react函数调用顺序图.png)

### react转义
```
import React from 'react'
import ReactDOM from 'react-dom'
class Hello extends React.Component{
    constructor(){
        super();
        //初始化参数通过state来创建对应的参数
        this.state={
            myh1:'<h1>myh1</h1>'
        }
    }
    render(){
        return (
            <div dangerouslySetInnerHTML={{__html:this.state.myh1}}></div>
        )
    }
}
ReactDOM.render(
    <Hello></Hello>,
    document.getElementById('myDiv')
);
```

### 样式的设置
```
import React from 'react'
import ReactDOM from 'react-dom'
class Hello extends React.Component{
    constructor(){
        super();
        //初始化参数通过state来创建对应的参数
        this.state={
            myh1:'<h1>myh1</h1>',
            myStyle:{
                'background-color':'red'
            }
        }
    }
    render(){
        return (
            <div>
                <p style={this.state.myStyle}>red</p>
                <p className="blue">blue</p>
            </div>
        )
    }
}
ReactDOM.render(
    <Hello></Hello>,
    document.getElementById('myDiv')
);
```

### 组件嵌套使用
```
import React from 'react'
import ReactDOM from 'react-dom'
class Hello extends React.Component{
    constructor(){
        super();
        //初始化参数通过state来创建对应的参数
        this.state={
            myh1:'<h1>myh1</h1>',
            myStyle:{
                'background-color':'red'
            }
        }
    }
    render(){
        return (
            <div>
                <p style={this.state.myStyle}>{this.props.name}</p>
            </div>
        )
    }
}
class Main extends React.Component{
    constructor(){
        super();
    }
    render(){
        return (
            <div>
        <h1>主组件</h1>
        <Hello name="这是从主组件传递过来的参数"></Hello>
      </div>
        )
    }
}
ReactDOM.render(
    <Main></Main>,
    document.getElementById('myDiv')
);
```

### 双向数据绑定
```
import React from 'react'
import ReactDOM from 'react-dom'

class Main extends React.Component{
    constructor(){
        super();
        this.state={
            name:'初始值'
        }
    }
    inputValueChange(){
        this.setState({
            name:this.refs.inputEdit.value
        });
    }
    render(){
        return (
            <div>
                <input ref="inputEdit" type="text" defaultValue={this.state.name} onChange={this.inputValueChange.bind(this)} />
                {this.state.name}
      </div>
        )
    }
}
ReactDOM.render(
    <Main></Main>,
    document.getElementById('myDiv')
);
```

### 组件的注意事项 
尽量不要在componentWillUpdate和componentDidUpdate中使用this.state，会造成死循环

### 在生命周期中移除组件
```
class Main extends React.Component{
    constructor(){
        super();
    }
    render(){
        return <h1>Mian</h1>
    }
}
ReactDOM.render(
    <Main></Main>,
    document.getElementById('myDiv')
)
```

### 遍历
```
import React from 'react'
import ReactDOM from 'react-dom'
class  MyP extends  React.Component{
    constructor(){
        super();
    }
    render(){
        return <p>{this.props.name}</p>
    }
}
class Main extends React.Component{
    constructor(){
        super();
        this.state={
            arr:[1,2,3,4,5]
        }
    }
    render(){
        return (
            <ul>
                {
                    this.state.arr.map((item)=>{
                        return (
                            <li><MyP name={item}></MyP></li>
                        )
                    })
                }
            </ul>
        )
    }
}
ReactDOM.render(
    <Main></Main>,
    document.getElementById('myDiv')
);
```

### 路由
1. 下载路由包
2. 引用react路由包
3. 在ReactRouter.Route中配置不同的路由
```
ReactDOM.render(
    <ReactRouter.Router history={ReactRouter.hashHistory}>
        <ReactRouter.Route path="/" component={Mian}></ReactRouter.Route>
        <ReactRouter.Route path="/Book/(:id)" component={Book}></ReactRouter.Route>
    </ReactRouter.Router>
)
```
4. 要拿取数据通过{this.props.params.id}

### 抽象路由
1. 在主路由中放置
2. 通过不同的url标签进行切换
3. react有一个类似a标签的标签Link标签
```
<ReactRouter.Link to="/Book/100">100</ReactRouter.Link>
```
4. 要显示需要使用一个类似于view，匹配好规则以后显示
```
{this.props.children}
```
5. 下面的路由配置就是我们的嵌套路由
6. activeClassName表示该路由一旦匹配后就显示对应的样式
```
<ReactRouter.Link to="/Book/100" activeClassName>100</ReactRouter.Link>
```
7. 综合来看
```
import React from 'react'
import {render} from 'react-dom'
import {Router,Route,Link,hashHistory} from 'react-router'
class Main extends React.Component{
    constructor(){
        super();
    }
    render(){
        return <h1>Main</h1>
    }
}
class Book extends React.Component{
    constructor(){
        super();
    }
    render(){
        return <h1>Book</h1>
    }
}
render((
  <Router history={hashHistory}>
    <Route  path="/" component={Main}></Route>
    <Route  path="/Book(/:id)" component={Book}></Route>
  </Router>),
  document.getElementById('myDiv')
)
```


### TODO
1. 在官网上下载我们TODO的模板
2. 将我们依赖包下载下来
3. 将我们的组件分离出来，去掉注释，标签闭合，class转换为className
4. 要将我们的组件渲染到我们的页面上，模板上是一个class属性，我们需要取class的第一个
5. 创建一个配置文件webpack.config.js
6. 将当前的TODOList循环遍历出来，对显示的文本进行绑定，对勾选按钮使用checked绑定状态，不能使用defaultValue绑定，这样只能绑定一次
7. 我们需要在TODOList数据中加上ID，是一个随机数去掉前面两位
8. 添加一个随机数函数，在我们需要生成ID的地方调用
9. 给每个li加一个key的id值
10. 添加一个form通过onSubmit事件提交数据的方法addTodo
11. 在表单中通过ref传入一个值来拿到数据
12. 提交事件的时候要改变this的值
13. 增加todo的时候通过refs获取到我们提交过来的数据
14. 在我们添加数据回车时会跳到新的界面
15. 我们通过e.preventDefault阻止默认事件
16. 将我们获取到的值进行重新渲染到下面的列表中
17. 要使用concat将新添加的数据和以前的todoList数据拼接
18. 做容错处理
19. 判断当前newTodo长度大于0，看看输入是否为空
20. 添加后将输入栏添加清空说
21. 删除的时候在按钮上添加一个点击事件
22. 因为要对this进行处理，我们要要对this进行绑定，还需要知道我们我们当前需要删除的项，传入参数todo即可
23. 在删除函数中我们需要先拿到当前todo
24. 通过下标进行删除
25. 再将整个todoList进行重新渲染
26. 在复选框添加一个onChange事件，将复选款的状态取反，重新渲染
27. 通过第三方插件classnames改变我们的样式的数据
28. 下载classnames包，并引入classnames包
29. 将样式改变className={classNames({completed:todo.completetd})}
30. 对数据进行修改
31. 双击label修改todo，如果修改的要修改的todo和当前todo相等就是我们需要修改状态的todo
32. 双击的时候进行重新渲染
33. 失去焦点的时候保存当前的todo修改的值，设置一个ref的id值，通过todo将id传出去
34. 在失去焦点的时候获取当前refs中的id值，将对应的数据进行保存
35. 修改editingTodo将其不指向当前Todo，置为空对象
36. 将获取到的修改的值保存在todo的text中
37. 子组件中不需要引用react-dom，将子组件写好之后放到我们的主组件中
38. 子组件要在外部进行访问，需要export ddefault出去
39. 在我们的主组件中引入我们的子组件
40. 将我们的尾部用标签显示
41. 通过props将外部属性进行传递
42. 通过过滤器进行统计已完成的数量
43. 将长度绑定到我们需要传回给子组件的属性上
44. 清除已经完成的就将过滤掉为false的，进行重新渲染
45. 全选的时候将下面的统计的数量进行取反来判断是否全选
46. 调用rander的时候可以对数量进行统计
47. 但是注意的是我们不能在判断之后使用setState，而是使用state
48. 在渲染的时候将数量进行取反
49. 根据toggleAll的取反的值更新到todoList的每一个todo的completed上
50. 通过map将每一个数给抛出，重新渲染
51. defaultChecked只能使用一次，应该使用checked
52. 下载react的路由模块，并引用路由模块
53. 根据锚点值取出数据，过滤的时候进行switch
54. 定义一个空数组将三种情况保存起来，进行渲染