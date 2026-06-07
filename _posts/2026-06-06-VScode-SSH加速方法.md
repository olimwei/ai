---
title: VS code里插件Remote‑SSH加速的几个有效方法"
layout: blog
excerpt: "Remote‑SSH 加速的 7 个最有效方法（从最重要到次重要）"
read_time: true
comments: true
share: true
# author_profile: true
classes: wide
categories:
  - 欧耶之AI
tags:
  - VScode
  - SSH
---
🚀 Remote‑SSH 加速的 7 个最有效方法（从最重要到次重要）
🟦 1. 启用 SSH Multiplexing（速度提升最明显）
Remote‑SSH 每次连接都会重新建立 SSH 通道，
但如果你启用 SSH 多路复用，速度会快很多。

编辑本地电脑的 SSH 配置：

Code
vim ~/.ssh/config
加入：

Code
Host *
    ControlMaster auto
    ControlPath ~/.ssh/control-%r@%h:%p
    ControlPersist 10m
效果：

第一次连接稍慢

之后所有连接几乎瞬间完成

VS Code Remote‑SSH 也会快很多

👉 想深入了解：SSH 多路复用

🟩 2. 关闭 VS Code 自动安装扩展到远端
Remote‑SSH 默认会把你本地的扩展同步到 VPS，
这会导致：

连接变慢

远端安装扩展

占用 VPS 空间

你可以关闭它：

VS Code 设置 → 搜索：remote.SSH.defaultExtensions → 清空

或者：

Code
"remote.SSH.defaultExtensions": []
👉 想深入了解：远端扩展管理

🟧 3. 禁用 VS Code 自动更新（避免频繁安装 Server）
你之前已经发现：

本地 VS Code 更新

远端就必须安装新版本 VS Code Server

这会导致连接变慢

你可以关闭自动更新：

设置 → 搜索：update.mode → 设为 none

或者 settings.json：

Code
"update.mode": "none"
👉 想深入了解：禁用 VS Code 自动更新

🟨 4. 清理远端旧版本 VS Code Server（减少扫描时间）
你 VPS 上现在有多个版本：

Code
~/.vscode-server/code-xxxx
~/.vscode-server/code-yyyy
VS Code 在连接时会扫描这些目录，
太多版本会让连接变慢。

你可以安全删除旧版本：

Code
rm -rf ~/.vscode-server/bin/*
VS Code 会自动重新安装当前版本。

👉 想深入了解：清理 VS Code Server

🟪 5. 关闭远端的文件监控（尤其是大项目）
VS Code 会在远端监控文件变化，
如果你的项目文件很多，会拖慢 Remote‑SSH。

在 settings.json 加：

Code
"files.watcherExclude": {
    "**": true
}
或者只排除大目录：

Code
"files.watcherExclude": {
    "**/node_modules/**": true,
    "**/.git/**": true
}
👉 想深入了解：文件监控优化

🟫 6. 使用更快的 SSH 加密算法（可选）
在 ~/.ssh/config 加：

Code
Host *
    Ciphers aes128-ctr
    MACs hmac-sha1
这会让 SSH 更快（尤其是弱 CPU 的 VPS）。

👉 想深入了解：SSH 加密优化

🟥 7. 使用本地代理（ProxyJump / ProxyCommand）
如果你是从加拿大连接亚洲 VPS，
延迟会比较高。

你可以用中转节点加速：

Code
Host vps
    HostName your-vps-ip
    ProxyJump fast-node
👉 想深入了解：SSH ProxyJump

🎉 最推荐的组合（立刻见效）
如果你只想做最有效的三件事：

开启 SSH Multiplexing（效果最明显）

关闭 VS Code 自动更新（避免重复安装 Server）

关闭扩展同步（减少远端负担）

这三项能让 Remote‑SSH 速度提升 2～5 倍。