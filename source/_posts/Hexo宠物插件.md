---
title: Hexo宠物插件
date: 2018-11-24 14:49:26
tags:
- hexo
- 宠物
categories:
- Hexo自定义
---
# 序
参考了[hexo 增添宠物](https://blog.csdn.net/qq_43020645/article/details/82794092)这篇文章，但是其内容有相应的遗漏，故写下该文章，以作补充。
# 完整步骤
在终端切换到你的博客的路径里，然后输入如下代码：
```
npm install -save hexo-helper-live2d
```
然后打开选择下面一个萌宠或萌妹子
```
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16

```
如果你选择的是live2d-widget-model-wanko
则需要在命令行执行
```
npm install -save live2d-widget-model-wanko
```
<font color=#FF0000>参考文章中没有以上这一步。</font>  
然后再在 hexo 的 _config.yml中添加参数：
```
live2d:
  enable: true
  scriptFrom: local
  model:
    use: live2d-widget-model-wanko
  display:
    position: right
    width: 140
    height: 260
  mobile:
    show: true
```
然后hexo clean ，hexo g ，hexo d 即可  
# 参考资料
* [hexo 增添宠物](https://blog.csdn.net/qq_43020645/article/details/82794092)
