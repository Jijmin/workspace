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

