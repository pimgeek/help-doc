---
layout: post
categories: ['jekyll hack']
reader-title: 从零开始构建基于 Jekyll 的文档站
title: doc-site-from-scratch
next-post: jekyll-environment-setup.html
---

为了帮助 MINDPIN 团队把团队内的各种文档有效地组织起来，我围绕以下要点进行了一系列调研：

- GitHub Help 站点的内容组织和页面导航
- GitHub Help 与 Facebook React Doc 两个站点的部分实用 feature
  - GitHub Flavored Markdown
  - 无后端的全文搜索（lunrjs）
  - 为若干篇文档按类别生成独立的索引页
  - 为一篇文档指定前置文档和后继文档
  - 为一篇文档指定若干个子文档
  - 在父文档的正文内自动生成子文档列表
  - 多篇不同的文档中引用同一段文字（conref）

接下来的文档将首先介绍在本地 Debian Linux 虚拟机环境下运行的 jekyll 文档站的基本配置。
