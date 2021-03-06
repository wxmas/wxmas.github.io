---
layout: post
title: MIT HAKMEM算法分析
date: 2017-08-24 14:30:00 
categories:
- C
- C++
- 算法优化
tags: 
- C
- C++
- 算法优化
---


>原文出处：[MIT HAKMEM算法分析](http://blog.csdn.net/msquare/article/details/4536388)
>其他算法参考:[求一个整数的二进制中1的个数](http://blog.csdn.net/wangjun_1218/article/details/4464129)


今天学习了一种很有趣的BitCount算法——MIT HAKMEM算法。

本文中^表示乘方

# 问题描述 #

问题需求：计算32位整型数中的'1'的个数

# 思路分析 #

1.整型数 i 的数值，实际上就是各位乘以权重——也就是一个以2为底的多项式：

`i = A0*2^0+A1*2^1+A2*2^2+...`

因此，要求1的位数，实际上只要将各位消权：

`i = A0+A1+A2+...`

所得的系数和就是'1'的个数。

2.对任何自然数n的N次幂，用n-1取模得数为1，证明如下：

若 n^(k-1) % (n-1) = 1 成立

则 n^k % (n-1) = ((n-1)*n^(k-1) + n^(k-1)) % (n-1) = 0 + n^(k-1) % (n-1)  = 1 也成立

又有 n^(1-1) % (n-1) = 1

故对任意非负整数N, n^N %(n-1)=1

3.因此，对一个系数为{Ai}的以n为底的多项式P(N)， P(N)%(n-1) = (sum({Ai})) % (n-1) ;

如果能保证sum({Ai}) < (n-1)，则 P(N)%(n-1) = (sum({Ai}))  ，也就是说，此时只要用n-1对多项式取模，就可以完成消权，得到系数和。

于是，问题转化为，将以2为底的多项式转化为以n为底的多项式，其中n要足够大，使得n-1 > sum({Ai})恒成立。

32位整型数中Ai=0或1，sum({Ai})<=32。n-1 > 32 ，n需要大于33。

因此取n=2^6=64>33作为新多项式的底。

4.将32位二进制数的每6位作为一个单位，看作以64为底的多项式:

` i = t0*64^0 + t1*64^1 + t2*64^2 + t3*64^3 + ... `

各项的系数ti就是每6位2进制数的值。

这样，只要通过运算，将各个单位中的6位数变为这6位中含有的'1'的个数，再用63取模，就可以得到所求的总的'1'的个数。

5.取其中任意一项的6位数ti进行考虑，最简单的方法显然是对每次对1位进行mask然后相加，即
	
```C
(ti>>5)&(000001) + (ti&>>4)(000001) + (ti>>3)&(000001) + (ti>>2)&(000001) + (ti>>1)&(000001) + ti&(000001)
```

其中000001为2进制数

由于ti最多含有6个1，因此上式最大值为000110，绝不会产生溢出，所以上式中的操作完全可以直接对整型数 i 进行套用，操作过程中，t0～t6将并行地完成上式的运算。

注意：不能将&运算提取出来先+后&，想想为什么。

# 实现代码(1) #

因此，bit count的实现代码如下：

{% highlight C++ linenos %}
int bitcount(unsigned int n)  
{  
    unsigned int tmp;  
  
    tmp = (n &010101010101)  
     +((n>>1)&010101010101)  
     +((n>>2)&010101010101)  
     +((n>>3)&010101010101)  
     +((n>>4)&010101010101)  
     +((n>>5)&010101010101);  
  
    return (tmp%63);  
}  
{% endhighlight %}

# 实现代码(2) #

但MIT HAKMEM最终的算法要比上面的代码更加简单一些。

为什么说上面的式子中不能先把(ti>>k)都先提取出来相加，然后再进行&运算呢？

因为用&(000001)进行MASK后，产生的有效位只有1位，只要6位数中的'1'个数超过1位，那么在"先加"的过程中，得数就会从最低位中向上溢出。

但是我们注意到，6位数中最多只有6个'1'，也就是000110，只需要3位有效位。上面的式子实际上是以1位为单位提取出'1'的个数再相加求和求出6位中'1'的总个数的，所以用的是&(000001)。如果以3位为单位算出'1'的个数再进行相加的话，那么就完全可以先加后MASK。算法如下：

`tmp = (ti>>2)&(001001) + (ti>>1)&(001001) + ti&(001001)`

`(tmp + tmp>>3)&(000111)`

C代码：

{% highlight C linenos %}
int bitcount(unsigned int n)  
{  
    unsigned int tmp;  
  
    tmp = (n &011111111111)  
     +((n>>1)&011111111111)  
     +((n>>2)&011111111111);  
       
    tmp = (tmp + (tmp>>3)) &030707070707;  
  
    return (tmp%63);  
}  
{% endhighlight %}

注：代码中是使用8进制数进行MASK的，11位8进制数为33位2进制数，多出一位，因此第一位八进制数会把最高位舍去(7->3)以免超出int长度。

从第一个版本到第二个实际上是一个“提取公因式”的过程。用1组+, >>, &运算代替了3组。并且已经提取了"最大公因式"。然而这仍然不是最终的MIT HAKMEM算法，不过已经非常接近了，看看代码吧。

# 最终代码 #

MIT HAKMEM算法：

{% highlight C linenos %}
int bitcount(unsigned int n)  
{  
    unsigned int tmp;  
  
    tmp = n  
        - ((n >> 1) & 033333333333)  
        - ((n >> 2) & 011111111111);  
  
    tmp = (tmp + (tmp >> 3)) & 030707070707  
  
    return (tmp%63);  
}  
{% endhighlight %}

又减少了一组+, >>, &运算。被优化的是3位2进制数“组”内的计算。再回到多项式，一个3位2进制数是4a+2b+c，我们想要求的是a
+b+c，n>>1的结果是2a+b，n>>2的结果是a。

于是： (4a+2b+c) - (2a+b) - (a) = a + b + c

中间的MASK是为了屏蔽"组间""串扰"，即屏蔽掉从左边组的低位移动过来的数。

# 总结 #

第一次自己写算法分析，感觉收获还是蛮大的。

看算法的时候都是看这个算法是如何工作的，写的过程则是去模拟发现者的思路，看这个算法是如何被推导出来的，把思路反过来又重新梳理了一遍。

最大的感慨是数学对于算法的重要性。这个算法最开始的思路就是多项式消权，并且贯穿了整个算法推导和优化的过程。而第二步的必要条件则是对取模和幂运算关系的了解。优化的第一步用到了“提取公因式”思想，第二步则回归到了多项式的基本运算。

其中除了取模和幂运算的知识不算太基础以外，其他无一例外都是初中以内的数学知识与思想。特别是对2进制数本质的理解，所有人都清楚如何求得一个二进制整型数的数值，但是却很少有人对其多项式本质始终保持着清晰的认识。

就是这些小得不能再小，基础得不能再基础的地方，决定了我们的水平。
