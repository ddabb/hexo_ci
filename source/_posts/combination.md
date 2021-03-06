---
title: m个球放到n个箱子中几种解法  
toc: true
top: 100
mathjax: false
date: 2018-11-29 22:23:23
tags:
- 排列组合
- 不同颜色
categories:
- 逻辑思维
---
# 序

该文的诞生源于一个问题:**斗地主有多少种牌型组合?**  
众所周知,斗地主共54张牌分别给三个人20,17,17张牌，因为方块3和梅花3在斗地主中无区别，转化一下问题就是13种不同的颜色（不包括黑白）的球各4个,黑球，白球各1个,放到容积分别为20,17,17的三个箱子中有多少种方法？  
第一想法是想到采取排列组合的方式去解，但是多久没有做排列组合的习题，我发现**3红3黄3蓝1黑1白共11个球放到容积为5,3,3的三个箱子中,不考虑顺序总共有多少种方法？**这个问题我都计算困难,该文简单讲一下该问题的两种解法。

# 正文

## 方案1：全排列

### 方案1.1 考虑黑白球

假设11个球的颜色全不相同,则所有排列顺序是1~11的全排列是**11!=39916800**。  
再假设1,4,7为红球,2,5,8为黄球,3,6,9为蓝球，10为黑球,11为白球。  
<font color=#FF0000>也可以1,2,3为红球,4,5,6为黄球,7,8,9为蓝球，10为白球,11为黑球。给球编号是为了计算全排列方便。</font>
以全排列元素中的一个**链表X**(全排列的取值之一) **{9,8,3,5,6,4,11,7,1,2,10}**为例,  
可以分成 **{9,8,3,5,6}**和 **{4,11,7}**和 **{1,2,10}**三组。  
将每组数据中≤9的对3取余,结果变成了A:**{0,2,0,2,0}**和B:**{1,1,11}**和C:**{1,2,10}**三组数据。
∵ 在箱子中 **{4,11,7}**和 **{4,7,11}**是等价的。  
∴ A、B、C三组都需要排序，排序后为{0,0,0,2,2}，{1,1,11},{1,2,10}  
∴**{9,8,3,5,6,4,11,7,1,2,10}** 表示A箱子有3个蓝球，2个黄球；B箱子有2个红球,1个白球；C箱子有1个红球,1个黄球,1个黑球。  
∴链表X的表达式是0,0,0,2,2,1,1,11,1,2,10  
∴将39916800链表的表达式去重就可以算出序中提到的问题的解为355个。  

### 方案1.2 先不考虑黑白球

∵黑球的可能位置只有3种,白球的可能位置也只有3种。  
即黑白球都在A箱,都在B箱,都在C箱,分别在AC箱,分别在BC箱，分别在AB箱。  
所有组合情况如下  
![黑白球位置组合](nine.png)
这样就只需要计算9的全排列**9!=362880**次数据。
计算方式如方案1.1,再次不再赘述。  
结果依旧为355。  
具体计算代码请参考[方案一代码](https://github.com/ddabb/permutations/blob/master/Program.cs)

## 方案2：笛卡尔积

在不考虑各箱子容积的前提下；  
每个球都有3个箱子可以选择，则11个球的位置有**3的11次方=177147**中可能性。  
若箱子A,B,C编号为{1,2,3}。大小分别为{5,3,3}。  
则第一个球和第二球的箱子可能组合是{1,2,3}和{1,2,3}的笛卡尔积  

```
{   {1,1},{1,2},{1,3},   {2,1},{2,2},{2,3},   {3,1},{3,2},{3,3}  }  
```

将每个球所有的可能性做笛卡尔积之后，会得到一个177147个**元素个数为11的链表L**的集合S。  
若链表L中有5个元素1，有3个元素2，有3个元素3。则链表L符合球进入箱子的逻辑。否则不符合球进入箱子的逻辑。  
假若链表L为{1,2,3,2,3,2,1,1,1,1,3}且11个球的顺序为**红,红,红,黄,黄,黄,蓝,蓝,蓝,黑,白**
则箱子内的颜色情况如下:  
A箱：红蓝蓝蓝黑  
B箱：红黄蓝  
C箱：红黄白  
所以链表L的表示式为 A-红蓝蓝蓝黑 B-红黄蓝 C-红黄白
将所有符合进箱逻辑的链表的表达式算出来去重,算出来**结果依旧为355。**  
具体计算代码请参考[方案二代码](https://github.com/ddabb/combination)  
>**<font color=#FF0000>注意：相同颜色的球的顺序应该连在一起，否则箱子内的球需要再做一次排序去重。</font>**  

# 总结

该文是对排列组合知识的一个补充。如果有更好的解决m个不同颜色的球放到n个箱子中的更好的解法，欢迎留言或者直接联系我。
如果能有算出斗地主有多少种牌型组合的方法，则更希望能联系我。  
方案2在箱子不够多,球不够多的情况下,的确是不错的一个计算方案。

---
# 参考资料 

* [C# 用Linq的方式实现组合和笛卡尔积（支持泛型T）](https://www.cnblogs.com/localhost2016/p/8668355.html)
* [终于有个高效率的排列组合算法](https://blog.csdn.net/MaybeHelios/article/details/759315)
* [欢乐斗地主出牌方式统计](https://www.60points.com/Fight-the-Landlord-Card-Type-Aanalysis/)