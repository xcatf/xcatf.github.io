---
title: 云服务器Linux下修改ssh默认端口
tags: 
	- 服务器
  	- linux
categories: linux
description: 最近服务器受到了ssh暴力破解攻击，目前我的解决方法是修改ssh端口

---

##### 修改ssh配置文件中的默认端口

修改`/etc/ssh/sshd_config`配置文件

~~~
vim /etc/ssh/sshd_config
~~~

ssh默认监听端口是`22`，如果不强制说明其它端口，Port 22注不注释都是开放22端口

~~~
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
#Port 22 #默认22端口是被注释的，我们在下面再写一行Port
Port 端口号#这里写你想要的端口
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::
~~~

##### 防火墙开放对应端口号

千万不要急着重启ssh服务器，先让防火墙开放对应端口

`注意：permanent代表防火墙对该端口永久开放，没有该字段重启防火墙该端口失效`

~~~
firewall-cmd --zone=public --add-port=端口号/tcp --permanent
~~~

##### 防火墙重启服务

~~~
systemctl restart firewalld
~~~

##### 查看端口号是否生效

~~~
firewall-cmd --zone=public --query-port=端口号/tcp
yes
~~~

##### 服务器控制台配置对应安全组

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200221094958825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 服务器本地尝试用新端口号登录ssh

~~~
ssh root@localhost -p 端口号
~~~

##### 重启ssh服务

`[注]：要确保防火墙&控制台安全组开放新端口前提下再重启ssh服务`

~~~
systemctl restart sshd
~~~

退出连接

~~~
exit
~~~

##### 修改连接的端口再次连接服务器

这里我用的Xshell工具

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200221094833630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 如果出现问题，再次检查前面每个步骤，确保端口对防火墙开放

出现问题，ssh连接不上服务器的话可以打开服务器控制台，选择`VNC`连接

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200221095045752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)