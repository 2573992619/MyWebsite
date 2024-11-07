---
title: nodejs
date: 2023-11-29 19:33:46
tags: 后端
---

# <center> NodeJS 
## 一、file system 文件系统
### 1.导入
```javascript
const fs = require('fs');
```
### 2.写入文件
```javascript
fs.writeFile('./test.txt', 'this is a test file!', (err) => {
//如果写入失败，err为错误对象，否则err为null
    if (err) {
        console.log(err);
    } else {
        console.log('写入成功');
    }
});
```
### 3.同步与异步
```javascript
fs.writeFileSync('./test.txt', 'this is a test file!');
```
### 4.追加写入
```javascript
fs.appendFile('./test.txt', 'this is a test file!', (err) => {
    if (err) {
        console.log(err);
    } else {
        console.log('追加成功');
    }
});
```
### 5.流式写入
适用于写入需求较高的场景
```javascript
const fs = require('fs');
//创建写入流对象
const ws = fs.createWriteStream('./test.txt');

//write
ws.write("\r\n 流");
ws.write("\r\n 式");
ws.write("\r\n 写");
ws.write("\r\n 入");

//关闭通道(可选)
ws.close();
```
### 6.读取文件
- 一次性全部读取
```javascript
const fs = require('fs')
fs.readFile('./test.txt',
    (err, data) => {
    //data 接收读取的内容
        if (err) {
            console.log(err);            
        }
        console.log(data.toString());        
    }
);    
```
- 流式读取：
```javascript
const fs = require('fs')

//
const rs = fs.createReadStream('./test.txt');

rs.on('data', chunk =>{
    //每个chunk块64K
    console.log(chunk);
}
);
```
### 7.实现文件的复制
```javascript
const fs = require('fs');
//创建读取流
const rs = fs.createReadStream('./test.txt');
//创建写入流
const ws = fs.createWriteStream('./test1.txt');

//绑定data事件
rs.on('data',chunk =>{
    ws.write(chunk);
})
```
## 二、http模块
### 1.创建http服务端
```javascript
const http = require('http');
//创建服务器对象
const server = http.createServer((request,response)=>{
//两个参数分别对应请求报文、响应报文，该回调函数在服务端接收到http请求时执行
    response.end('hello http');//设置响应体并结束响应    
});

server.listen(9000,()=>{
    console.log('server start')
})
```
### 2.获取http请求报文
需要通过request对象获取
```javascript
const http = require('http');
//创建服务器对象
const server = http.createServer((request,response)=>{
//获取请求的方法request.method
    console.log(request.method);
    //获取请求的url request.url
    console.log(request.url);
    //获取http请求报文中的headers对象
    console.log(request.headers);

    response.end('hello http');//设置响应体并结束响应    
});

server.listen(9000,()=>{
    console.log('server start')
})
```
### 3.获取请求体
```javascript
const http = require('http');
//创建服务器对象
const server = http.createServer((request,response)=>{
    //1.声明一个变量用来承接请求体的数据
    let body = ''
    //2.绑定data事件
    request.on('data',chunk =>{
        body += chunk;
        })
    //3.绑定end事件
    request.on('end',()=>{
        console.log(body);
        response.end('hello http');//设置响应体并结束响应        
    })
});

server.listen(9000,()=>{
    console.log('server start')
})
```