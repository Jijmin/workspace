# 基础
## 介绍
### VueJS是什么
- Vue.js是一套构建用户界面的渐进式框架。
- 与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。
- Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合。
- 另一方面，Vue 完全有能力驱动采用单文件组件和 Vue 生态系统支持的库开发的复杂单页应用。
- Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。

### 引入vue.js
```
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

### 命令行工具
Vue.js 提供一个官方命令行工具，可用于快速搭建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev
```

### 声明式渲染
Vue.js 的核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统：
```
<div id="app">
{{message}}
</div>
```
```
var app=new Vue({
    el:'#app',
    data:{
        message:'Hello Vue!'
    }
});
```
除了绑定插入的文本内容，我们还可以通过采用v-bind方式绑定DOM的属性
```
<div id="app-2">
    <span v-bind:title="message">Hello</span>
</div>
```
```
var app2=new Vue({
    el:'#app-2',
    data:{
        message:'Hi'
    }
});
```
- `v-bind`属性被称为指令。指令前缀带有v-，表示-它们是VueJS提供的额特殊属性。
- 将这个元素节点的 title 属性和 Vue 实例的 message 属性绑定到一起。
- 你再次打开浏览器的控制台输入 app2.message = 'some new message'，你就会再一次看到这个绑定了title属性的HTML已经进行了更新。

### 条件与循环
```
<div id="app-3">
    <p v-if="seen">123</p>
</div>
```
```
var app3=new Vue({
    el:'#app-3',
    data:{
        seen:true
    }
});
```
- 在控制台输入`app3.seen = false`，你会发现 “123” 消失了。移除了整个DOM节点
- 我们不仅可以绑定 DOM 文本到数据，也可以绑定 DOM 结构到数据。
- Vue.js 也提供一个强大的过渡效果系统，可以在 Vue 插入/删除元素时自动应用过渡效果。
- `v-for` 指令可以绑定数据到数组来渲染一个列表
```
<div id="app-4">
    <ol>
        <li v-for="todo in todos">
            {{todo.text}}
        </li>
    </ol>
</div>
```
```
var app4=new Vue({
    el:'#app-4',
    data:{
        todos:[
            {text:'JavaScript'},
            {text:'HTML'},
            {text:'CSS'}
        ]
    }
});
```
在控制台里，输入 `app4.todos.push({ text: 'New item' })`。你会发现列表中多了一栏新内容

### 处理用户输入
为了让用户和你的应用进行互动，我们可以用 v-on 指令绑定一个监听事件用于调用我们 Vue 实例中定义的方法
```
<div id="app">
    <p>{{message}}</p>
    <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
```
var app=new Vue({
    el:'#app',
    data:{
        message:'Hello VueJS'
    },
    methods:{
        reverseMessage:function(){
            this.message=this.message.split('').reverse().join('')
        }
    }
});
```
在 reverseMessage 方法中，我们在没有接触 DOM 的情况下更新了应用的状态 - 所有的 DOM 操作都由 Vue 来处理，你写的代码只需要关注基本逻辑。
Vue 也提供了 v-model 指令，它使得在表单输入和应用状态中做双向数据绑定变得非常轻巧。
```
<p>{{message}}</p>
<input type="text" v-model="message">
```

### 用组件构建（应用）
- 组件系统是 Vue.js 另一个重要概念，因为它提供了一种抽象，让我们可以用独立可复用的小组件来构建大型应用。
- 在 Vue 里，一个组件实质上是一个拥有预定义选项的一个 Vue 实例
```
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```
现在你可以另一个组件模板中写入它
```
<ol>
  <todo-item></todo-item>
</ol>
```
但是这样会为每个 todo 渲染同样的文本，这看起来并不是很酷。我们应该将数据从父作用域传到子组件。让我们来修改一下组件的定义，使得它能够接受一个 prop 字段：
```
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```
现在，我们可以使用 v-bind 指令将 todo 传到每一个重复的组件中
```
<div id="app-7">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
```
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
子元素通过 props 接口实现了与父亲元素很好的解耦

## Vue实例
### 构造器
每个 Vue.js 应用都是通过构造函数 Vue 创建一个 Vue 的根实例 启动的：
```
var vm = new Vue({
  // 选项
})
```
- 在实例化 Vue 时，需要传入一个选项对象，它可以包含数据、模板、挂载元素、方法、生命周期钩子等选项。
- 可以扩展 Vue 构造器，从而用预定义选项创建可复用的组件构造器。
```
var MyComponent=Vue.extend({
    //扩展选项
});
// 所有的 `MyComponent` 实例都将以预定义的扩展选项被创建
var myComponentInstance = new MyComponent()
```
- 尽管可以命令式地创建扩展实例，不过在多数情况下建议将组件构造器注册为一个自定义元素，然后声明式地用在模板中。

### 属性与方法
每个 Vue 实例都会代理其 data 对象里所有的属性：
```
var data = { a: 1 }
var vm = new Vue({
  data: data
})
vm.a === data.a // -> true
// 设置属性也会影响到原始数据
vm.a = 2
data.a // -> 2
// ... 反之亦然
data.a = 3
vm.a // -> 3
```
- 注意只有这些被代理的属性是响应的。如果在实例创建之后添加新的属性到实例上，它不会触发视图更新。
- 除了 data 属性， Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。
```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
  // 这个回调将在 `vm.a`  改变后调用
})
```
> 注意，不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。

### 实例生命周期
每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如，实例需要配置数据观测(data observer)、编译模版、挂载实例到 DOM ，然后在数据变化时更新 DOM 。在这个过程中，实例也会调用一些 生命周期钩子 ，这就给我们提供了执行自定义逻辑的机会。例如，created 这个钩子在实例被创建之后被调用：
```
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
```
也有一些其它的钩子，在实例生命周期的不同阶段调用，如 mounted、 updated 、destroyed 。钩子的 this 指向调用它的 Vue 实例。一些用户可能会问 Vue.js 是否有“控制器”的概念？答案是，没有。组件的自定义逻辑可以分布在这些钩子中。

### 生命周期图示
![Vue](../../images/vue_lifecycle.png)

## 模板语法
1. Vue.js 使用了基于 HTML 的模版语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。
2. 在底层的实现上， Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。
3. 如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，直接写渲染（render）函数，使用可选的 JSX 语法。

### 插值
1. 文本
```
<span>Message: {{ msg }}</span>
```
```
//能执行一次性地插值，当数据改变时，插值处的内容不会更新
<span v-once>This will never change: {{ msg }}</span>
```
2. 纯HTML
```
<div v-html="rawHtml"></div>
```
被插入的内容都会被当做 HTML —— 数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担任 UI 重用与复合的基本单元。
> 你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容插值。
3. 属性
```
<div v-bind:id="dynamicId"></div>
```
这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
```
<button v-bind:disabled="someDynamicCondition">Button</button>
```
4. 使用JavaScript表达式
```
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
```
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```
> 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

### 指令
1. 参数
一些指令能接受一个“参数”，在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：
```
<a v-bind:href="url"></a>
//在这里 href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
```
另一个例子是 v-on 指令，它用于监听 DOM 事件：
```
<a v-on:click="doSomething">
```
2. 修饰符
修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指定应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
```
<form v-on:submit.prevent="onSubmit"></form>
```

### 过滤器
Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。过滤器应该被添加在 mustache 插值的尾部，由“管道符”指示：
```
{{ message | capitalize }}
<div v-bind:id="rawId | formatId"></div>
```
> Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用计算属性。

过滤器函数总接受表达式的值作为第一个参数
```
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```
过滤器可以串联：
```
{{ message | filterA | filterB }}
```
过滤器是 JavaScript 函数，因此可以接受参数：
```
{{ message | filterA('arg1', arg2) }}
```

### 缩写
1. v-bind缩写
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```
2. v-on缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

## 计算属性
### 计算属性
```
<div id="example">
  {{message.split('').reverse().join('')}}
</div>
```
在实现反向显示 message 之前，你应该确认它。这个问题在你不止一次反向显示 message 的时候变得更加糟糕。
这就是为什么任何复杂逻辑，你都应当使用计算属性。

#### 基础例子
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```
var vm=new Vue({
  el:'#example',
  data:{
    message:'Hello'
  },
  computed:{
    reversedMessage:function(){
      return this.message.split('').reverse().join('');
    }
  }
});
//你可以打开浏览器的控制台，修改 vm 。 vm.reversedMessage 的值始终取决于 vm.message 的值。
```
你可以像绑定普通属性一样在模板中绑定计算属性。 Vue 知道 vm.reversedMessage 依赖于 vm.message ，因此当 vm.message 发生改变时，依赖于 vm.reversedMessage 的绑定也会更新。而且最妙的是我们是声明式地创建这种依赖关系：计算属性的 getter 是干净无副作用的，因此也是易于测试和理解的。

#### 计算缓存VS Methods
1. 我们可以通过添加函数一样实现倒置的功能
2. 计算属性是基于它的依赖缓存
3. 计算属性只有在它的相关依赖发生改变时才会重新取值
4. 只要 message 没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
5. 这也同样意味着如下计算属性将不会更新，因为 Date.now() 不是响应式依赖
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
6. 相比而言，每当重新渲染的时候，method 调用总会执行函数。
7. 我们为什么需要缓存？
- 假设我们有一个重要的计算属性 A 
- 这个计算属性需要一个巨大的数组遍历和做大量的计算
- 然后我们可能有其他的计算属性依赖于 A 
- 如果没有缓存，我们将不可避免的多次执行 A 的 getter 
- 如果你不希望有缓存，请用 method 替代

#### 计算属性VS Watched Property
1. Vue.js 提供了一个方法 $watch ，它用于观察 Vue 实例上的数据变动。
2. 当一些数据需要根据其它数据变化时， $watch很好使用
3. 通常更好的办法是使用计算属性而不是一个命令式的 $watch 回调。
```
<div id="demo">{{ fullName }}</div>
```
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
```
var vm=new Vue({
    el:'#demo',
    data:{
      firstName:'Foo',
      lastName:'Bar',
    },
    computed:{
      fullName:function(){
        return this.firstName+' '+this.lastName
      }
    }
  });
```

#### 计算setter
```
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
现在在运行 vm.fullName = 'John Doe' 时， setter 会被调用， vm.firstName 和 vm.lastName 也会被对应更新。

### 观察 Watchers
虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的 watcher 。这是为什么 Vue 提供一个更通用的方法通过 watch 选项，来响应数据的变化。当你想要在数据变化响应时，执行异步操作或开销较大的操作，这是很有用的。

