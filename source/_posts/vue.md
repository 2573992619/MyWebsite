---
title: Vue框架 
date: 2023-11-21 13:07:41
tags: 前端 
---
# <center>Vue框架学习
- Vue 是一套前端框架，免除原生JavaScript中的DOM操作，简化书写。
- Vue基于MVVM(Model-View-ViewModel)思想，实现数据的双向绑定，将编程的关注点放在数据上。
- 详情请见[Vue的官方地址](https://cn.vuejs.org/)

## 一、使用步骤
1. 在HTML页面引入Vue.js文件
```html
<script src="js/vue.js"></script>
```
2. 在JS代码区域，创建Vue核心对象，定义数据模型
```html
<script>
    new Vue({
        el:"#app",//Vue接管的区域
        data: {
            message: "Hello Vue."
        }
    })
</script>
```
3. 编写视图
```html
<div id="app">
    <input type="text" v-model="message">
    {{ message }}//{{表达式}}称为插值表达式
</div>
```

## 二、常用指令
### 1.绑定
- v-bind:为HTML标签绑定属性，如设置href、css
- v-model：在表单元素上建立双向数据绑定
- v-on
- 注意：通过v-bind或v-model绑定的元素应在数据模型中有相关定义
```html
<!--示例一-->
<script>
    new Vue({
        el:"#app",//Vue接管的区域
        data: {
            url : "https：//www.miaojiangnan.com"
        }
    })
</script>
<input type="text" v-model="url">
<!--这里的输入框内的内容会直接影响数据模型url的内容-->
```
```html
<!--示例二-->
<body>
    <div id="app">
        <a v-bind:herf="url">链接1</a> 
<!--   这里a标签中的herf属性通过v-bind与数据模型中的url绑定     -->
    </div>
</body>
<script>
    new Vue({
        el:"#app",//Vue接管的区域
        data: {
            url : "https：//www.miaojiangnan.com"
        }
    })
</script>
```
### 2.条件渲染
- v-if
- v-else-if
- v-else
- v-show:条件展示
```html
年龄<input type="text" v-model="age">经判定为：
<span v-show="age<=35">年轻人</span>
```
### 3.for遍历
- v-for
```html
<div v-for="addr in addrs">{{addr}}</div>
<div v-for="(addr,index) in addrs">{{index + 1}} : {{addr}}</div>
data:{
    ......
    addrs : ['北京','上海']
}，
```
## 三、生命周期
![生命周期](http://123.56.141.254/img/Vue.png)