### TODO实现
1. 在官方的GitHub下将模板克隆下来
2. 创建模型
3. 我们的控制器控制$scope对象和$filter
4. 小细节是要将我们的angular对象传入沙箱，就是我们的自执行函数
5. 我们用数组对象模拟我们的数据
```
(function (window,angular) {
  'use strict';
  var app=angular.module('todoApp',[]);
  app.controller('todoCtrl', ['$scope','$filter',function($scope,$filter){
    //查看
    //数据
    $scope.todoList=[
      {text:'css',completed:true},
      {text:'JavaScript',completed:false},
      {text:'NodeJS',completed:true}
    ];
  }])
})(window,angular);
```
6. 我们要显示的数据有多条我们需要使用angular中的遍历
7. 遍历我们的数据，我们给复选框添加了一个判断值，我们需要将状态值与前台显示进行绑定
```
<li ng-repeat="todo in todoList" ng-class="{completed:todo.completed}">
  <div class="view">
    <input class="toggle" type="checkbox" ng-model="todo.completed">
    <label>{{todo.text}}</label>
    <button class="destroy"></button>
  </div>
  <input class="edit" value="Rule the web">
</li>
```
8. 查询完毕，再实现增加功能
9. angular中没有按下回车键触发的事件，我们需要用一个小技巧，可以在input表单外面放置一个form，在我们的form中有一个提交事件
```
<form ng-submit="addTodo()">
```
10. 取到我们提交过来的值，插入我们的数据数组中
```
$scope.addValue='';
$scope.addTodo=function(){
  $scope.todoList.push({text:$scope.addValue,completed:false});
}
```
11. 提高容错性，我们要排除用户输入空以及提交后要将input标签清空
```
if($scope.addValue.trim().length>0){
  $scope.todoList.push({text:$scope.addValue,completed:false});
}
$scope.addValue='';
```
12. 删除
```
<button class="destroy" ng-click="delTodo(data)"></button>

$scope.delTodo=function(todo){
  //拿到当前的下标
  var index=$scope.todoList.indexOf(todo);
  $scope.todoList.splice(index,1);
}
```
13. 双击编辑
```
//模板帮我们实现了一个editing样式，我们可以根据判是空对象还是当前对象进行操作
<li ng-repeat="todo in todoList" ng-class="{completed:todo.completed,editing:todoedit==todo}">
//绑定双击事件
<label ng-dblclick="editTodo(todo)">{{todo.text}}</label>
//绑定一个是焦点事件，我们在编辑的时候需要取得原来的值
<input class="edit" ng-model="todo.text" type="text" ng-blur="saveTodo()">

$scope.todoedit={};
$scope.editTodo=function(todo){
  $scope.todoedit=todo;
};
$scope.saveTodo=function(){
  $scope.todoedit={};
};
```
14. 通过过滤器将我们没有完成的任务筛选出来，取出长度就是显示的值
```
$scope.countTodo=$filter('filter')($scope.todoList,{completed:false}).length;
<span class="todo-count"><strong>{{countTodo}}</strong> item left</span>
```
15. 这样是将数据写死了的，我们应该加入一个监控，监控数据的改变
```
$scope.$watch('todoList',function(newVal,oldValue){
  $scope.countTodo=$filter('filter')($scope.todoList,{completed:false}).length;
},true);
```
16. 添加过滤器对completed进行筛选
```
<ul class="filters">
  <li>
    <a class="selected" href="#/"  ng-click="changeCompleted('all')">All</a>
  </li>
  <li>
    <a href="#/active" ng-click="changeCompleted('active')">Active</a>
  </li>
  <li>
    <a href="#/completed"  ng-click="changeCompleted('completed')">Completed</a>
  </li>
</ul>
```
17. 渲染时添加过滤器
```
<li ng-repeat="todo in todoList | filter:completedValue" ng-class="{completed:todo.completed,editing:todoedit==todo}">
```
18. 过滤规则
```
$scope.completedValue={};
$scope.changeCompleted=function(status){
  switch(status){
    case 'active':
      $scope.completedValue={completed:false};
      break;
    case 'completed':
      $scope.completedValue={completed:true};
      break;
    default:
      $scope.completedValue={};
  }
};
```
19. 通过判断布尔值completed的取值可以实现样式之间的切换，布尔值只有两个，我们有三个状态切换，如果是全部都列列出来的，那么我们给completedValue传递的值是一个空对象，可以通过判断这个对象下面的completed值是否等于undefined进行判断
```
<ul class="filters">
  <li>
    <a class="selected" href="#/"  ng-click="changeCompleted('all')" ng-class="{selected:completedValue.completed==undefined}">All</a>
  </li>
  <li>
    <a href="#/active" ng-click="changeCompleted('active')"  ng-class="{selected:completedValue.completed==false}">Active</a>
  </li>
  <li>
    <a href="#/completed"  ng-click="changeCompleted('completed')"  ng-class="{selected:completedValue.completed==ture}">Completed</a>
  </li>
</ul>
```
20. 将完成的项目清除
```
$scope.clearCompleted=function(){
  var templateTodoList=[];
  $scope.todoList.forEach(function(todo){
    if(todo.completed==false){
      templateTodoList.push(todo);
    }
    $scope.todoList=templateTodoList;
  });
};
```
21. 全选和全不全
```
$scope.toggleChange=function(){
  $scope.todoList.forEach(function(todo){
    todo.completed=$scope.toggleAll;
  });
};
```
22. 但是发现我们在下面去掉勾选之后上面的没有得到对应的改变
```
$scope.$watch('todoList',function(newVal,oldValue){
  $scope.countTodo=$filter('filter')($scope.todoList,{completed:false}).length;
  $scope.toggleAll=!$scope.countTodo;
},true);
```
23. 但是我在这里犯了一个很严重的问题，我们的逻辑是切换的选项是一个布尔值，不是一个条件判断
24. 在双击页面的时候没有聚焦
```
app.directive('editFoucs',function(){
  return {
    link:function(scope,ele,attr){
      ele.on('dblclick',function(){
        //转换成JQlite对象
        angular.element(this).find('input').eq(1)[0].focus();
      });
    }
  }
});
```
25. 创建一个指令要在页面添加上我们的指令
```
<li edit-foucs ng-repeat="todo in todoList | filter:completedValue" ng-class="{completed:todo.completed,editing:todoedit==todo}">
//...
</li>
```
26. 改造成单页面应用程序
27. 创建一个路由模块，angular里面路由是分开的
28. 因此我们需要将路由模块引入
```
<script src="node_modules/angular-route/angular-route.js"></script>
```
29. 定义一个路由，将路由注入我们的模块中
```
var app=angular.module('todoApp',['ngRoute'],function ($routeProvider) {
  $routeProvider
    .when('/:completedValue?',{
      //:表示改参数可以变换
      //加上?表示可以为空
      templateUrl:'template/todoTemplate.html',
      controller:'todoCtrl'
    })
});
```
30. 我们对下面按钮的切换会有锚点跳转，页面没有刷新，所以我们需要进行路由设置
31. 将我们的页面分离出来方便路由控制
32. 添加一个我们的指令`<ng-view></ng-view>`占位显示
33. 但是一个问题就是我们会发现数据没有变化，我们需要将数据存储在我们localStorage中
```
function setTodoList(todoList) {
  localStorage.setItem("todoList",JSON.stringify(todoList));
}
//获取数据
function getTodoList() {
  return JSON.parse(localStorage.getItem("todoList")||'[]');
}
```
34. 只能通过监视completedValue的值才能实时存储我们的数据
```
$scope.routeParamsCompletedValue=$routeParams.completedValue;
$scope.$watch('routeParamsCompletedValue',function (newVal,oldVal) {
  switch(newVal){
  case 'active':
    $scope.completedValue={completed:false};
    break;
  case 'completed':
    $scope.completedValue={completed:true};
    break;
  default:
    $scope.completedValue={};
    break;
  }
});
```
35. 但是发现有一个问题，就是我们新添加一条数据的时候，再刷线，我们的数据才会存储起来，其他的时候在数据前面打钩或者是去掉钩再切换时是没有得到对应的改变的