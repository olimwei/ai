---
title: "用Cloudflare的pages搭建图库"
layout: blog
excerpt: "如何用Cloudflare的pages搭建图库？本着能不花钱绝不花钱的宗旨，摸索出一种实测可行的方法。"
read_time: true
comments: true
share: true
# author_profile: true
classes: wide
categories:
  - 欧耶之AI
tags:
  - cloudflae
  - pages
  - 图库
---

个人写博客的时候经常用到大量的图片，寄存在github上的代码空间心里总觉得哪一天不够用，所以想把图片这种占用空间较大的文件寄存在别处。对于我这种有限了解一点点电脑知识的非理工男，本着能不花钱绝不花钱的宗旨，摸索出一种实测可行的方法。

基于下面这样的构想：
1. 网站代码寄存在github；
2. 网站通过github page或netlify，cloudflare等其他免费的方式发布到网上；
3. 网站内大文件如图片视频等为不占用github的每个repository的空间而寄放在别处。

针对图片等大文件寄存在别处的免费方法有很多：
1. Amazon的S3免费5G存储空间；
2. Google云免费存储空间；
3. Oracle云免费200G磁盘空间和20G云存储空间；
4. cloudflare的R2 10G存储空间，以及pages空间。

cloudflare慷慨地提供了很多免费的功能，在研究的时候发现一个目前看来对我这种仅仅对图片存储有需求的最方便有效的方法。就是把所有图片当做一个网站通过cloudflare pages发布在网上，这样可以最大化利用每个pages提供的免费空间，而不占用cloudflare账户内其它免费资源。当然pages有本身的一些其它限制，如每个上传文件大小不得超过25M，总文件不得超过20000个，每个项目最多包含10个自定义域名，等等。。。对于我这种小用户是足够了。

方法就是：用wrangler上传网站文件到cloudflare pages的project里。因为如果通过cloudflare网站拖拉上传有总文件大小的限制。用wrangler上传可以在cloudflare的限制内最大化利用。
1. 安装wrangler到本地电脑；
2. 在cloudflare账户内创建密钥供wrangler连接cloudflare；
3. Wrangler登陆cloudflare后使用命令：wrangler pages deploy <BUILD_OUTPUT_DIRECTORY>,deloy后面跟的是你想上传的网站目录
4. 根据出现的选项，可以选择在cloudflare里新建立一个project来上传上面选择的上传目录，也可以上传到原有的project里。
注意的是，如果一次上传的文件太大太多，比如说几千个文件上G大小，可能会出现因网站缓冲设置的原因而出现不成功的现象，实在需要上传很多很大文件，可以分批上传。这种上传方式有个特点是，虽然每次都选择那个目录上传，但每次只上传增加或修改的文件。

好像在某处看到，每个pages的project，可以有总共20G的发布空间，而cloudflare允许创建包括workers在内100个projects，理论上cloudflare提供给每个免费账户2T的空间，外加R2，D1等其它免费存储空间。。。因为技术水平，我还没充分利用。

Cloudflare真是良心活菩萨！

贴一张用这样的方法存的图吧👇
![SPX走势图](https://image.olim.cc/2025/BB-20250212-hour-c.jpeg "hover me")

我还是老派按照文件夹结构和文件命名法来分类存储文件。😂
