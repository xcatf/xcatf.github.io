---
title: Linux服务器下搭建hexo个人博客01：hexo部署到Nginx
categories: 前端
tags: 
    - linux
    - 前端
date: 2020-2-16
time: 12:00:00
description: "前几天领了阿里云半年免费的云服务器。最近一直在服务器上捣鼓。听说hexo框架搭建静态博客极其方便，于是用hexo框架搭建了个人博客，部署于云服务器Linux上，后续将会优化我的博客。欢迎大家评论交流~"
---

##### 安装nginx

~~~
yum install -y nginx
~~~

修改nginx配置文件nginx.conf部分内容

```
[root@iZwz93tzhqv97y8089coonZ /]# vim /etc/nginx/nginx.conf
```

~~~
 server {
        listen       80 default_server;
        server_name  localhost;
 
 
 
        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
 
        location / {
           # 这里写自己hexo搭建的目录
           root /opt/hexo/public; # public文件夹是hexo生成静态文件的默认文件夹
           index index.html index.htm;
        }
 
        error_page 404 /404.html;
            location = /40x.html {
              root  html;
        }
 
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
              root html;
       }
}
~~~

##### 启动nginx服务

~~~
systemctl start nginx
systemctl enable nginx # 开机启动nginx服务
~~~

##### 配置安全组，开放80端口

![img](https://img-blog.csdnimg.cn/20200216193141199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 安装Git

~~~
yum install -y git
~~~

##### 安装Node.js

下载nodejs

~~~
wget https://nodejs.org/dist/v12.16.0/node-v12.16.0-linux-x64.tar.xz
~~~

解压

~~~
tar -xvf node-v12.16.0-linux-x64.tar.xz
~~~

为了简洁，文件重命名为nodejs

```
mv node-v12.16.0-linux-x64 nodejs
```

修改配置文件etc/profile

```
vim /etc/profile
```

在配置文件中添加环境变量：PATH=/opt/nodejs/bin:$PATH

```
for i in /etc/profile.d/*.sh /etc/profile.d/sh.local ; do
    if [ -r "$i" ]; then
        if [ "${-#*i}" != "$-" ]; then
            . "$i"
        else
            . "$i" >/dev/null
        fi
    fi
done
PATH=/root/anaconda3/bin:$PATH
PATH=/opt/nodejs/bin:$PATH
unset i
unset -f pathmunge
```

重新加载配置文件

```
source /etc/profile
```

node -v/npm -v测试

~~~
[root@iZwz93tzhqv97y8089coonZ /]# npm -v
6.13.4
[root@iZwz93tzhqv97y8089coonZ /]# node -v
v12.16.0
~~~

##### 安装hexo

```
npm install hexo-cli -g
```

##### 部署hexo博客

在指定文件夹在部署hexo博客，我选择的是/opt/hexo目录下，与nginx配置文件中目录保持一致

```
[root@iZwz93tzhqv97y8089coonZ hexo]# pwd
/opt/hexo
[root@iZwz93tzhqv97y8089coonZ hexo]# hexo init # 在当前文件夹下建立网站
[root@iZwz93tzhqv97y8089coonZ hexo]# hexo g # 生成静态文件(public文件夹)
[root@iZwz93tzhqv97y8089coonZ hexo]# ll
total 352
-rw-r--r--   1 root root   3412 Feb 16 19:05 _config.yml
-rw-r--r--   1 root root  69279 Feb 16 19:05 db.json
drwxr-xr-x 472 root root  20480 Feb 16 19:04 node_modules
-rw-r--r--   1 root root    772 Feb 16 18:26 package.json
-rw-r--r--   1 root root 241687 Feb 16 18:26 package-lock.json
drwxr-xr-x   9 root root   4096 Feb 16 19:05 public
drwxr-xr-x   2 root root   4096 Feb 15 16:39 scaffolds
drwxr-xr-x   3 root root   4096 Feb 15 17:15 source
drwxr-xr-x   4 root root   4096 Feb 16 15:38 themes
```

##### 浏览器访问

由于之前已经修改Nginx服务器配置文件，所以直接用公网ip访问将会显示hexo博客

```
http://公网ip/
```
