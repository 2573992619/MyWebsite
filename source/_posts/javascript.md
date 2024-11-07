---
title: Javascript
date: 2023-11-20 19:27:53
tags: 前端
---
# <center>JavaScript学习笔记
JavaScript的详细文档资料见[W3school](https://www.w3school.com.cn/jsref/index.asp)
## 一、引入方式
### 1.作为内部脚本
直接定义在HTML文档中，可以定义任意数量、放在任意位置。
- 必须位于<script></script>标签之间
- 一般放置在<body>元素底部，可以改善显示速度
### 2.作为外部脚本
将JS代码定义在外部.js文件中，然后引入到HTML页面中
- 外部文件中不用包含标签，只写代码
- 引入HTML时, script标签不可以自闭合
## 二、基本语法
- JS的语法区分大小写、
- 结尾分号可有可无、
- 单行注释用//，多行用/* xxx */
- 用大括号表示代码块
### 1.输出语句
1. 使用 window.alert() 写入警告框使用
2. document.write() 写入 HTML 输出使用
3. console.log() 写入浏览器控制台

### 2.变量
1. 通过关键字var定义全局变量
2. var中的变量可以是多种类型，且可以重复
3. let关键字定义局部变量，不可重复定义
4. const关键字定义常量，定义后不可改

### 3.数据类型
- number: 数字(整数、小数、NaN(Not a Number))
- string:字符串，单双引皆可
- boolean: 布尔。true，false
- null: 对象为空
- undefined:当声明的变量未初始化时，该变量的默认值是 undefined
- 使用 typeof 关键字可以获取变量的数据类型
```javascript
    var a = 3.14;
    alart(typeof a);
```
### 4.运算符
与别的语言一样，额外需要注意的是==和===
- ==比较的变量类型不同的时候会先类型转换
- ===不进行转换，类型不同就是false  
```javascript
    10 == 10 ; //true
    10 == '10'; //true
    10 === 10 ; //true
    10 === '10'; //false
```
## 三、函数
函数的定义如下所示：
```javascript
//方式一
function functionname(param1,param2...){
    //执行的代码
    //返回值不需要定于，如需返回直接return
}

//方式二
var functionname = function (param1,param2...) {
    //执行的代码
    //返回值不需要定于，如需返回直接return
}
```
## 四、对象
### 1.Array数组
定义方式入下：
```javascript
var 变量名 = [元素列表]//元素长度可变、类型可变
```
常用的属性和方法有：
```javascript
//属性：
arr.length //数组元素个数
//方法：
forEach(func)//遍历有值的元素，每次遍历调用func函数
push(元素列表)//添加元素到数组尾部
splice(start,num)//从start位置开始删除num个（包括start）
```
### 2.字符串
```javascript
    var 变量名 = 'xxx'
```
### 3.自定义对象
```javascript
var 对象名 = {
    属性名1: 属性值1,
    属性名2: 属性值2,
    属性名3: 属性值3,
    函数名称: function (形参列表) {
        //函数内容
    }
};
```
### 4.JSON对象
示例：
```javascript
var 变量名 = 
'{
  "name" : "Tom",
  "age" : 20,
  "gender" : "male",
  "list" : [e1,e2]
}';
```
前端获取时要将JSON字符串转化成JSON对象
```javascript
var object = JSON.parse(jsonstr) //字符串解析为对象
var jsonstr = JSON.stringify(obj) //对象转化为字符串
```
### 5.BOM (Browser Object Moudule)浏览器对象模型
允许JS与浏览器对话，将浏览器的各个组成部分封装成对象。
- Window ： 浏览器窗口
- Navigator ： 浏览器
- Screen ： 屏幕对象
- History ： 历史记录对象
- Location ：地址栏对象
#### Window
1. 属性：
    - history ：对History对象的只读引用
    - location
    - navigator
2. 方法：
    - alert():显示带有一段消息和一个确认按钮的警告框
    - confirm():显示带有确认和取消的对话框，返回true\false
    - setInterval():按照指定的周期(毫秒计)来调用函数或计算表达式
    - setTimeout():在指定的毫秒数后调用函数或计算表达式
#### Location
地址栏对象
- 属性：location.href = "xxx".设置或返回完整URL

### 6.DOM(Doucument Object Model 文档对象模型)
将标记语言的各个组成部分封装成对应的对象
- Document：整个文档对象
- Element ： 元素对象
- Attribute ： 属性对象
- Text ： 文本对象
- Comment ： 注释对象

## 五、事件
如按钮点击、按下键盘等、鼠标放在元素上等
### 1.事件绑定
1. 方式1：通过HTML标签中的事件属性进行绑定
```html
<input type="button" onclick="on()" value="按钮1">
//onclick 绑定函数on()

<script>
   function on(){
       alert('我被点击了');
   }
</script>

```
2. 方式2：通过DOM绑定
```html
<input type="button" id="btn" value="按钮2">

<script>
   document.getElementById('btn').onclick=function(){
       alert('我被点击了!');
   }
</script>
```
