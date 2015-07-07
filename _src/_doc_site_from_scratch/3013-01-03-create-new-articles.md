---
layout: post
reader-title: 创建新文档
title: create-new-articles
prev-post: jekyll-environment-setup.html
---

- 不带子文档的文档，直接以 markdown 源码形式放入 `help-doc/_posts` 下面

    `help-doc/_posts` 目录下的 Front Matter 必须遵循以下格式： 其中，reader-title 可以包含中文字符，title 必须是英文数字、字母和连字符组成

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

### 特别说明

Jekyll 2.5.3 所使用的 Liquid 模板语法在调用变量值时支持使用 filter 例如：`{{ "{{ var1 = var2 | downcase" }}}}`，但有些 filter 存在问题，比如 strip, lstrip, rstrip 三个 filter 工作均不正常。

