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

导入Microsoft GPG key：

```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```

官网下载vscode
```
sudo gedit /etc/yum.repos.d/vscode.repo
```

将下面的内容粘贴进去，并且启用 VS Code 源：

```
[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc
```
保存文件，并且关闭你的文本编辑器。

安装最新版本的 Visual Studio Code，输入：
```
sudo dnf install code
```
升级visualstudio code

当一个新版本发布时，你可以通过你的桌面软件升级工具或者通过在终端运行下面的命令，来升级 Visual Studio Code：  
```
sudo dnf update
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
