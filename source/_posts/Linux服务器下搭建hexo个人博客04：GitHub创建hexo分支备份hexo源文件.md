---
titles: Linux服务器下搭建hexo个人博客04：GitHub创建hexo分支备份hexo源文件
tags: 
	- linux
	- 服务器
	- hexo
	- git
	- github
categories:
	- 前端
description: hexo -d命令只会在GitHub上面部署生成的html，css，js等网页相关文件，而hexo源文件则仍然保存在服务器/本机上，这不利于hexo博客的搬迁。实际上，我们可以在GitHub上的hexo博客仓库里创建一个`hexo分支`保存所有的`hexo源文件`，这样以后在其它服务器上我们可以通过`git clone`实现hexo博客一键移植
---

##### 在之前username.github.io仓库里新建hexo分支
点击`Branch`输入`hexo`，即可创建hexo分支
因为我已经创好hexo分支了，所以这里创建分支a示例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200223124233208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)

##### 将hexo设为默认分支
`为了简化后面的部署，建议hexo设为默认分支`
`settings->Branches`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200223124411549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)
##### 服务器git clone该仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200223124655654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxMTAxMzQ1OTkyMA==,size_16,color_FFFFFF,t_70)
指定目录下clone该项目，这里我选择的是`/opt/hexo_surce`,`hexo`为之前`hexo源文件`的目录
~~~
[root@iZwz93tzhqv97y8089coonZ opt]# pwd
/opt
[root@iZwz93tzhqv97y8089coonZ opt]# git clone https://github.com/html1211/html1211.github.io.git hexo_source
[root@iZwz93tzhqv97y8089coonZ opt]# ls
hexo  hexo_source  nodejs
~~~
##### 删除hexo分支下原有内容
由于新建的hexo分支会与之前master分支内容一致(`保存的是静态网页`)，`hexo分支`不需要存放这些文件
~~~
git  rm -r <file>
~~~
删除所有仓库文件(`.git除外`)
~~~
[root@iZwz93tzhqv97y8089coonZ hexo_source]# ls -a
.  ..  2020  about  archives  atom.xml  categories  css  .git  images  index.html  js  lib  live2dw  schedule  search.xml  tags
[root@iZwz93tzhqv97y8089coonZ hexo_source]# git rm -r 2020  about  archives  atom.xml  categories  css  images  index.html  js  lib  live2dw  schedule  search.xml  tags
~~~
##### 设置git origin
~~~
git remote add origin https://github.com/username/username.github.io.git
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200223130843914.png)
##### 复制hexo源文件到hexo_source目录
检查一下hexo目录下是否有`.git`和,有的话删除它，不然它会覆盖`hexo_source`目录下b的`.git`，
`本地hexo_source仓库`就会失效
同时，如果hexo某个子目录存在`.git`文件，那么这个子目录将无法git add到`缓存区`提交到`GitHub`
一般来说
`hexo/themes/next`目录会有`.git`文件(`因为之前是通过git clone下载的next主题`)
`hexo/source/lib/canvas-nest`目录会有`.git`(`安装了这个插件的话`)
保险起见，查找所有.git文件
~~~
find / -name .git
~~~
如果是在`hexo/`目录下的话，`rm -rf`删除它们

为了简单，我们直接`拷贝hexo目录下所有文件至hexo_source`
~~~
/bin/cp -rf hexo/. hexo_source
~~~
##### git提交hexo源文件
`git add -f`强制提交所有源文件(包括`非版本控制文件`)
~~~
[root@iZwz93tzhqv97y8089coonZ hexo_source]# ls
_config.yml  db.json  node_modules  package.json  package-lock.json  public  scaffolds  source  themes
[root@iZwz93tzhqv97y8089coonZ hexo_source]# git add -f _config.yml  db.json  node_modules  package.json  package-lock.json  public  scaffolds  source  themes
~~~
~~~
git commit -m "add hexo source files"
~~~
git提交文件至`hexo分支`
~~~
git push origin hexo
~~~
这样，你的所有`hexo源文件`放在了参仓库的`hexo分支`
##### 移植新服务器
在新服务器下指定目录`git clone 仓库hexo源文件`
~~~
git clone https://github.com/html1211/html1211.github.io.git hexo
~~~
甚至可以删除当前服务器下原来的hexo目录，在当前服务器hexo_source目录下
开发，`hexo g`生成静态网页
通过`hexo d`部署到仓库的`master分支`
通过`git add -u`，`git commit -m ""` ,`git push origin hexo`部署到仓库的`hexo分支`