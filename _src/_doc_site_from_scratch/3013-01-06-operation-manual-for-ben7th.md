---
layout: post
reader-title: 给宋亮写的操作步骤说明
title: operation-manual-for-ben7th
---

设置 jekyll 文档站运行环境的大致步骤如下：

### 1 启动虚拟机并执行下面的命令

```
gem install jekyll
```

    我使用的 Ruby 和 Jekyll 环境如下： Jekyll 2.5.3 , ruby 2.2.0p0 (2014-12-25 revision 49005) [x86_64-linux]）

### 2 创建 jekyll 工作目录

```
mkdir redo-help-doc
mkdir redo-help-doc/_src
mkdir redo-help-doc/v009
```

### 3 创建 jekyll 配置文件 `_config.yml`

``` 
vim redo-help-doc/_config.yml

# 修改 `redo-help-doc/_config.yml` 文件的内容，加入以下配置：
baseurl: "/redo-help-doc/v009"
source: "_src"
destination: "v009"
markdown: redcarpet
redcarpet:
  extensions:
  - fenced_code_blocks
  - footnotes
sass:
  style: :compressed
  sass_dir: _css
```

#### 特别说明

* 因为 lunrjs 是作为 jekyll 插件的形式发挥作用的，而 github pages 目前不支持带插件输出静态网页，所以我选择在本地 `redo-help-doc/v009/` 目录下生成所有静态文件，然后直接把静态文件 push 到 github 版本库的 gh-pages 分支。


### 4 从 GitHub 版本库 clone help-doc 示例项目，并切换到 gh-pages 分支

```
git clone https://github.com/pimgeek/help-doc
cd help-doc
checkout gh-pages
```

#### 特别说明

* 如果只是想把 clone 站运行起来，那么直接在 help-doc 目录下执行 `jekyll s -H 0.0.0.0` 即可；如果想了解每个 feature 对应的必要操作，请继续阅读。

接下来，返回到 `redo-help-doc/` 目录下操作。

### 5 复制 `help-doc/_src` 目录下的 index.md  

```
cd ../redo-help-doc/
jekyll s -H 0.0.0.0 &
cp ../help-doc/_src/index.md _src/
```

后台启动 jekyll 以便继续执行前台操作，此时 jekyll 应提示

    Build Warning: Layout 'default' requested in index.md does not exist

### 6 创建 index.md 所依赖的 default.html

```
mkdir _src/_layouts
cp ../help-doc/_src/_layouts/default.html _src/_layouts/
```

此时 jekyll 应提示

    Liquid Exception: Included file '_includes/sidebar.html' not found in _layouts/default.html

### 7 创建 default.html 所依赖的 sidebar.html

```
mkdir _src/_includes
cp ../help-doc/_src/_includes/sidebar.html _src/_includes/
```

此时 jekyll 应提示

    Liquid Exception: Included file '_includes/search.html' not found in _layouts/default.html

### 8 创建 siderbar.html 所依赖的 search.html

```
cp ../help-doc/_src/_includes/search.html _src/_includes/
vim _src/_includes/search.html

修改其中的这一行，把其中的 "/help-doc/v008" 改为当前 jekyll
# _config.yml 中指定的 "/redo-help-doc/v009"：

<h3>
  <!-- 下列代码中的 /help-doc/... 部分应与
       _config.yml 中定义的 baseurl 保持一致-->
  <a href="/help-doc/v008{{url}}</a>
</h3>
```

此时 jekyll 无错误提示

可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

### 9 创建一个示例文档

```
mkdir _src/_posts
cp ../help-doc/_src/_posts/3012-01-01-help-doc-research.md _src/_posts
```

此时 jekyll 应提示

    Build Warning: Layout 'post' requested in _posts/3012-01-01-help-doc-research.md does not exist.
    Liquid Exception: Included file '_includes/coll-selector.html' not found in not found in _includes/sidebar.html, included in _layouts/default.html

### 10 创建示例文档所依赖的 post.html 和 coll-selector.html

```
cp ../help-doc/_src/_layouts/post.html _src/_layouts/
cp ../help-doc/_src/_includes/coll-selector.html _src/_includes
```

此时 jekyll 无错误提示

可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

### 11 修改 `_config.yml` 使之输出 `categories/mindpin-docs` 这样的分类索引入口网址

```
vim _config.yml
# 修改 _config.yml，在文档末尾追加以下内容

# 为了让文档 URL 符合 GitHub 的 /articles/xxx.html 形式
permalink: /articles/:title.html

collections:
  # 为了实现分类汇总而设置 category collection
  categories:
    output: true
    permalink: /categories/:path/
  # 下面这些是为了呈现子文档列表而设置的
  help_doc_research:
    output: true
    permalink: /articles/:title.html
```

保存修改后，重新后台启动 jekyll，此时 jekyll 应提示

    Liquid Exception: Included file '_includes/sub-articles.html' not found in _includes/coll-selector.html, included in _layouts/post.html

### 12 创建 coll-selector.html 所依赖的 sub-articles.html

```
cp ../help-doc/_src/_includes/sub-articles.html _src/_includes/
```

操作完毕后，重新后台启动 jekyll，此时 jekyll 无错误提示

可以打开 http://192.168.1.32:4000/redo-help-doc/v009/ 查看效果

### 13 从 `help-doc/_src` 目录下复制一系列子文档，以便测试子文档列表的显示是否正常

```
mkdir _src/_help_doc_research
cp ../help-doc/_src/_help_doc_research/* _src/_help_doc_research/
```

接下来等待 5 秒钟左右，然后打开以下网页超链接：

http://192.168.1.32:4000/redo-help-doc/v009/articles/articles/help-doc-research.html

在网页右侧将看到如下所示的子文档列表：

    子文档列表
    * 用 Jekyll 实现纯前端搜索
    * GitHub Help 内容架构
    * 用 Jekyll 模拟 GitHub Help
    * conref 使用效果展示

### 14 创建分类索引的实际内容页

```
mkdir _src/_categories
cp ../help-doc/_src/_categories/mindpin-docs.md _src/_categories/
```

此时 jekyll 应提示

    Liquid Exception: Included file '_includes/category-links.html' not found in _categories/mindpin-docs.md



### 15 创建分类索引内容页所依赖的 category-links.html 和 category.html

```
cp ../help-doc/_src/_includes/category-links.html _src/_includes/
```

此时 jekyll 应提示

    Build Warning: Layout 'category' requested in _categories/mindpin-docs.md does not exist.

```
cp ../help-doc/_src/_layouts/category.html _src/_layouts/
```

此时 jekyll 无错误提示

接下来等待 5 秒钟左右，再次尝试在浏览器中打开以下网页

http://192.168.1.32:4000/redo-help-doc/v009/categories/mindpin-docs

此时应当在网页的右半部分看到如下内容

    Category / Mindpin docs
    * Help Doc 相关调研
      * 用 Jekyll 实现纯前端搜索
      * GitHub Help 内容架构
      * 用 Jekyll 模拟 GitHub Help
      * conref 使用效果展示

### 16 创建 conrefs 内容复用 feature 所依赖的 conrefs.yml

```
mkdir _src/_data
cp ../help-doc/_src/_data/conrefs.yml _src/_data/
```

此时 jekyll 无错误提示

接下来等待 5 秒钟左右，尝试在浏览器中打开以下网页

http://192.168.1.32:4000/redo-help-doc/v009/articles/conref-usage-sample.html

网页载入完毕后，应当显示如下内容

    下列文字使用了 conrefs 引用：
    这段文字来自 jekyll 源码目录下的 _data 目录下的 conrefs.yml。
    ……
    

### 17 创建独立的搜索页

```
mkdir _src/search
cp ../help-doc/_src/search/index.html _src/search/
```

此时 jekyll 应提示

    Build Warning: Layout 'page' requested in search/index.html does not exist.

### 18 创建搜索页所依赖的 page.html，以及相关的代码文件 

```
cp ../help-doc/_src/_layouts/page.html _src/_layouts/
mkdir _src/_plugins
cp ../help-doc/_src/_plugins/jekyll_lunr_js_search.rb _src/_plugins
mkdir _src/js
cp ../help-doc/_src/js/jquery.lunr.search.js _src/js/
cp ../help-doc/_src/js/lunr.min.js _src/js/
cp ../help-doc/_src/js/search.min.js _src/js/
```

### 18 重启 jekyll 并测试搜索页

重启 jekyll s

接下来等待 5 秒钟左右，再次尝试在网页右半部分的搜索框中

搜索 facebook 关键字。回车确认后，将打开以下网页超链接：

http://192.168.1.32:4000/redo-help-doc/v009/search?q=facebook

此时浏览器应显示以下内容：

    Search results
    
    help-doc-research

其中，help-doc-research 是一个超链接，点击后将进入包含 facebook 关键词的文档页面。

#### 特别说明

有两种引入 lunrjs 的方式，其一是利用 jekyll-lunr-js-search 插件，在 `_config.yml` 的 gems: 配置中增加，其优点是无需手动复制 js 静态文件到相关目录，只要增加模板代码即可；缺点是插件代码不一定是最新版本，而且不易调整

我采用的是另一种方式，直接 clone lunrjs github 版本库，从其中的 dist 目录复制最新版代码到 `redo-help-doc/_src/` 的相关目录下。**而且为了适应 jekyll 的 baseurl，我还修改了 `jekyll_lunr_js_search.rb` 文件中的部分代码**

更多详情请参考这里：https://github.com/slashdotdash/jekyll-lunr-js-search

另外，lunrjs 还有两个比较严重的问题尚未解决，我做过初步尝试但暂时没有调通。

- lunrjs 在对文件做索引时，对中文分词支持极差。
- lunrjs 官方插件代码只支持对 `_posts` 目录下的文档做索引，这导致类似 `_help_doc_research` 子文档目录下的文档无法被索引到。

### 19 创建优化网页显示效果的静态文件

最后，把用于提升显示效果的图片与 Sass/CSS 等静态文件复制过来

```
cp -r ../help-doc/_src/css _src/
cp -r ../help-doc/_src/_css _src/
cp -r ../help-doc/_src/img _src/
```

接下来等待 5 秒钟左右，然后打开以下网页超链接：

http://192.168.1.32:4000/redo-help-doc/v009/

http://192.168.1.32:4000/redo-help-doc/v009/articles/jekyll-frontend-search.html

将会看到增加了皮肤和代码高亮效果的网页

以上操作命令已经把涉及到 **/article/xxx 格式的文档链接，文档分类汇总页，子文档列表页，lunrjs 纯前端搜索，conref 内容复用，GitHub Flavored Markdown 支持** 等 6 项 feature 的代码文件都触及了。

前置和后继文档的功能相对简单，只要在 `_config.yml` 文件中定义好 permalink 格式，再在模板源码中利用每篇文档的 Front Matter slug，也就是 `title`, `prev-post` 和 `next-post` 等字段动态创建超链接即可实现。

最终形成的目录结构如下： 

```
redo-help-doc/
├── _src
│   ├── _categories
│   │   └── mindpin-docs.md
│   ├── css
│   │   └── ...
│   ├── _css
│   │   └── ...
│   ├── _data
│   │   └── conrefs.yml
│   ├── _help_doc_research
│   │   └── yyyy-mm-dd-xxx-xxx-xxx.md
│   ├── img
│   │   └── ...
│   ├── _includes
│   │   ├── category-links.html
│   │   ├── coll-selector.html
│   │   ├── search.html
│   │   ├── sidebar.html
│   │   └── sub-articles.html
│   ├── js
│   │   ├── jquery.lunr.search.js
│   │   ├── lunr.min.js
│   │   └── search.min.js
│   ├── _layouts
│   │   ├── category.html
│   │   ├── default.html
│   │   ├── page.html
│   │   └── post.html
│   ├── _plugins
│   │   └── jekyll_lunr_js_search.rb
│   ├── _posts
│   │   └── yyyy-mm-dd-xx-yyy-zzzz.md
│   ├── search
│   │   └── index.html
│   └── index.md
└── _config.yml

```
