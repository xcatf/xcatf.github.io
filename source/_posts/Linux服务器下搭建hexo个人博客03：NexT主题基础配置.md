---
title: Linux服务器下搭建hexo个人博客03：NexT主题基础配置
description: 初始的hexo博客主题不是特别美观，我们选择用简约风格的NexT主题来美化我们的博客。目前，我把我暂时的美化设置写在了博客里面，方便以后的查阅。同时，也欢迎大家来踩一踩，共同交流~~
categories: 前端
date: 2020-2-18
time: 22:38:12
tags:
   - linux 
   - hexo
   - 服务器
---

#### 安装NexT主题

在hexo目录在使用git命令安装NexT主题，目前NexT主题的版本为V7.7.2，因此可能有些配置与之前版本会存在一点差异

使用git来安装NexT，指令clone目录为themes/next，便于hexo框架以后更换主题

~~~
git clone https://github.com/theme-next/hexo-theme-next themes/next
~~~

#### NexT主题设定

首先我们认识一下配置文件`_config.yml`

hexo目录下的`_config.yml`：主题配置文件

themes/next目录下的`_config.yml`：站点配置文件

后面，我们主题的美化，将会经常修改这两个配置文件

#### 更换当前hexo主题，设置站点名、语言、作者及其相关描述

打开站点配置文件

~~~
vim _config.yml
~~~

vim可在命令模式下:/Site来找到关键字Site，修改相关内容

~~~yml
# Site
title: xcatf's Blog
subtitle:
description: 千般荒凉，以此为梦。<br>万里蹀躞，以此为归。
keywords:
author: xcatf # 作者名字
language: zh-CN # NexT V7简体中文为zh-CN
timezone: '' # timezone为时区，默认设置即可
~~~

:/theme寻找关键字`theme`，修改hexo主题

~~~yml
# Extensions
## Plugins: https://hexo.io/plugins/
plugins: hexo-generate-feed
## Themes: https://hexo.io/themes/
theme: next
~~~

#### 更换风格scheme

NexT主题有四种不同风格

在主题配置文件`theme/next/_config.yml`下查找`Schemes`

~~~
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
~~~

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

Scheme 的切换通过更改 **主题配置文件**，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 `#` 去除即可。

#### 设置菜单

编辑`主题配置文件`，寻找关键字`menu`，根据需要去除`#`即可

~~~yml
menu:
  home: / || home   # 主页
  about: /about/ || user # 关于页面
  tags: /tags/ || tags # 标签页
  categories: /categories/ || th # 分类页
  archives: /archives/ || archive # 归档页
  schedule: /schedule/ || calendar # 日程页
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
~~~

#### 设置头像

编辑`主题配置文件`，寻找关键字`avatar`，头像的路径在`themes/next/source/images`

~~~yml
# Sidebar Avatar 头像
avatar:
  # Replace the default image and set the url here.
  url: /images/avatar.png #图片路径
  # If true, the avatar will be dispalyed in circle.
  rounded: true   # 头像会根据你的图片展示成圆形或椭圆
  # If true, the avatar will be rotated with the cursor.
  rotated: true
  # 透明度
  opacity: 1
~~~

暂时写到这吧，查阅官方文档可以获得更加详细的帮助

`http://theme-next.iissnan.com/`
