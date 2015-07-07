---
layout: post
reader-title: 创建新文档
title: create-new-articles
prev-post: jekyll-environment-setup.html
---

创建新文档并不是前文所述的 7 种实用 feature 之一，但在创建新文档时，要遵循一定格式，才能激活相关的 feature。考虑到创建文档是文档撰写者最先引起关注的操作，所以首先介绍。

1 如果仅希望创建不带子文档的文档，直接以 markdown 源码形式放入 `help-doc/_posts` 下面。 `help-doc/_posts` 目录下的 Front Matter 遵循以下格式：

```yaml
---
layout: post
categories: ['category_name']
reader-title: XXX XXXX
title: article-url-name
prev-post: previous-article-url-name.html
next-post: next-article-url-name.html
----

文档正文

```
  * 如果无特殊考虑，layout 的取值总是 post
  * 如果希望当前文档在左侧导航栏中被列入某个特定的分类，就在 categories 中加入这个分类，比如 categories: ['mindpin docs']
  * reader-title 可以包含中文或其它 UTF-8 字符
  * title 以英文数字、字母和连字符组成，用于生成当前文档的 permalink，title 的取值与前置、后继文档 feature 相关。
  * prev-post 与 next-post 以英文数字、字母和连字符组成，用于生成当前文档的前置文档和后继文档链接。
    * prev-post 的取值应当是实际的前置文档 title + '.html'
    * next-post 的取值应当是实际的后继文档 title + '.html'

### 特别说明

Jekyll 2.5.3 所使用的 Liquid 模板语法在调用变量值时支持使用 filter 例如：`{{ "{{ var1 = var2 | downcase" }}}}`，但有些 filter 存在问题，比如 strip, lstrip, rstrip 三个 filter 工作均不正常。

下一篇文档将介绍具体的 feature。
