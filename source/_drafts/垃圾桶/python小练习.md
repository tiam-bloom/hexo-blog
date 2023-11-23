---
title: Python小练习
cover: >-
  http://qiniu.yujing.fit/image/nice/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201216212325.jpg
tags: []
id: '305'
categories:
  - - Python
abbrlink: 43ebbc9e
date: 2021-09-16 18:00:37
---

问:进步一天+0.02，退步一天减0.01，不退不进始终为1，那么一年中进步多少天，退步多少天，与全年每天都不进不退最接近。

思路：

根据问题可得到两个方程：

① 一年中进步x天，退步(365-x)天的收获为：y=(1.02^x)\*(0.99^(365-x))

② 全年每天都不进不退的收获为: y=1

画出函数图像，交叉点为最接近的，但由于天数只能为整数，所以不能取交叉点。

函数图像地址：[https://www.desmos.com/calculator/m2nm4xh78d](https://www.desmos.com/calculator/m2nm4xh78d)

![](http://47.101.172.219/wp-content/uploads/2021/09/desmos-graph.png)

放大：

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-48-1024x443.png)

代码如下：

```python
from math import *

min = 10
for i in range(1,366):  # 进步i天
    A = fabs(pow((1 + 0.02), i) * pow((1 - 0.01), (365 - i)) - 1)
    if A < min:
        min = A
        x = i
        y = 365 - i
print("当进步%d天,退步%d天时,最接近" % (x, y))
print("最接近的值为{}".format(pow((1 + 0.02), x) * pow((1 - 0.01), y)))
print(min)
```

输**出结果：**

当进步123天,退步242天时,最接近  
最接近的值为1.003548160754847  
0.003548160754847096