# 1. Vue.js是什么?
	1). 一位华裔前Google工程师(尤雨溪)开发的前端js库
	2). 作用: 动态构建用户界面
	3). 特点:
		* 遵循MVVM模式
		* 编码简洁, 体积小, 运行效率高, 移动/PC端开发
		* 它本身只关注UI, 可以轻松引入vue插件和其它第三库开发项目
	4). 与其它框架的关联:
		* 借鉴angular的模板和数据绑定技术
		* 借鉴react的组件化和虚拟DOM技术
	5). vue包含一系列的扩展插件(库):
		* vue-cli: vue脚手架
		* vue-resource(axios): ajax请求
		* vue-router: 路由
		* vuex: 状态管理
		* vue-lazyload: 图片懒加载
		* vue-scroller: 页面滑动相关
		* mint-ui: 基于vue的组件库(移动端)
		* element-ui: 基于vue的组件库(PC端)
  
# 2. 基本使用
	1). 引入vue.js
	2). 创建Vue实例对象(vm), 指定选项(配置)对象
		el : 指定dom标签容器的选择器
		data : 指定初始化状态数据的对象/函数(返回一个对象)
	3). 在页面模板中使用{{}}或vue指令
		
# 3. Vue对象的选项
## 1). el
	指定dom标签容器的选择器
	Vue就会管理对应的标签及其子标签

## 2). data
	对象或函数类型
	指定初始化状态属性数据的对象
	vm也会自动拥有data中所有属性
	页面中可以直接访问使用
	数据代理: 由vm对象来代理对data中所有属性的操作(读/写)

## 3). methods
	包含多个方法的对象
	供页面中的事件指令来绑定回调
	回调函数默认有event参数, 但也可以指定自己的参数
	所有的方法由vue对象来调用, 访问data中的属性直接使用this.xxx

## 4). computed （擅长处理的场景：一个数据受多个数据影响 ）
	包含多个方法的对象
	对状态属性进行计算返回一个新的数据, 供页面获取显示
	一般情况下是相当于是一个只读的属性
	利用set/get方法来实现属性数据的计算读取, 同时监视属性数据的变化
	如何给对象定义get/set属性
		在创建对象时指定: get name () {return xxx} / set name (value) {}
	  	对象创建之后指定: Object.defineProperty(obj, age, {get(){}, set(value){}})

## 5). watch（擅长处理的场景：多个数据受一个数据影响）
	包含多个属性监视的对象
	分为一般监视和深度监视
		'xxx' : {
			handler : fun(value),
			deep : true,
			immediate: false
		}
		1.第一个handler：其值是一个回调函数。即监听到变化时应该执行的函数。
		2.第二个是deep：其值是true或false；确认是否深入监听。（一般监听时是不能监听到对象属性值的变化的，数组的值变化可以听到。）
		3.第三个是immediate：其值是true或false；确认是否以当前的初始值执行handler的函数。
	另一种添加监视方式: vm.$watch('xxx', funn)

# 4. 过渡动画
	利用vue去操控css的transition/animation动画
	模板: 使用<transition name='xxx'>包含带动画的标签
	css样式
		.fade-enter-active: 进入过程, 指定进入的transition
		.fade-leave-active: 离开过程, 指定离开的transition
		.xxx-enter, .xxx-leave-to: 指定隐藏的样式
	编码例子
	    .xxx-enter-active, .xxx-leave-active {
	      transition: opacity .5s
	    }
	    .xxx-enter, .xxx-leave-to {
	      opacity: 0
	    }
	    
	    <transition name="xxx">
	      <p v-if="show">hello</p>
	    </transition>
    
# 5. 生命周期
	vm/组件对象
	生命周期图
	主要的生命周期函数(钩子)
    	created() / mounted(): 启动异步任务(启动定时器,发送ajax请求, 绑定监听)
    	beforeDestroy(): 做一些收尾的工作

# 6. 自定义过滤器
## 1). 理解
	对需要显示的数据进行格式化后再显示

## 2). 编码
	1). 定义过滤器
		Vue.filter(filterName, function(value[,arg1,arg2,...]){
		  // 进行一定的数据处理
		  return newValue
		})
	2). 使用过滤器
		<div>{{myData | filterName}}</div>
		<div>{{myData | filterName(arg)}}</div>
	
# 7. vue内置指令
	v-text/v-html: 指定标签体
    	* v-text : 当作纯文本
		* v-html : 将value作为html标签来解析
	v-if v-else v-show: 显示/隐藏元素
		* v-if : 如果vlaue为true, 当前标签会输出在页面中
		* v-else : 与v-if一起使用, 如果value为false, 将当前标签输出到页面中
		* v-show: 就会在标签中添加display样式, 如果vlaue为true, display=block, 否则是none
	v-for : 遍历
		* 遍历数组 : v-for="(person, index) in persons"   
		* 遍历对象 : v-for="value in person"   $key
	v-on : 绑定事件监听
		* v-on:事件名, 可以缩写为: @事件名
		* 监视具体的按键: @keyup.keyCode   @keyup.enter
		* 停止事件的冒泡和阻止事件默认行为: @click.stop   @click.prevent
		* 隐含对象: $event
	v-bind : 强制绑定解析表达式  
		* html标签属性是不支持表达式的, 就可以使用v-bind
		* 可以缩写为:  :id='name'
		* :class
		  * :class="a"
			* :class="{classA : isA, classB : isB}"
			* :class="[classA, classB]"
		* :style
			:style="{color : color}"
	v-model
		* 双向数据绑定
		* 自动收集用户输入数据
	ref : 标识某个标签
		* ref='xxx'
		* 读取得到标签对象: this.$refs.xxx
  
# 8. 自定义指令
## 1). 注册全局指令
    Vue.directive('my-directive', function(el, binding){
      el.innerHTML = binding.value.toUpperCase()
    })

## 2). 注册局部指令
    directives : {
      'my-directive' : function(el, binding) {
          el.innerHTML = binding.value.toUpperCase()
      }
    }

## 3). 使用指令
    <div v-my-directive='xxx'>

# 9. key的作用
    1. 虚拟DOM的key的作用?
       1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用
       2). 详细的说: 当列表数组中的数据发生变化生成新的虚拟DOM后, React进行新旧虚拟DOM的diff比较
           a. key没有变
               item数据没变, 直接使用原来的真实DOM
               item数据变了, 对原来的真实DOM进行数据更新
           b. key变了
               销毁原来的真实DOM, 根据item数据创建新的真实DOM显示(即使item数据没有变)
    2. key为index的问题
       1). 添加/删除/排序 => 产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低
       2). 如果item界面还有输入框 => 产生错误的真实DOM更新 ==> 界面有问题
       注意: 如果不存在添加/删除/排序操作, 用index没有问题
    3. 解决:
       使用item数据的标识数据作为key, 比如id属性值
# 10. extend构造器
## 1.理解
		注册组件三部曲：1. 创建组件构造器	2. 注册组件	3. 使用组件
## 2. 注册组件
```
//直接挂载到元素上
new Vue.extend({}).$mount('#app')  //这个方法返回实例自身，因而可以链式调用其它实例方法

//局部注册使用方法
1. 创建组件构造器
var App = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
});
new Vue({
  components: {
    // 2. 注册组件，3.同时挂载到#app元素上使用组件
    'App': App
  },
  template: '<App/>'
}).$mount('#app') */

全局注册组件
1创建组件构造器
  var App = Vue.extend({
      template: '<p>This is a component</p>'
  });
  //2全局注册组件
  Vue.component('App', App);
  //3使用组件
  var vm = new Vue({
      el: '#app'
  })

// 简化以上两种写法
new Vue({
  el: '#app',
  //注册组件的时候定义组件构造器
  components: {
    'App': {
      template: '<p>This is a component, it belong {{name}}</p>',
      data: function () {
        return {
          name:'world'
        }
      }
    }
  }
})
```
# 11. 分析Component和Vue的关系：
```
VueComponent.prototype.__proto__ === Vue.prototype
1.一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
2.为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。
组件实例对象（vc）是一个小型的vm ,没有el.data只能是对象式

原文链接：https://blog.csdn.net/qq_14993591/article/details/121174829
```
