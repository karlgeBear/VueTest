# 什么是vue的生命周期
Vue 实例从开始创建、初始化数据、编译模板、挂载Dom和渲染、更新和渲染、卸载等一系列过程，这是 Vue 的生命周期

## vue的生命周期的八个钩子函数
- beforeCreat() 创建前 在new一个vue实例后，只有一些默认的生命周期钩子和默认事件，其他的东西都还没创建。在此生命周期执行的时候，data和methods中的数据都还没有初始化。不能在这个阶段使用data中的数据和methods中的方法
- created（）被创建 data 和 methods都已经被初始化好了，可以调用了
- beforeMount() 挂载前 在内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的
- mounted（）已挂载 Vue实例已经初始化完成了。此时组件脱离了创建阶段，进入到了运行阶段。 如果我们想要通过插件操作页面上的DOM节点，最早可以在和这个阶段中进行
- beforeupdate（）更新前 页面中的显示的数据还是旧的，data中的数据是更新后的， 页面还没有和最新的数据保持同步
- updated（）更新 页面显示的数据和data中的数据已经保持同步了，都是最新的
- beforeDestroy() 销毁前 Vue实例从运行阶段进入到了销毁阶段，这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于可用状态。还没有真正被销毁
- destroyed（）被销毁 这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于不可用状态。组件已经被销毁了。
## vue生命周期的作用是什么
- 它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑

## 第一次页面加载会触发哪几个钩子
- 第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子

## DOM渲染在哪个周期中已经完成
mounted

## 简述每个周期具体适合哪些场景
    beforecreate : 可以在这加个loading事件，在加载实例时触发
    created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用
    mounted : 挂载元素，获取到DOM节点
    updated : 如果对数据统一处理，在这里写上相应函数
    beforeDestroy : 可以做一个确认停止事件的确认框
    nextTick : 更新数据后立即操作dom

## created和mounted的区别
- created:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。
- mounted:在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。