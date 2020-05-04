---
layout:     post
title:      linux使用证书登录
subtitle:   连接被拒绝？
date:       2020-05-04
author:     hukss
header-img: img/linux-putty.jpg
catalog: true
tags:
    - linux
---
### 记录一次linux使用证书登录发现的问题
今日iK发货了，开启了折腾


### 问题重现
使用root账户登录ssh之后第一时间设置证书登录
这里使用的是树莓派的环境，因此进入树莓派账户的家目录/home/pi
创建.ssh文件夹，修改权限为700
接着进入.ssh文件夹，使用ssh-keygen生成证书文件并创建公钥
```
touch authorized_keys
```

修改公钥文件权限
```
chmod 600 authorized_keys
```
将生成的公钥添加到公钥文件
```
cat id_rsa.pub >>  authorized_keys
```

本地用psftp连接远程机器下载id_rsa文件，在这一步可以用刚才打开的终端将authorized_keys文件复制到home里面，方便用psftp来get到本地，之后使用putty的puttygen来转换格式并保存

紧接着修改ssh的配置文件/etc/ssh/sshd_config

```
修改登录端口
Port xxxx

禁用root登录
PermitRootLogin no

启用这个选项表示ssh程序会检查用户目录下的.ssh文件夹相关权限，具体原则为.ssh文件夹的权限必须为登录的用户可读可写可执行（像700这样就可以），
下面的authorized_keys也必须为当前用户可读（像600这样可，400也可）
StrictModes yes

允许使用证书登录
PubkeyAuthentication yes

关闭密码登录
PasswordAuthentication no

禁止空密码登录
#PermitEmptyPasswords no
```


接着保存重启sshd程序
/etc/init.d/ssh restart


然后新开一个终端用证书登录试试
ip，端口
Connection里面data填上用户名
Connection-》Auth-》Private key file for authentication里面点浏览，找到转换后的rsa-key，存个名字然后打开

问题来了，证书被拒绝

### 解决办法
进入终端查看日志auth.log无果后绝望了

然后继续检查。
ls -al /home/pi/.ssh

发现文件夹所有者和群组是root，真相了，继续查看authorized_keys，拥有者也是root
查到改变文件所有者和群组的命令如下：
chgrp  用户名    文件名  -R
chown 用户名   文件名  -R

改完便可以登录了

### 总结
其实这次的经历是少有的，因为一般情况下我都是使用普通用户登录进去sudo做事情，但是今天的iK邮件发过来的是root账户密码，也没想到会产生这个问题。仔细想想，这是必然的。因为我之前没用过iK，不知道他这个树莓派有个默认账户是pi，然后自己的linux也处于那种要用啥命令现查的那种水平。系统性的学习linux看来是个必然的了。


