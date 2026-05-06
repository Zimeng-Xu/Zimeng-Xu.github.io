---
title: 关于在Hexo博客中插入图片显示失败的解决办法
date: 2025-04-17 21:52:38
tags: Hexo
categories: 经验分享
---

# 前言

今日在个人Hexo博客中上传博文时，插入的图片无法成功显示，于CSDN上阅读多篇帖子的教程，均无法解决问题。后在不断尝试之下终于找到了一个解决办法，故在此分享，希望可以帮助到需要的人。

# 解决办法（已失效）

- 打开浏览器，访问[Image Upload - Free image hosting](https://imgloc.com/)。

- 点击开始上传，选择你需要插入的图片。

  ![imgloc首页](%E5%85%B3%E4%BA%8E%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E5%A4%B1%E8%B4%A5%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/imgloc%E9%A6%96%E9%A1%B5.png)

- 点击上传。

  ![imgloc准备上传](%E5%85%B3%E4%BA%8E%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E5%A4%B1%E8%B4%A5%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/imgloc%E5%87%86%E5%A4%87%E4%B8%8A%E4%BC%A0.png)

- 点击`嵌入代码`的小三角，选择`HTML图像`。

  ![imgloc改变嵌入代码](%E5%85%B3%E4%BA%8E%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E5%A4%B1%E8%B4%A5%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/imgloc%E6%94%B9%E5%8F%98%E5%B5%8C%E5%85%A5%E4%BB%A3%E7%A0%81.png)

  ![imgloc选择HTML图像](%E5%85%B3%E4%BA%8E%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E5%A4%B1%E8%B4%A5%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/imgloc%E9%80%89%E6%8B%A9HTML%E5%9B%BE%E5%83%8F.png)

- 复制下方连接。

  ![imgloc复制链接](%E5%85%B3%E4%BA%8E%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E5%A4%B1%E8%B4%A5%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/imgloc%E5%A4%8D%E5%88%B6%E9%93%BE%E6%8E%A5.png)

- 将该连接粘贴至Typora中，保存文件，使用`hexo g`、`hexo d`，将博文上传至博客。

# 解决办法 - 新

由于上述网站调整，图片托管有效期从永久调整为1年。新解决办法参见该GitHub中的内容：https://github.com/slcnx/hexo-typora-asset 。