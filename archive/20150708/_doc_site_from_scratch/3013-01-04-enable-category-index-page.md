---
layout: post
reader-title: 启用分类汇总页
title: enable-category-index-page
prev-post: create-new-article.html
next-post: enable-sub-artile-listing.html
---

本文档主要讲解如何利用 jekyll 的内置功能自动生成文档的分类汇总页。

1 首先要给分类起一个名字，比如 'mindpin docs'

2 其次要在 `_config.yml` 配置文件中定义一个 `collection`，名为 `categories`, 并且带有以下参数设置：

```
categories:
  output: true
  permalink: /categories/:path/
```

3 再次，在 `_src` 目录中创建一个子目录，名叫 `_categories`

4 第四，在 `_categories` 目录下创建一个 markdown 源码文件，名叫 mindpin-docs.md ，其内容为

```
---
layout: category
---

{{ "{% assign cat-title-array=page.url | split:'/' " }}%}
{{ "{% assign cat-title=cat-title-array[2] | replace:'-',' ' " }}%}
{{ "{% assign cat-obj=site.categories[cat-title] " }}%}

{{ "{% include category-links.html which-cat=cat-obj " }}%}
```

5 第五，因为 mindpin-docs.md 中包含了 category-links.html 文件，所以要在 `_includes` 目录下增加这个文件，其内容为：

```html
<div>
  <ul>
    {{ "{% for post in include.which-cat reversed " }}%}
      <li>
        <b><a class="post-link" href="{{ "{{ post.url | prepend: site.baseurl | prepend: site.url" }}}}">{{ "{{ post.reader-title" }}}}</a></b>
        <!-- 如果 post 对应的文档在 _config.yml 和 _src 目录下
             有对应的 collection 定义，就把它的子文档列举出来。-->
        {{ "{% include coll-selector.html post-url=post.title " }}%}
      </li>
    {{ "{% endfor " }}%}
  </ul>
</div>

```

6 第六，因为 category-links.html 文件包含了 coll-selector.html 所以要在 `_includes` 目录下增加这个文件，其内容为：

```html
{{ "{% assign post-id = include.post-url | replace:'.html','' | replace:'-','_' | downcase " }}%}
{{ "{% if site.collections[post-id].label == post-id " }}%}
  {{ "{% assign coll-obj = site.collections[post-id].docs " }}%}
  {{ "{% include sub-articles.html which-coll=coll-obj " }}%}
{{ "{% endif " }}%}

```
7 第七，因为 coll-selector.html 文件包含了 sub-articles.html 所以要在 `_includes` 目录下增加这个文件，其内容为：

```html
<ul>
  {{ "{% for post in include.which-coll " }}%}
    <li><a class="post-link" href="{{ "{{ post.url | prepend: site.baseurl | prepend: site.url" }}}}">{{ "{{ post.reader-title" }}}}</a></li>
  {{ "{% endfor " }}%}
</ul>
```

#### 特别说明：

分类文章汇总功能是借助模板参数传递机制实现的。include 可以传递参数给被包含的模板，用法如下。
* 在发起包含请求的模板中，使用语法 `{{ "{% include tmpl.html var='abc' " }}%}`，
* 在被包含的模板 tmpl.html 中要用 `{{ "{{ include.var" }}}}` 或者 `{{ "{% include.var " }}%}` 这种形式去访问传进来的参数

这样一来，传入的变量与非传入变量就不会混淆。请参考 请参考 http://jekyllrb.com/docs/templates/ 中的 **ProTip：Use variables as file name** 了解更多细节。

5 最后，在 `_posts` 目录下创建文章，`yyyy-mm-dd-article-name.md` , 其内容为

```
---
layout: post
categories: ['mindpin docs']
reader-title: XXX XXX
title: article-name
prev-post: ...
next-post: ...
---

正文部分
……
```

接下来的文档将介绍其它实用 feature 的实现步骤。

