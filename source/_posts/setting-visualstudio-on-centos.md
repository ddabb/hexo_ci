---
title: centos安装visualstudio code
toc: true
top: 100
mathjax: false
date: 2021-12-26 17:07:56
tags:
- centos
- visualstudion
categories:
- 代码世界

---
**Visual Studio Code 安装**

安装libXss.so.1


```
yum install libXScrnSaver
```

官网下载vscode
```
wget https://vscode.cdn.azure.cn/stable/0f3794b38477eea13fb47fbe15a42798e6129338/code-1.36.0-1562161214.el7.x86_64.rpm
```

通过yum 安装


```
yum install code-1.36.0-1562161214.el7.x86_64.rpm
```

**.net core 安装**



注册 Microsoft 密钥和源
```
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
```

依次执行安装SDK和运行时。
```
sudo yum install dotnet-sdk-3.1
sudo yum install dotnet-runtime-3.1
sudo yum install aspnetcore-runtime-3.1
```

打开扩展页面，搜索C#，安装C#支持；

打开sln文件所在文件夹，配置好launch.json文件，然后ctrl+f5就可以进行调试了。
