---
title: Hexo使用markdown插入图片无法显示解决方法
date: 2019-05-25 20:17:06
tags:
  - hexo
  - Markdown
---

hexo发布博客时，文章中引用的本地图片总是无法显示。
hexo默认无法处理文章插入本地图片，需要通过扩展插件支持。

# 插件安装与配置
1、我们需要安装一个图片路径转换的插件，插件名字为**hexo-asset-image**，只需执行以下命令
```
cd /d 你的hexo根目录
npm install https://github.com/7ym0n/hexo-asset-image --save
```

<!--more-->

2、打开hexo根目录下的_config.yml文件，修改下述内容
```
post_asset_folder: true

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yourname.github.io
```	

# 博文插入本地图片
运行`hexo n "你的博文名字"`生成md博文时，/source/_posts目录下会生成一个与md同名的文件夹，将图片放人该文件夹。
	
	使用 ![xxx](你的博文名字/xxx.png)插入图片即可。