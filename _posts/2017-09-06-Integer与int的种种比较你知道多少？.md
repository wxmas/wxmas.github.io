---
layout: post
title: Integer与int的种种比较你知道多少？
date: 2017-09-06 21:33
categories:
- Java
tags: 
- Java
---

>原文出处：[Integer与int的种种比较你知道多少？](http://www.cnblogs.com/liuling/archive/2013/05/05/intAndInteger.html)

如果面试官问Integer与int的区别：估计大多数人只会说道两点，Ingeter是int的包装类，int的初值为0，Ingeter的初值为null。但是如果面试官再问一下Integer i = 1;int ii = 1; i==ii为true还是为false？估计就有一部分人答不出来了，如果再问一下其他的，估计更多的人会头脑一片混乱。所以我对它们进行了总结，希望对大家有帮助。

首先看代码：

{% highlight Java linenos %}
 1 package com.test;
 2 /**
 3  * 
 4  * @author 刘玲
 5  *
 6  */
 7 public class TestInteger {
 8 
 9     /**
10      * @param args
11      */
12     public static void main(String[] args) {
13         int i = 128;
14         Integer i2 = 128;
15         Integer i3 = new Integer(128);
16         //Integer会自动拆箱为int，所以为true
17         System.out.println(i == i2);
18         System.out.println(i == i3);
19         System.out.println("**************");
20         Integer i5 = 127;//java在编译的时候,被翻译成-> Integer i5 = Integer.valueOf(127);
21         Integer i6 = 127;
22         System.out.println(i5 == i6);//true
23         /*Integer i5 = 128;
24         Integer i6 = 128;
25         System.out.println(i5 == i6);//false
26 */        Integer ii5 = new Integer(127);
27         System.out.println(i5 == ii5); //false
28         Integer i7 = new Integer(128);
29         Integer i8 = new Integer(123);
30         System.out.println(i7 == i8);  //false
31     }
{% endhighlight %}

首先，17行和18行输出结果都为true,因为Integer和int比都会自动拆箱（jdk1.5以上）。

22行的结果为true,而25行则为false,很多人都不动为什么。其实java在编译Integer i5 = 127的时候,被翻译成-> Integer i5 = Integer.valueOf(127);所以关键就是看valueOf()函数了。只要看看valueOf()函数的源码就会明白了。JDK源码的valueOf函数式这样的：

{% highlight Java linenos %}
1 public static Integer valueOf(int i) {
2         assert IntegerCache.high >= 127;
3         if (i >= IntegerCache.low && i <= IntegerCache.high)
4             return IntegerCache.cache[i + (-IntegerCache.low)];
5         return new Integer(i);
6     }
{% endhighlight %}

看一下源码大家都会明白，对于-128到127之间的数，会进行缓存，Integer i5 = 127时，会将127进行缓存，下次再写Integer i6 = 127时，就会直接从缓存中取，就不会new了。所以22行的结果为true,而25行为false。

对于27行和30行，因为对象不一样，所以为false。

我对于以上的情况总结如下：

  ①无论如何，Integer与new Integer不会相等。不会经历拆箱过程，i3的引用指向堆，而i4指向专门存放他的内存（常量池），他们的内存地址不一样，所以为false
  ②两个都是非new出来的Integer，如果数在-128到127之间，则是true,否则为false
  java在编译Integer i2 = 128的时候,被翻译成-> Integer i2 = Integer.valueOf(128);而valueOf()函数会对-128到127之间的数进行缓存
  ③两个都是new出来的,都为false
  ④int和integer(无论new否)比，都为true，因为会把Integer自动拆箱为int再去比

如果大家觉得有什么不对的地方，欢迎指示。