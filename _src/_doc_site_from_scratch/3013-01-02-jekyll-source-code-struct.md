---
layout: post
reader-title: Jekyll 的源码目录结构
title: jekyll-source-code-dir-struct
prev-post: jekyll-environment-setup.html
next-post: create-new-articles.html
---

为了实现 GitHub Help 与 Facebook React Doc 的若干项实用 feature，需要形成以下目录结构：

下面给出具体操作步骤：

1 从 GitHub 版本库 clone help-doc 示例项目，并切换到 gh-pages 分支

```
git clone https://github.com/pimgeek/help-doc
cd help-doc
checkout gh-pages
```

2 返回到新创建的 `redo-help-doc/` 目录下，严格按顺序执行如下操作，以便大概了解文档源码文件，各个子模板、包含文件、数据文件等组成部分之间的依赖关系。

```
jekyll s -H 0.0.0.0 &
cp ../help-doc/_src/index.md 
# 此时 jekyll 应提示
# Build Warning: Layout 'default' requested in index.md does not exist

mkdir ../help-doc/_src/_layouts
cp ../help-doc/_src/_layouts/default.html _src/_layouts/
# 此时 jekyll 应提示
# Liquid Exception: Included file '_includes/sidebar.html' not found in _layouts/default.html

mkdir ../help-doc/_src/_includes
cp ../help-doc/_src/_includes/sidebar.html _src/_includes/
# 此时 jekyll 应提示
# Liquid Exception: Included file '_includes/search.html' not found in _layouts/default.html

cp ../help-doc/_src/_includes/search.html _src/_includes/
vim _src/_includes/search.html
# 修改其中的这一行，把其中的 "/help-doc/v008" 改为当前 jekyll
# _config.yml 中指定的 "/redo-help-doc/v009"：

<h3>
  <!-- 下列代码中的 /help-doc/v008 部分应与
       _config.yml 中定义的 baseurl 保持一致-->
  <a href="/help-doc/v008{{url}}</a>
</h3>

# 此时 jekyll 无错误提示
# 可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

mkdir _src/_posts
cp ../help-doc/_src/_posts/3012-01-01-help-doc-research.md _src/_posts
# 此时 jekyll 应提示
# Build Warning: Layout 'post' requested in _posts/3012-01-01-help-doc-research.md does not exist.
# Liquid Exception: Included file '_includes/coll-selector.html' not found in not found in _includes/sidebar.html, included in _layouts/default.html

cp ../help-doc/_src/_layouts/post.html _src/_layouts/
cp ../help-doc/_src/_includes/coll-selector.html _src/_includes
# 此时 jekyll 无错误提示
# 可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

vim _config.yml
# 修改 _config.yml，在文档末尾追加以下内容

# 为了让文档 URL 符合 GitHub 的 /articles/xxx.html 形式
permalink: /articles/:title.html

collections:
  # 为了实现分类汇总而设置 category collection
  categories:
    output: true
    permalink: /categories/:path/
  # 下面这些都是为了呈现子文档列表而设置的
  help_doc_research:
    output: true
    permalink: /articles/:title.html

# 保存修改后，执行下面的命令：
jobs
# 接下来，应看到类似如下的提示
# [1]+  Running                 jekyll s -H 0.0.0.0 &
#
# 如果 jekyll 所在的那一行的 Running 前面是 [2]
# 则把下列命令改为 fg %2，以此类推。

fg %1 
# 接下来，应看到类似如下的提示
# jekyll s -H 0.0.0.0

# 点击 Ctrl+C 退出，然后再次执行下列命令
jekyll s -H 0.0.0.0 &
# 此时 jekyll 应提示
# Liquid Exception: Included file '_includes/sub-articles.html' not found in _includes/coll-selector.html, included in _layouts/post.html

cp ../help-doc/_src/_includes/sub-articles.html _src/_includes/
jekyll s -H 0.0.0.0
# 此时 jekyll 无错误提示
# 可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

cp ../help-doc/_src/_help_doc_research/* _src/_help_doc_research/
# 此时 jekyll 无错误提示
# 等待 5 秒钟左右，打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

# 接下来，尝试在浏览器中打开以下网页
# http://192.168.1.32:4000/redo-help-doc/v009/categories/mindpin-docs
# 此时浏览器应提示
# Not Found
#
# `/redo-help-doc/v009/categories/mindpin-docs' not found.

mkdir _src/_categories
cp ../help-doc/_src/_categories/mindpin-docs.md _src/_categories/
# 此时 jekyll 应提示
# Liquid Exception: Included file '_includes/category-links.html' not found in _categories/mindpin-docs.md

cp ../help-doc/_src/_includes/category-links.html _src/_includes/
# 此时 jekyll 应提示
# Build Warning: Layout 'category' requested in _categories/mindpin-docs.md does not exist.

cp ../help-doc/_src/_layouts/category.html _src/_layouts/
# 此时 jekyll 无错误提示
# 接下来等待 5 秒钟左右，再次尝试在浏览器中打开以下网页
# http://192.168.1.32:4000/redo-help-doc/v009/categories/mindpin-docs
# 此时应当在网页的右半部分看到如下内容
# Category / Mindpin docs
# * Help Doc 相关调研
#   * 用 Jekyll 实现纯前端搜索
#   * GitHub Help 内容架构
#   * 用 Jekyll 模拟 GitHub Help
#   * conref 使用效果展示

# 接下来，尝试打开下面的网页
# http://192.168.1.32:4000/redo-help-doc/v009/articles/conref-usage-sample.html
# 此时应当在网页的右半部分看到如下内容
# 下列文字使用了 conrefs 引用：
# ----
# 子文档列表
# ----
# <- prev

mkdir _src/_data
cp ../help-doc/_src/_data/conrefs.yml _src/_data/
# 此时 jekyll 无错误提示
# 接下来等待 5 秒钟左右，再次尝试在浏览器中打开以下网页 http://192.168.1.32:4000/redo-help-doc/v009/articles/conref-usage-sample.html
# 网页载入完毕后，将能够看到与上一次浏览此页时，明显不一样的内容
#
# 下列文字使用了 conrefs 引用：
# 这段文字来自 jekyll 源码目录下的 _data 目录下的 conrefs.yml。
# ……
#

# 接下来，尝试在网页右半部分的搜索框中搜索 facebook 关键字
# 回车确认后，将打开以下网页超链接：
# http://192.168.1.32:4000/redo-help-doc/v009/search?q=facebook
# 此时浏览器应显示以下内容：
# Not Found
#
# `/redo-help-doc/v009/search' not found.

mkdir _src/search
cp ../help-doc/_src/search/index.html _src/search/

# 此时 jekyll 应提示
# Build Warning: Layout 'page' requested in search/index.html does not exist.

cp ../help-doc/_src/_layouts/page.html _src/_layouts/
mkdir _src/_plugins
cp ../help-doc/_src/_plugins/jekyll_lunr_js_search.rb _src/_plugins
mkdir _src/js
cp ../help-doc/_src/js/jquery.lunr.search.js _src/js/
cp ../help-doc/_src/js/lunr.min.js _src/js/
cp ../help-doc/_src/js/search.min.js _src/js/

# 完成上述操作后，执行下面的命令：
jobs
# 接下来，应看到类似如下的提示
# [1]+  Running                 jekyll s -H 0.0.0.0 &
#

fg %1 
# 接下来，应看到类似如下的提示
# jekyll s -H 0.0.0.0

# 点击 Ctrl+C 退出，然后再次执行下列命令
jekyll s -H 0.0.0.0 &

# 此时 jekyll 无错误提示
# 接下来等待 5 秒钟左右，再次尝试在网页右半部分的搜索框中
# 搜索 facebook 关键字。回车确认后，将打开以下网页超链接：
# http://192.168.1.32:4000/redo-help-doc/v009/search?q=facebook
# 此时浏览器应显示以下内容：
# Search results
#
# help-doc-research
# 其中，help-doc-research 是一个超链接，点击后
# 将进入包含 facebook 关键词的文档页面。

```

到目前为止，以上操作命令已经把涉及到 **/article/xxx 格式的文档链接，文档分类汇总页，子文档列表页，lunrjs 纯前端搜索，conref 内容复用，GitHub Flavored Markdown 支持** 等 6 项 feature 的代码文件都触及了。

前置和后继文档的功能相对简单，只要在 `_config.yml` 文件中定义好 permalink 格式，再在模板源码中利用每篇文档的 Front Matter slug —— `title`, `prev-post` 和 `next-post` 等字段动态创建超链接即可实现。

从下一篇文档开始，我将把 7 种 feature 的复现步骤单独列出来，以便读者孤立地复现其中的一部分 feature。
