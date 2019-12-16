---
title: Vue Element UI + OSS上传文件
url: 315.html
id: 315
categories:
  - 生活随笔
tags:
---

一 Web端直传实践简介 一 [JavaScript客户端签名直传](http://jamesxu.wang/2019/01/19/3%e5%ba%a6%e5%b8%a6%e7%9a%84%e8%bf%98%e6%98%af6%e5%ba%a6%e5%b8%a6%e8%ae%b0%e5%bd%95/) 介绍如何基于POST Policy的使用规则在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。 二 [服务端签名后直传](https://help.aliyun.com/document_detail/31926.html?spm=a2c4g.11186623.2.11.5e6b6212brheo1#concept-en4-sjy-5db) 介绍如何在服务端完成签名并通过表单直传数据到OSS。 三 [服务端签名直传并设置上传回调](https://help.aliyun.com/document_detail/31927.html?spm=a2c4g.11186623.6.1201.75742f08jT53bK) OSS回调完成后，应用服务器再将结果返回给客户端，以便服务端实时了解用户上传了什么文件。