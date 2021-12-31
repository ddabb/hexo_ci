---
title: python画爱心
toc: true
top: 100
mathjax: false
date: 2018-11-26 20:57:46
tags:
- python
- 画爱心
- hello world
categories:
- 代码世界
- python
---
# 序
该文算是python版的"Hello world"初探，故作相关的记录。
环境准备:Visual Studio 2017,python

# 正文
## 爱心一
```
import numpy as np
import matplotlib.pyplot as plt

x_coords = np.linspace(- 100, 100, 500)
y_coords = np.linspace(- 100, 100, 500)
points = []

for y in y_coords:
 for x in x_coords:
    if((x* 0.03)** 2+(y* 0.03)** 2- 1)** 3-(x* 0.03)** 2*(y* 0.03)** 3<= 0:
       points.append({ "x": x, "y": y})

heart_x = list(map( lambda point: point[ "x"], points))
heart_y = list(map( lambda point: point[ "y"], points))

plt.scatter(heart_x, heart_y, s= 10, alpha= 0.5)
plt.show()
```
效果如图
![爱心1](heart1.png)  
## 爱心二
```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
 
import turtle
import time
 
# 画心形圆弧
def hart_arc():
    for i in range(200):
        turtle.right(1)
        turtle.forward(2)
 
def move_pen_position(x, y):
    turtle.hideturtle()     # 隐藏画笔（先）
    turtle.up()     # 提笔
    turtle.goto(x, y)    # 移动画笔到指定起始坐标（窗口中心为0,0）
    turtle.down()   # 下笔
    turtle.showturtle()     # 显示画笔
 
 
#love = input("请输入表白话语，默认为‘I Love You’：")
#signature = input("请签署你的大名，不填写默认不显示：")

signature= ''
love = 'I Love You'

if love == '':
    love = 'I Love You'
 
# 初始化
turtle.setup(width=800, height=500)     # 窗口（画布）大小
turtle.color('red', 'pink')     # 画笔颜色
turtle.pensize(3)       # 画笔粗细
turtle.speed(1)     # 描绘速度
# 初始化画笔起始坐标
move_pen_position(x=0,y=-180)   # 移动画笔位置
turtle.left(140)    # 向左旋转140度
 
turtle.begin_fill()     # 标记背景填充位置
 
# 画心形直线（ 左下方 ）
turtle.forward(224)    # 向前移动画笔，长度为224
# 画爱心圆弧
hart_arc()      # 左侧圆弧
turtle.left(120)    # 调整画笔角度
hart_arc()      # 右侧圆弧
# 画心形直线（ 右下方 ）
turtle.forward(224)
 
turtle.end_fill()       # 标记背景填充结束位置
 
# 在心形中写上表白话语
move_pen_position(0,0)      # 表白语位置
turtle.hideturtle()     # 隐藏画笔
turtle.color('#CD5C5C', 'pink')      # 字体颜色
# font:设定字体、尺寸（电脑下存在的字体都可设置）  align:中心对齐
turtle.write(love, font=('Arial', 30, 'bold'), align="center")
 
# 签写署名
if signature != '':
    turtle.color('red', 'pink')
    time.sleep(2)
    move_pen_position(180, -180)
    turtle.hideturtle()  # 隐藏画笔
    turtle.write(signature, font=('Arial', 20), align="center")
 
# 点击窗口关闭程序
window = turtle.Screen()
window.exitonclick()
```
效果如下图
![爱心2](heart2.png)  
## 爱心三
```
import matplotlib.pyplot as plt
from matplotlib.font_manager import FontProperties
import numpy as np

x=np.linspace(-2,2,200)
y1 =np.sqrt(1-np.square(np.fabs(x)-1))
y2 =np.arccos(1-np.fabs(x))-np.pi

plt.plot(x,y1,'r',x,y2,'r')
plt.axis([-2.5,2.5,-3.5,1.5])

plt.title('hello world of python,copy from @andrew',fontsize=16)
plt.show()
```
效果如下图
![爱心3](heart3.png)  

# 遇到问题
**在visual studio 2017中命令行执行 pip install numpy 无效**  
在nuget程序包管理器控制台执行即可  
**pip install pyinstaller之后生成的exe无法执行**
在我电脑上的路径是**C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python36_64\Scripts**,
把run.py拷贝到该目录,执行**pyinstaller -F -W p . run.py**解决问题

# 总结
![暴击](hurt.png)

---
# 参考资料
* [520情人节，用python画个爱心送给你的那个她！](https://cloud.tencent.com/developer/news/216095)
* [七夕将至，教你用Python绘制爱心，优雅的表白520~](https://www.taitaiblog.com/1314.html)
