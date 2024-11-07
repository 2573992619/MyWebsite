---
title: Ajax
date: 2023-11-21 17:56:32
tags: 前端
---
# <center>Ajax
## 一、简介
- Asynchronous Javascript And XML 。[官方文档](https://www.w3school.com.cn/js/js_ajax_intro.asp)
- 作用：
    - 数据交换：给服务器发请求，获取响应数据
    - 异步交互：在**不加载整个页面**的情况下，与服务器交互并**更新部分页面**
![工作流程](https://www.w3school.com.cn/i/ajax.gif)
## 二、原生Ajax步骤
1. 准备数据地址
2. 创建XMLHttpRequest对象，用于和服务器交换数据
3. 向服务器发送请求，获取响应数据
```html
<!--官方示例如下-->
Function loadDoc()
function loadDoc() {
<!--创建XMLHttpRequest对象-->
  var xhttp = new XMLHttpRequest();

  xhttp.onreadystatechange = function() {
    <!--判断是否成功响应-->
    if (this.readyState == 4 && this.status == 200) {
    <!--获取数据-->
     document.getElementById("demo").innerHTML = this.responseText;
    }
  };

  xhttp.open("GET", "ajax_info.txt", true);
<!--发送异步请求，成功响应会触发上方的函数-->
  xhttp.send();
```
## 三、Axios
对原生Ajax进行了封装，简化书写，快速开发。![官方文档](https://www.axios-http.cn/docs/intro)

## <center>未完待续