---
layout: post
reader-title: 用 Jekyll 实现纯前端搜索
title: jekyll-frontend-search
prev-post: help-doc-research.html
next-post: github-help-doc-clone-with-jekyll.html
---

【目标】尝试把 jekyll 与 lunrjs 串接起来使用，实现纯前端全文搜索
【GTD 结果】已走通，只是不支持中文搜索

#### 验收标准：
1 要说明每个尝试步骤的要点
2 要说明尝试实际操作的结果如何
3 要把已经走通的步骤中比较关键的操作细节记录下来

#### jekyll + lunrjs 操作步骤

**特别说明**：我只是按下列方法调通了，但更多细节还需参考原始网站 - https://github.com/slashdotdash/jekyll-lunr-js-search

01 - gem install jekyll 

02 - gem install jekyll-lunr-js-search

03 - jekyll new lunr

04 - 编辑 `_config.yml` ，增加下列代码

```yaml
gems: ['jekyll-lunr-js-search']
```
05 - 在 `_include/` 目录下创建 search.html 模板

```html
<div id="search">
  <form action="{{ "{{ site.baseurl" }}}}/search" method="get">
    <input type="text" id="search-query" name="q" placeholder="Search" autocomplete="off">
  </form>
</div>
<section id="search-results" style="display: none;">
  <p>Search results</p>
  <div class="entries">
  </div>
</section>
{{ "{% raw" }}%}
<script id="search-results-template" type="text/mustache">
  {{ "{{#entrie"s}}}}
    <article>
      <h3>
        {{ "{{#dat"e}}}}<small><time datetime="{{ "{{pubdat"e}}}}" pubdate>{{displaydate}}</time></small>{{/date}}
        <a href="{{{ site.baseurl }}}{{ "{{ur"l}}}}">{{ "{{titl"e}}}}</a>
      </h3>
    </article>
  {{ "{{/entrie"s}}}}
</script>
{{ "{% endraw" }}%}
<script src="{{ "{{ site.baseurl" }}}}/js/search.min.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript">
  $(function() {
    $('#search-query').lunrSearch({
      indexUrl: '{{ "{{ site.baseurl" }}}}/js/index.json',   // Url for the .json file containing search index data
      results : '#search-results',  // selector for containing search results element
      entries : '.entries',         // selector for search entries containing element (contained within results above)
      template: '#search-results-template'  // selector for Mustache.js template
    });
  });
</script>
```

06 - 在需要提供搜索功能的模板中增加下列代码

```html
{{ "{% include search.html" }}%}
```

07 - 创建 search/ 目录并创建搜索页模板 index.html

```html
---
layout: page
title: Search
---

{{ "{% include search.html" }}%}
```

08 - 在 `_posts/` 目录下放置一些 markdown 文档
例如：yyyy-mm-dd-postname.md

09 - 执行 jekyll serve -H 0.0.0.0 预览效果
例如：http://192.168.1.32/

10 - 部署到 github pages（project 类站点）前的准备
修改 `_config.yml`

`baseurl: "/help-doc/v001" `
注释掉 `url:` 这一行
然后修改以下模板文件 `_include/search.html` 中的相关路径
特别是 `{{ "{{ur"l}}}}`这一行要改为

```html
<a href="/help-doc/v001{{ "{{ur"l}}}}">{{ "{{titl"e}}}}</a>
```
再次执行 `jekyll serve -H 0.0.0.0` 预览效果
例如：http://192.168.1.32/help-doc/v001/

11 创建一个新的版本库 https://github.com/username/help-doc
然后先 clone 到本地，再创建并切换到 gh-pages 分支，再
在其中创建 v001 目录。

12 把 `_site` 中的静态文件完全复制到 help-doc 源码库的 v001 目录下
执行 `git add / commit / push origin gh-pages` 等命令
稍等片刻，就可以用 yourname.github.io/help-doc/v001 访问之。
例如：http://pimgeek.github.io/help-doc/v001/
简单试用过后，发现 lunrjs 无力对中文内容做索引

有个号称支持中文分词的 luna.js 我曾试着替换 jekyll 项目中的 js 文件，但没有调通。具体步骤略。

#### 参考资料

Jekyll 的 lunajs 搜索插件 - https://github.com/slashdotdash/jekyll-lunr-js-search
一个使用 lunrjs 的典型博客 - https://github.com/rohit01/rohit01.github.io
一个号称支持中文分词的 lunr.js  - https://github.com/nandy007/lunr.js

