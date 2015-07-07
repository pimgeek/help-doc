---
layout: post
reader-title: 设置 Jekyll 运行环境
title: jekyll-environment-setup
prev-post: doc-site-from-scratch.html
next-post: jekyll-source-code-dir-struct.html
---

设置 jekyll 运行环境的大致步骤如下：

1 gem install jekyll

    我使用的 Ruby 和 Jekyll 环境如下： Jekyll 2.5.3 , ruby 2.2.0p0 (2014-12-25 revision 49005) [x86_64-linux]）

2 确保 jekyll 安装无误后，按序进行以下操作

```
mkdir redo-help-doc
mkdir redo-help-doc/_src
mkdir redo-help-doc/v009
vim redo-help-doc/_config.yml

# 修改 `redo-help-doc/_config.yml` 文件的内容，加入以下配置：
baseurl: "/redo-help-doc"
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

3 在 `redo-help-doc` 目录下执行 `jekyll s -H 0.0.0.0` ，然后打开网址 `http://192.168.1.32:4000/redo-help-doc/v009/` 进行预览

#### 特别说明

* 我使用的文档生成方法是在本地 `redo-help-doc/v009/` 目录下生成所有静态文件，然后直接把静态文件 push 到 github 版本库的 gh-pages 分支。这样做等于绕过了 github pages 内置的 jekyll 源码编译的机制。如果你不希望绕开这个机制，请参考 [GitHub Pages 官方帮助文档](https://help.github.com/articles/using-jekyll-with-pages/)

* `_config.yml` 中的 `exclude: ['dir']` 设置可以阻止 github pages 输出 `dir` 目录下的静态网页。利用这个机制，可以把 `redo-help-doc/` 目录中一些不相干的内容屏蔽，避免输出到 `username.github.io/redo-help-doc/...` 。

