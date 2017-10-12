---
layout: post
title: 零值比较--BOOL，int，float，指针变量与零值比较的if语句
date: 2017-10-12 18:51
categories:
- C
- C++
tags: 
- C
- C++
---

这是程序员面试的一道常见题，也是个C++基础问题。若只在大学里看过几本基础的编程入门书，看见这道题可能会觉得奇怪，不就是和0比较吗，直接拿出来比就是了，其实非也。下文引自google搜索结果，出处不详，高手可以无视，菜菜留下，记得做好笔记。
首先给个提示：题目中要求的是零值比较，而非与`0`进行比较，在C++里“零值”的范围可就大了，可以是`0`，`0.0`，`FALSE`或者“空指针”。

## int型变量`n`与“零值”比较的`if`语句 ##

{% highlight C linenos %}
if ( n == 0 )
if ( n != 0 )
如下写法均属不良风格:
if ( n )  // 会让人误解 n 是布尔变量
if ( !n  )
{% endhighlight %}

## float x 与“零值”比较的 if 语句 ##

千万要留意，无论是`float`还是`double`类型的变量，都有精度限制，都不可以用“==”或“！=”与任何数字比较，应该设法转化成“>=”或“<=”形式。(为什么？文章之后有详细的讨论，可参考)
假设浮点变量的名字为`x`，应当将

**if (x == 0.0)  // 隐含错误的比较**
**转化为**
**if ((x>=-EPSINON) && (x<=EPSINON))**
其中EPSINON 是允许的误差（即精度）。

标准答案示例：

{% highlight C linenos %}
const float EPSINON = 0.00001;
if ((x >= - EPSINON) && (x <= EPSINON)
如下是错误的写法。
if (x == 0.0) 
if (x != 0.0)
{% endhighlight %}

## char *p 与“零值”比较的 if 语句 ##

{% highlight C linenos %}
if (p == NULL)
if (p != NULL)
如下写法均属不良风格。
if (p == 0)        // 容易让人误解p是整型变量
if (p != 0) 
if (p)                // 容易让人误解p是bool型变量
if (!p)
{% endhighlight %}
