---
title: Centos ssh免密码登陆配置
toc: true
top: 100
mathjax: false
date: 2021-12-26 17:48:22
tags:
- centos
- ssh
categories:
- 代码世界
---
测试 ssh 是否安装
```
rpm -qa | grep ssh
```
若返回的结果中包括 SSH client 和 SSH server ,则不需要再安装 若需安装，通过以下命令
```
yum install openssh-clients 
yum install openssh-server
```

查看主机名称
```
hostname
```

然后 
```
ssh hostname(自己的主机名)
```
设置免密码登陆
```
cd ~/.ssh 
ssh-keygen -t rsa
```

通过 ssh-keygen 命令来生成密钥对在.ssh 目录下（会有提示，回车即可）。 
此命令执行后，会在~/.ssh 目录下生成私钥 id_rsa 和公钥 id_rsa.pub 
```
cat id_rsa.pub >> authorized_keys 
```
将 id_rsa.pub 中的公钥信息保存到 authorized_keys 中 
```
chmod 600 ./authorized_keys
``` 
修改文件授权 此时用 ssh hostname 命令，无需密码即可直接登陆。