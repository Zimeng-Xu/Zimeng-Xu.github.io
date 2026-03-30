---
title: 关于在Hexo博客中插入图片显示失败的解决办法
date: 2025-04-17 21:52:38
tags: Hexo
categories: 经验分享
---

# 前言

今日在个人Hexo博客中上传博文时，插入的图片无法成功显示，于CSDN上阅读多篇帖子的教程，均无法解决问题。后在不断尝试之下终于找到了一个解决办法，故在此分享，希望可以帮助到需要的人。

# 解决办法

- 打开浏览器，访问[Image Upload - Free image hosting](https://imgloc.com/)。

- 点击开始上传，选择你需要插入的图片。

  <img src="https://i.imgs.ovh/2025/04/17/js32t.png" alt="js32t.png" border="0">

- 点击上传。

  <img src="https://i.imgs.ovh/2025/04/17/j9Z5H.png" alt="j9Z5H.png" border="0">

- 点击`嵌入代码`的小三角，选择`HTML图像`。

  <img src="https://i.imgs.ovh/2025/04/17/j97D4.png" alt="j97D4.png" border="0">

  <img src="https://i.imgs.ovh/2025/04/17/j9OaN.png" alt="j9OaN.png" border="0">

- 复制下方连接。

  <img src="https://i.imgs.ovh/2025/04/17/j9ChA.png" alt="j9ChA.png" border="0">

- 将该连接粘贴至Typora中，保存文件，使用`hexo g`、`hexo d`，将博文上传至博客。
