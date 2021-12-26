---
title: 用Dictionary替换switch case的注意事项
toc: true
top: 100
mathjax: false
date: 2019-01-21 23:40:18
tags:
- 重构
categories:
- 代码世界
---
# 序
最近试图重构一段现做现卖的祖传代码,结果改完之后,性能急速下降,下面给出示意代码的截图,以便提醒自己工作需要更加认真和细心。  

# 示例代码

## 错误的重构

![差异对比](差异对比.png)
Dictionary的执行时间竟然是Switch case的四倍以上？原因是啥？我们来看一下各个动物的构造函数
![构造函数](构造函数.png)
即生成Dictionary的每一个键值对的值的时候,都实例化了一个Animal的子类,每个子类的实例化都等待了十秒钟,总实例化就耗费了40秒钟。

## 正确的重构

![正确重构](正确重构.png)
先获取对象的type,然后通过Activator.CreateInstance(type)创建对象。

# 总结

switch case转换成dictionary算得上是一种重构,起到了**减少代码量，提高可维护性的效果。**  
这次我的失误也算是明白了一个深刻的道理,所有的重构需要建立在完整的测试机制的前提下，否则可能会造成严重的损失。
最后一句箴言<font color=#FF0000>如无必要，勿动祖传代码</font>
