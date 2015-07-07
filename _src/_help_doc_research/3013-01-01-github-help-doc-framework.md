---
layout: post
reader-title: GitHub Help 内容架构
title: github-help-doc-framework
prev-post: help-doc-research.html
next-post: jekyll-frontend-search.html
---
【目标】我要抽取出 help.github.com 的站点结构
【GTD 结果】已走通

#### 执行参考
@ben7th 的建议：可以在熟悉 help.github.com 站点结构的过程中把它的一些和内容浏览，导航相关的特性，如文章分类，搜索，内部链接外部链接，贴图贴代码（这是我打比方的，实际上有哪些特性按他的为准），各自是如何实现的，和开源工具中的一些用法对应起来，并在记录操作步骤的时候列出。

#### 验收标准
1 主要的页面类型都点过，留有印象
2 主要的页面互动模块和内容模块都看过，留有印象
3 各种模块与开源工具的对应关系要给出
4 配置开源工具的过程中，一些容易踩坑的操作细节要列出

1 主要页面包括
 - 主站首页(/)，各个子产品的各版本首页(/sub-prod-name/ver/)，文章页(`/articles/*`)，分类页(`/categories/*`)，搜索页(/search/)

2 各主要页面的主要模块
 - 首页以及子产品各版本首页模块列举
   - 置顶文章模块(4个图片链接)
   - 搜索框模块(search)
   - 常见问题模块(common issues)
   - 分类文章列表模块(categories)

 - 文章页模块列举
   - 搜索框模块(search)
   - 文章主体模块，其中含若干子模块
     - 不同操作系统的标签页模块(article_platform_nav)，参见 Set Up Git
     - 可折叠的帮助信息模块(helper)，参见 Set Up Git
   - 文章侧边栏模块
     - 同一文章的在其它子产品和版本下的链接

 - 分类页模块列举
   - 搜索框模块(search)
   - 文章列表模块(articles)
