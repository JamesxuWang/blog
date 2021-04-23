---
title: GITHUB 被墙解决
categories:
  - 生活随笔
date: 2021-04-15 18:32:00
tags: [生活日志]
---

# 新电脑！
换了新电脑，装博客。 发现GitHub页面都不太能进去。就开了梯子，开了全局代理都没办法clone。

报错： fatal: unable to access 'https://github.com/xxx.git/': Failed to connect to github.com port 443: Timed out 

在网上搜了解决方案~

配置 git 的代理 (这里，梯子要始终开着使用, 以Lantern为例)
git config --global http.proxy "127.0.0.1:51564"
git config --global https.proxy "127.0.0.1:51564" 
 https 方式  clone git clone https://github.com/xxx.git 
 (注意：这里必须使用 https 方式， ssh 方式即使配置了 git 的代理也不好使。)。后续后来
 
 
 如果GitHub又能访问了...开梯子会出现了如下错误：
  fatal: unable to access 'https://github.com/FengGuanxi/HDU-Experience.git/': Failed to connect to 127.0.0.1 port 1080: Connection refused 
  
  那就需要取消代理
  命令是： 
  git config --global --unset http.proxy
  git config --global --unset https.proxy

  
先配置

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

再 git （add . / commit -m "xxx"/ push）一套操作就行啦！ 