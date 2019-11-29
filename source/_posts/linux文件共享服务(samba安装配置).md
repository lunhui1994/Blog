---
title: linux文件共享服务(Samba安装配置)
date: 2019-11-27 18:14:27
categories: Linux
tags: Linux
keywords: 文件共享, linux, Samba
---

我们的服务器基本都是linux系统的，但是我们需要在windows下开发。

解决方案有两种：
1. 通过git同步linux和windows下的代码。
2. 通过文件共享使windows连接linux下的共享文件夹

现在说第二种方式如何实现：

Samba的介绍我就不多说了，总结一下就是可以在windows下操作编辑linux里面共享的内容。


<!-- more -->

#####（一）samba 安装

安装之前用rpm确定一下自己的服务器是否已经安装过了。

```
    rpm -qa | grep samba
```
如果没有，我们就通过yum来下载rpm包来安装它

```
    yum install -y samba
```
安装完成之后可以再通过第一条命令查看是否安装成功。

安装成功之后，我们就需要修改samba的配置文件了。

#####（二）samba 配置

一般都在etc中
 ```
    vim /etc/samba/smb.conf
 ```
 
 打开之后其他的不用管，在该配置文件的末尾增加一段

 例如我们要共享根目录下的opt文件夹

 ```
[opt]
        comment = OPT
        path = /opt/
        public = yes
        writable = yes
        printable = no
        guest ok = yes

 ```
path 是你要共享的路径。

保存之后重启服务。

```
    systemctl start smb

    systemctl restart smb

    systemctl stop smb
```
 
重启完之后基本samba就算安装配置完成。

想要使用还需要配置samba所需要的端口。

#####（三）配置 samba 端口

在这里使用阿里云服务器举例。

![github-key](/img/anquanzu.jpg)
![github-key](/img/anquanzu1.jpg)
![github-key](/img/anquanzu2.jpg)

如图添加安全组规则

分别添加 136/138和445端口。

至此linux系统的问题都ok了。

#####（四）映射网络驱动器

然后到你的windows系统中

1. 右键我的电脑，映射网络驱动器。
2. 输入\\ip\xxx   以opt为例：\\ip\opt ,ip为你的linux服务器ip
3. 点击完成输入你的用户和密码。

我们也可以创建一个用来使用samba的用户

```
    smbpasswd -a xiaoming  #添加用户xiaoming到Samba用户中
```

然后接着会让你设置密码，设置完之后。重启samba就可以了。

#####（五）配置 samba 防火墙

如果连接不上，请关闭windows的防火墙试试。

如果还不行就设置下linux的防火墙

iptables

```
    iptables -I RH-Firewall-1-INPUT 5 -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
    iptables -I RH-Firewall-1-INPUT 5 -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT
    iptables -I RH-Firewall-1-INPUT 5 -p udp -m udp --dport 137 -j ACCEPT
    iptables -I RH-Firewall-1-INPUT 5 -p udp -m udp --dport 138-j ACCEPT
    iptables-save
    systemctl restart iptables
```

selinux

```
    setsebool -P samba_enable_home_dirs on
    setsebool -P samba_export_all_rw on
```


然后重新映射网络位置。

#####（六）放弃

如果还不可以，那放弃就好了。