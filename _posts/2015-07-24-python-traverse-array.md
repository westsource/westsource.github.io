---
layout:  post
title:  Python遍历数组的两种办法
date:  2015-07-24 22:01:00
---

Python遍历数组有两种办法，一种通过for in遍历数组，一种为先获得数组的长度，然后根据索引号遍历数组。

#第一种，通过for in遍历数组

colours = ["red","green","blue"]

for colour in colours:

    print(colour)

输出

> red

> green

> blue

#第二步，先获得数组的长度，然后根据索引号遍历数组

colours = ["red","green","blue"]

for i in range(0, len(colours)):

    print(colour[i])

> red

> green

> blue