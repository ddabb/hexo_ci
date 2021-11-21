---
title: 第一个AI应用
toc: true
date: 2018-11-11 19:25:27
tags:
 - python
 - 手写识别
 - 原创
categories:
 - IT经验
 - 人工智能
---
# 序

我的第一个AI应用是参考[实战：从0搭建完整 AI 开发环境写出第一个 AI 应用](https://cloud.tencent.com/developer/article/1348205)和[AI应用开发实战 - 从零开始配置环境](http://www.cnblogs.com/ms-uap/p/9123033.html)两篇文章进行的实施,故以该文做出相应的补充。

# 遇到的问题

## 'python' 不是内部或外部变量
命令行执行**python --version**提示'python' 不是内部或外部变量，在命令行中输入
```
set PATH="C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python36_64";%PATH%
```
之后,再输入
```
python --version
```
输出版本为3.6.5
但是关闭命令行之后，再输入python --version时,依旧提示 **'python' 不是内部或外部变量**。  
故在计算机→属性右键→高级系统设置→环境变量→选择**PATH**→点击新建→将**C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python36_64**添加→确定，之后再运行python --version，显示正常。

## saved_model.pb路径不对
单独参考第一篇文章进行配置时，发现没有**samples-for-ai\export\saved_model.pb**这个路径。  
原因是没有启动**examples\tensorflow\TensorflowExamples.sln**这个解决方案，将MNIST项目设置为启动项目并运行,则会有samples-for-ai\export\saved_model.pb这个文件了。

## 命令行无响应

安装**scipy-1.1.0**和**mxnet_cu90-1.2.0**时,命令行一直无响应,解决方案是到[scipy-1.1.0](https://pypi.org/project/scipy/1.1.0/)和[mxnet-cu90 1.2.0](https://pypi.org/project/mxnet-cu90/1.2.0/)下载指定的文件，然后通过pip3命令来执行安装，其余问题也可以通过类似命令来解决。

```
pip3 install D:\scipy-1.1.0-cp36-none-win_amd64.whl
pip3 install D:\mxnet_cu90-1.2.0-py2.py3-none-win_amd64.whl
```
通过下载的文件可以得知，文件较大，命令行无法及时完成下载,所以**需要有一定的耐心等待**
>注意需要将**C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python36_64\Scripts**添加到**PATH变量**中。

## cudnn版本不对

现在官网[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)提供的cudnn版本是7.4.1,而微软示例代码中的cudnn版本是7.0.3,高版本的cudnn也会导致编译失败，需要找低版本的7.0.*的cudnn替换到**C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\bin\cudnn64_7.dll**  

# 运行结果
解决了这部分问题之后,能正常展示winform界面，运行结果如下:
![正确识别](正确.png)
![错误识别](错误.png)
该部分涉及到训练模型是否足够多的问题，该文不做深入的研究。

# 总结

参考资料中提及的两篇文章都已经是做**手写识别**非常好的入门资料，该文仅仅是对这两篇文章做一个相应的补充，以作备忘。  
另外我希望早日掌握以下技能
>识别**开心消消乐**的游戏界面，然后通过能够确定执行的最佳下一步，达到这个目的，我觉得我对人工智能的了解和我的AI编程就进入了新的层次了。

#参考资料
* [实战：从0搭建完整 AI 开发环境写出第一个 AI 应用](https://cloud.tencent.com/developer/article/1348205)
* [AI应用开发实战 - 从零开始配置环境](http://www.cnblogs.com/ms-uap/p/9123033.html)
