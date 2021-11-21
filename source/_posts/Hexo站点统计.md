---
title: Hexo站点统计
toc: true
date: 2018-11-14 22:01:45
tags:
- hexo
- 不蒜子
- 统计数量
categories:
- Hexo自定义
---
## 问题
页脚的总访问人数和单页访问人数显示不正常。
## 解决方案
查找
```
<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
替换成
```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
即可  
## 参考资料
* [busuanzi.ibruce.info](http://busuanzi.ibruce.info/)
