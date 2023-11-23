---
title: Python-turtle
tags: []
id: '247'
categories:
  - - Python
abbrlink: a378bd8e
date: 2021-09-07 14:03:09
---

## Hello World

**画个五角星**

导入turtle模块

注意:1、文件名不能以 turtle.py 命名，否则会报错：AttributeError: partially initialized module 'turtle' has no attribute 'pencolor' (most likely due to a circular import)

2、是 True 不是 Ture

代码如下：

```python
import turtle as t
t.pencolor("red")
t.fillcolor("yellow")
t.begin_fill()
while True:
    t.forward(200)
    t.right(144)
    if abs(t.pos())<1:
        break

t.end_fill()
```

结果显示：

绘制同起点不同大小的5个五角星

```python
import turtle as t
def draw_fiveStars(leng):
    count =1
    while count <=5:
        t.forward(leng)
        t.right(144)
        count+=1
    leng+=10
    if leng <=100:
        draw_fiveStars(leng)
def main():
    t.penup()
    t.backward(100)
    t.pendown()
    t.pensize(2)
    t.pencolor('red')
    segment =50
    draw_fiveStars(segment)
    t.exitonclick()
if __name__== '__main__':
    main()
```
