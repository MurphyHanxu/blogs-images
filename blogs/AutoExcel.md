+++

author = "Murphy"
title = "Python(xlrd,xlwt)对于excel的操作"
date = "2022-08-01"

description = ""
tags = [
    "Python",
]

categories = [
    "CS",
   ]

math=true

toc=true

+++

总结xlrd，xlwt，pandas对于excel的常用操作。

| 库   | .xls | .xlsx | 读取 | 消耗时间（以10MB.xlsx文件为例） | 写入 | 修改 | 保存 | 样式调整 |
| ---- | ---- | ----- | ---- | ------------------------------- | ---- | ---- | ---- | -------- |
| xlrd | √    | √     | √    | 12.38s                          | ×    | ×    | ×    | ×        |
| xlwt | √    | ×     | ×    | -                               | √    | √    | √    | √        |

<!--more-->

## xlrd

##### 1.导入模块

```python
import xlrd
```



##### 2.打开文件

```python
path = r"example.xlsx
xlrd.open_workbook(path)
```

```python
path = r"example.xlsx
wb = xlrd.open_workbook(path)
print(wb)
#  <xlrd.book.Book object at 0x000001F905A33DC0>
```



##### 3.获取所有表名

```python
wb.sheet_names()
```

```python
sheet_names_list = wb.sheet_names()  # list
print(sheet_names_list)
#  ['Sheet1', 'Sheet2', 'Sheet3']
```



##### 4.选定sheet表

```python
wb.sheet_by_index(索引)
wb.sheet_by_name("sheet表名")
```

```python
#  方式1：索引顺序获取
sheet_1 = wb.sheet_by_index(0)

#  方式2:名称获取
sheet_2 = wb.sheet_by_name("Sheet1")
```



##### 5.对sheet表的行操作

```python
#  1.sheet.nrows:获取sheet表的有效行数
print(sheet_1.nrows)

#  2.sheet.row(1)返回由该行中所有单元格对象组成的列表，列表中的内容是键值对形式
print(sheet_1.row(1))
#  [列名1:"值1", 列名2:"值2"...]

#  3.sheet.row_values(rowx, start_colx=0, end_colx=None)：返回指定行的所有单元格数值组成的列表
print(sheet_1.row_values(1))
#  ["值1", "值2"...]

#  4.sheet.row_slice(rowx, start_col=0, end_col=None):可以获得一个行中指定的列的切片
print(sheet_1.row_slice(1, 0, 3))
#  [列名1:"值1", 列名2:"值2", 列名3:"值3"]

#  5.sheet.row_len(rox):返回指定行的有效长度
print(sheet_1.row_len(1))

#  6.sheet.get_rows():获取表中所有行构成一个迭代器对象
for row in list(sheet_1.get_rows())[0:15]:
    print(row)
#  使用list()的好处是对获取的所有行返回的列表进行切片操作
```



##### 6.对sheet表的列操作

```python
#  1.sheet.ncols:返回指定sheet表的有效列数
print(sheet_1.ncols)

#  2.sheet.col(1):返回由该列中所有的单元格对象组成的列表，列表内是键值对
print(sheet_1.col(1))
#  ["行名1":"值1", "行名2":"值2"...] 若无行名，则默认行名为empty

#  3.sheet.col_values(colx, start_colx=0, end_colx=None)：返回指定列的所有单元格数值组成的列表
print(sheet_1.col_values(0))  # 带列名
print(sheet_1.col_values(0, 1))  # 不带列名
#  ["列名", "值1", "值2"...]
#  [值1", "值2"...]

#  4.sheet.col_slice(colx, start_rowx=0, end_rowx=None)：返回由该列中所有的单元格对象组成的列表
print(sheet1.col_slice(0, 0, 3))
#  [列名:"值1", type:"值2",type:"值3"]

#  5.sheet.col_types(colx, start_rowx=0, end_rowx=None)：返回由该列中所有单元格的数据类型组成的列表
print(sheet1.col_types(0))
#  0-空，1-文本，2-数字，3-日期，4-布尔，5-array
```



##### 7.对sheet表的单元格操作

```python
#  1.sheet.cell(rowx,colx)：返回单元格对象
#  键值对

#  2.sheet.cell_type(rowx,colx)：返回单元格中的数据类型
#  0-空，1-文本，2-数字，3-日期，4-布尔，5-array

#  3.sheet.cell_value(rowx,colx)：返回单元格中的数据
```





## xlwt

##### 1.导入模块

```python
import xlwt
```



##### 2.写入单个数据

```python
#  1.创建Excel表对象
wb = xlwt.Workbook(encoding='utf8')

#  2.新建sheet表
sheet = workbook.add_sheet('Sheet1')

#  3.写入数据到指定单元格
sheet.write(0, 0, "python")

# 4. 保存文件分两种格式
wb.save('test.xls')
wb.save('test.xlsx')
```



##### 3.写入多个数据

```python
data_list = [('小白', '20', '男'), ('小黑', '21', '男'), ('小红', '20', '女')]

#  1.创建Excel表对象
wb = xlwt.Workbook(encoding='utf8')

#  2.新建sheet表
sheet = workbook.add_sheet('Sheet1')

#  3.自定义列名
col1 = ('姓名', '年龄', '性别')

#  4.将列属性元组col写进sheet表单中第一行
for i in range(0, len(col1)):
    wb.write(0, i, col1[i])
    
#  5.将数据写进sheet表单中
for i in range(0, len(data_list)):
    data = data_list[i]
    for j in range(0, len(col1)):
        worksheet.write(i + 1, j, data[j])
        
#  6.保存文件分两种格式
wb.save('test.xls')
```



##### 4.设置列宽

```python
for c in range(len(col1)):
    sheet.col(c).width = 256 * 24
```



##### 5.设置行高

```python
#  1.将数据写进sheet表单中
for i in range(0, len(data_list)):
    data = data_list[i]
    for j in range(0, len(col1)):
        sheet.write(i + 1, j, data[j])
        #  2.设置行高
        sheet.row(i + 1).height_mismatch = True
        sheet.row(i + 1).height = 1600
```



##### 6.设置单元格风格

```python
def body_style():
    #  1.创建一个样式对象，初始化样式 style
    style = xlwt.XFStyle()  # Create Style对象

    #  2.字体风格设置
    font = xlwt.Font()  # Create Font对象
    font.name = "SimSun"  # 设置字体类型，宋体
    font.colour_index = 4  # 设置字体颜色
    font.height = 20 * 12  # 字体大小，12为字号，20为衡量单位
    font.bont = True  # 设置字体加粗
    font.underline = True  # 下划线
    font.italic = True  # 斜体字

    #  3.背景设置
    pattern = xlwt.Pattern()
    pattern.pattern = xlwt.Pattern.SOLID_PATTERN  # May be: NO_PATTERN, SOLID_PATTERN, or 0x00 through 0x12
    pattern.pattern_fore_colour = 4  # 给背景颜色赋值

    #  4.边框设置
    borders = xlwt.Borders()  # 创建边框对象，    # .DASHED：虚线；.NO_LINE：没有
    #  上下左右都添加边框
    borders.left = 1
    borders.right = 1
    borders.top = 1
    borders.bottom = 1
    #  设置边框颜色
    borders.left_colour = 2
    borders.right_colour = 2
    borders.top_colour = 2
    borders.bottom_colour = 2

    #  5.位置设置
    alignment = xlwt.Alignment()
    alignment.horz = 1  # 设置水平位置，0是左对齐，1是居中，2是右对齐
    alignment.wrap = 1  # 设置自动换行

    #  6.设置好之后，全部都加到style上
    style.alignment = alignment
    style.font = font
    style.borders = borders
    return style
```

