---
title: 转移至GitHub Page的记录
date: 2024-1-9 16:00:00
---

### 前言

自己的blog一直在AWS的free tier的服务器上，最近发现free tier马上要过期；在看到了其他人的blog静态页面部分在GitHub Page上托管的，就有了准备转移的想法~~（主要是不用管太多，而且不要钱）~~，这篇文章用来记录自己在转移blog文档的时候的部分情况。

鉴于之前的blog文件都在云服务上面，所以这次选择转移到自己的macOS上面通过git进行管理；

### 配置Github

通过查阅Hexo的[官方文档](https://hexo.io/docs/github-pages.html)，转移的过程可以简单概括为两部：创建修改仓库；推送文件即可完成。按照文档指引，首先需要修改Github仓库的设置，将Setting的Pages里的`Build and deploy`的选项修改为`GitHub Actions`，之后便可以开始修改本地的仓库。

### 配置本地仓库

首先在终端使用`git init`命令在一个空的文件夹里建立工作目录，然后`cd`进入工作目录使用命令`git remote add origin https://github.com/username/username.github.io`远程连接到自己的仓库，先将官方文档里的`pages.yml`文件存放到git工作目录的`.github/workflows/`目录下，然后就就可以把源文件复制到工作目录中，`git add .`添加目录下的所有文件，`git commit -m "init"`添加commit信息，检查blog的配置文件没有问题以后就可以将本地文件用命令`git push -u origin main`推送到GitHub上自动构建。

~~虽然这一段花了非常久的时间，git的指令用的太少了都是现场查的（~~

### 解决出现的问题

第一次构建完毕后，Actions里输出的信息没有错误，但是进入网站后没有显示内容。在重新检查了配置并确定没有错误，通过搜索后确定是缺失`hexo-renderer-pug`这个包才导致build不成功，但是此时并不知道如何在build阶段添加进缺失的依赖。

询问了前端的朋友以后，才知道依赖的目录是存放在根目录的`package.json`文件内，将缺失的包名称添加进该文件后即可正常构建，页面也恢复了正常显示。

造成此问题的原因暂不明确（应该是有些操作太下饭了主动遗忘了），此前在本地build的过程没有问题，页面也是正常的，可能是自己在push的过程中由于对git的不熟悉导致推送时`package.json`文件是稍早版本导致了此次问题。

