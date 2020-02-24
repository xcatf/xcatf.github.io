---
title: Linux服务器下搭建hexo个人博客02：hexo部署到GitHub Pages
tags: 
   - 前端
   - hexo
   - 服务器
categories: 前端
date: 2020-2-17
time: 12:00:00
description: 如果我们没有服务器的话，把hexo博客部署到GitHub上面是个不错的主意。虽然访问会有些慢，不过极大地方便了我们的管理
---

##### 1.在GitHub上新增一个repository

![img](https://img-blog.csdnimg.cn/20200215213309334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

新建仓库的配置信息如下：

![img](https://img-blog.csdnimg.cn/20200215214353119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 定义GitHub Pages域名

![img](https://img-blog.csdnimg.cn/20200215214625355.png)

##### 此时我们访问这个网址将会显示readme文件信息

~~~
https://html1211.github.io/
~~~

##### GitHub添加ssh-key

~~~
[root@iZwz93tzhqv97y8089coonZ ~]# ssh-keygen -t rsa  #需要输入的地方直接Enter即可
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:S4lmg3k4y7ei2QgK8aSxR7866vEo8EviRnUgBLfGXkE root@iZwz93tzhqv97y8089coonZ
The key's randomart image is:
+---[RSA 2048]----+
|+...E            |
| + o .           |
|  = o            |
| o o = . .       |
|o = * * S        |
|.X o * o .       |
|Oo+ + . .        |
|=**+.o .         |
|*=**o..          |
+----[SHA256]-----+
[root@iZwz93tzhqv97y8089coonZ ~]# cd .ssh
[root@iZwz93tzhqv97y8089coonZ .ssh]# ls
authorized_keys  id_rsa  id_rsa.pub
[root@iZwz93tzhqv97y8089coonZ .ssh]# ll
total 8
-rw------- 1 root root    0 Feb 15 12:04 authorized_keys
-rw------- 1 root root 1675 Feb 15 20:58 id_rsa
-rw-r--r-- 1 root root  410 Feb 15 20:58 id_rsa.pub
[root@iZwz93tzhqv97y8089coonZ .ssh]# cat id_rsa.pub  #查看公钥内容
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC3z1vmc/Fgzbhk+Ubkbi1KvN6ovHhsuBmGVZRclBDhkWaYX23Psre1dn9mJsQv5n1y1LjkPLtliaaYROBRFCzHDSVQQSLVvG1OIMGtYAD1FIiUf3Sy2Of3UxSRMFVbTaFJCg7STV6QIyigPeJq5xHeuHh4BpIZRajzex9Vqprv3hyjcT46F547Zpl3pzPvBuEuoA5xJIQ4cG5JzWbmgLFvMBvMG0labxvYFbWZJeUBxUx55wacOpP/NhzqM46Ihrw2S11+JDDrwqE9rRZyRKeZ9xqmdTvQL594AA3h6f+w1UBj6LG1f32YaeQyXD5PNPZE+XiDMwP4IFh345/9rCW/ root@iZwz93tzhqv97y8089coonZ
~~~

![img](https://img-blog.csdnimg.cn/20200215220058232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/2020021522040914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 服务器通过ssh连接GitHub

`服务器成功连接GitHub信息如下`

~~~
[root@iZwz93tzhqv97y8089coonZ /]# ssh -T  git@github.com
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
Hi html1211! You've successfully authenticated, but GitHub does not provide shell access.
~~~

##### 设置服务器Git账户

`账户和邮箱均需与GitHub保持一致`

~~~
[root@iZwz93tzhqv97y8089coonZ /]# git config --global user.name "html1211"
[root@iZwz93tzhqv97y8089coonZ /]# git config --global user.email "1013459920@qq.com"
~~~

##### 修改hexo站点配置文件，配置hexo默认git提交仓库

打开站点配置文件`_config.yml`

~~~
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:html1211/html1211.github.io.git
  branch: master
~~~

hexo部署环境需要`hexo-deployer-git`插件

~~~
[root@iZwz93tzhqv97y8089coonZ hexo]# npm install hexo-deployer-git --save
~~~

##### 部署hexo到GitHub

~~~
[root@iZwz93tzhqv97y8089coonZ hexo]# hexo d -g  # -g可选，表示生成静态网页
~~~

##### 浏览器访问

~~~
https://html1211.github.io/    #GitHub Pages访问
http://服务器公网ip/           	 #个人网站访问
http://服务器域名/                #个人网站访问
~~~

