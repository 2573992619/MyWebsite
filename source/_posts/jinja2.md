---
title: jinja2
date: 2024-02-23 15:31:45
tags: 模板渲染
---
# <center> jinja技术总结 </center>

## 1.简介
jinja是为Python 提供的一个功能齐全的模板引擎。 Jinja2提供了对Unicode 的完整支援


## 2.安装
```
pip install jinja2
```

## 3.基本用法

### 3.1 模板
jinja模板中使用{{
```
from jinja2 import Template
template = Template('Hello {{ name }}!')
```