---
title: openpyxl
date: 2024-02-08 18:51:51
tags: 数据处理
---
# <center> OpenPyxl技术总结
## 一、简介
openpyxl是python用来操作Excel的库，几乎可以实现所有excel的功能
## 二、安装
可以直接pip安装
```shell
    pip install openpyxl
```
## 三、基本操作
### 1. 创建一个工作簿
工作簿在openpyxl中是WorkBook类的一个实例，可以通过openpyxl.load_workbook()方法来加载一个已存在的Excel文件，或者通过openpyxl.Workbook()方法来创建一个新的工作簿
```python
from openpyxl import Workbook
# 1.加载已存在的Excel文件，IO参数为文件路径
wb = openpyxl.load_workbook('example.xlsx')

# 2.新建
wb = Workbook()
```

### 2. 获取工作表

工作表在openpyxl中是Worksheet类的一个实例，可以通过wb.active来获取当前活动的工作表，也可以通过wb['sheet_name']来获取指定名称的工作表
```python
#获取当前活动的工作表
ws = wb.active

#
ws = wb['sheet_name']

# 创建新的工作表,表名为sheet_name
ws = wb.create_sheet('sheet_name')
```

### 3. 读取单元格数据

读取数据可以通过ws['A1']来获取A1单元格的数据，也可以通过ws.cell(row=1,column=1)来获取A1单元格的数据
```python
# 读取A1单元格的数据
data = ws['A1'].value

# 读取A1单元格的数据
data = wb.active.cell(row=1,column=1).value
```

### 4. 写入单元格数据

写入数据可以通过ws['A1'] = 'hello'来将hello写入A1单元格，也可以通过ws.cell(row=1,column=1).value = 'hello'来将hello写入A1单元格
```python
# 写入数据
ws['A1'] = 'hello'

# 写入数据
ws.cell(row=1,column=1).value = 'hello'
```

### 5. 保存工作簿

保存工作簿可以通过wb.save('example.xlsx')来保存工作簿，或者通过wb.close()来关闭工作簿
