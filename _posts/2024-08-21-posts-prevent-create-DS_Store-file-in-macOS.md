---
title: "如何防止macOS自动生成.DS_Store文件"
header:
  image: 
  teaser: https://image.olim.in/ai.olim.ca/teaser/3-th.jpg
layout: blog
excerpt: "咨询了chatGPT：如何防止macOS自动生成.DS_Store文件？回答如下："
read_time: true
comments: true
share: true
# author_profile: true
classes: wide
categories:
  - 欧耶之AI
tags:
  - 
---

咨询了chatGPT：如何防止macOS自动生成.DS_Store文件？回答整理如下：

1. Network Share：
   1. 打开terminal
   2. 输入: 
   ```
   defaults write com.apple.desktopservices DSDontWriteNetworkStores true
   ```
   3. 重新启动电脑。
2. Removable Drivers(e.g. USD SD cards):
   1. 在terminal中输入：
   ```
   defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true
   ```
   2. 重启电脑。
3. 这个设置只针对当前用户，对同一台电脑其他用户无效。如果想重新启动自动生成.DS_Store文件，用上面的命令，把true换成false即可。 

chatGPT真是个得力小助手！