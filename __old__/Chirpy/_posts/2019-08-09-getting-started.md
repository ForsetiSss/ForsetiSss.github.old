---
title: Getting Started：使用Jekyll创建GitHub Pages站点搭建博客（Windows）
author: Shi Daming
date: 2021-03-01 17:00:00 +0800
categories: [Blogging, Tutorial]
tags: [getting started]
pin: true
---

### 一、必要组件的安装

需要首先安装以下内容

- **Ruby** - 是一种面向对象、命令式、函数式、动态的通用编程语言。Windows下直接下载安装即可[Ruby installer](https://rubyinstaller.org/)。注意，如遇报错，可能需要将安装目录（xxx\Ruby27-x64\bin）加入系统环境变量PATH。

- **Bundler** - 是一个官方推荐的Ruby gem包管理软件，可以减少Jekyll的编译错误，和环境依赖相关的bug，相关链接[Bundler](https://bundler.io/)。只需在终端中运行以下两步：

  1.安装bundler

  ```
  gem install bundler
  ```

  2.指定源

  ```
  gem sources 'https://rubygems.org'
  ```

- **Jekyll** -相当于一个编译工具，安装好jekyll后，你可以通过jekyll创建一个网站模板从而在本地进行预览，而不用上传至Github后再进行查看，此软件包将在之后的步骤中自动安装。

### 二、网站托管（在Github上创建自己的repo）

github.io是完全基于github创建的，其本质上是在你的github账户下创建一个特殊的repo。你可以参照如下步骤完成：

- 创建repo

  登陆你的账户后，创建一个新的repo。请务必注意该repo的名字，必须保持格式`<username>.github.io`，其中 `<username） 替换成你的github账户名。

- 把repo clone到本地

  在目录下新建一个index.html的文件,在里面输入任意内容，然后再把代码push送到github上。

- 测试地址

  浏览器里访问<u>https://(username).github.io/</u>,可以发现这个url可以被访问了。

到这里，一个免费且无限流量的Github代码托管仓库就创建完成了。

### 三、基于Jekyll模板建立网站

为了在发布前预览自己的网站，我们需要使用Jekyll，同时它也能为我们提供很多模板。

1. 首先点击前往[jekyll 主题官网](http://jekyllthemes.org/)。

2. 然后选择一个自己喜欢的主题模板，比如[Chirpy](http://jekyllthemes.org/themes/jekyll-theme-chirpy/)。

3. 点击Download下载该模板至repo的本地目录，注意确保根目录下含有Gemfile这个文件。

   <img src="/assets/img/QQ截图20210308194033.png"/>

   注意插入图片时需要将图片文件放入assets文件夹，在md文件中由于路径不一致会无法正常显示，在网页中是正常的，无须担心。

4. 在本地repo目录下打开命令窗口并运行

   ```
   bundle install
   ```

   自动安装所有依赖的环境

5. 最后一步，开启测试

   ```
   bundle exec jekyll serve
   ```

   成功运行之后，我们就可以通过http://127.0.0.1:4000/在本地访问创建的网站了。此时可以随时更改网站内容，并能实现自动实时更新。

### 四、Jekyll目录结构

​	Jekyll使用Ruby脚本根据模板生成静态网页，实现了内容与排版的分离。模板以嵌入[Liquid](https://shopify.github.io/liquid/)脚本的HTML格式存放。内容为markdown或者html。为了撰写自己的博客，需要对模板内容进行修改。

Jekyll模板通常包含的目录结构：

```
    _posts 博客内容
    _pages 其他需要生成的网页，如About页
    _layouts 网页排版模板
    _includes 被模板包含的HTML片段，可在_config.yml中修改位置
    assets 辅助资源 css布局 js脚本 图片等
    _data 动态数据
    _sites 最终生成的静态网页
    _config.yml 网站的一些配置信息
    index.html 网站的入口
```

1. 我们打开根目录下的index.html可以看到：

   ```html
   ---
   layout: home
   # Index page
   ---
   ```

2. 上面的home我们到_layouts目录下可以找到：

   ```html
   ---
   layout: page
   # The Home page layout
   ---
   
   {% raw %}
   {% assign pinned = site.posts | where_exp: "item", "item.pin == true"  %}
   {% assign default = site.posts | where_exp: "item", "item.pin != true"  %}
   {% assign posts = "" | split: "" %}
   ...
      <div class="post-meta text-muted d-flex justify-content-between">
   
         <div>
           <!-- posted date -->
           <i class="far fa-calendar fa-fw"></i>
           {% include timeago.html date=post.date tooltip=true %}
   
           <!-- time to read -->
           <i class="far fa-clock fa-fw"></i>
           {% include read-time.html content=post.content %}
   
           <!-- page views -->
           {% if site.google_analytics.pv.enabled %}
           <i class="far fa-eye fa-fw"></i>
           <span id="pv_{{-post.title-}}" class="pageviews">
             <i class="fas fa-spinner fa-spin fa-fw"></i>
           </span>
           {% endif %}
         </div>
   
         {% if post.pin %}
         <div class="pin">
           <i class="fas fa-thumbtack fa-fw"></i>
           <span>{{ site.data.label.pin_prompt | default: 'Pinned' }}</span>
         </div>
         {% endif %}
   
       </div> <!-- .post-meta -->
   
     </div> <!-- .post-review -->
   
   {% endfor %}
   
   </div> <!-- #post-list -->
   
   {% if paginator.total_pages > 0 %}
     {% include post-paginator.html %}
   {% endif %}
   {% endraw %}
   ```

   实际上根目录下index.html运行后是home里面的代码内容，1中**html代码段**会填充的上图中的**content**位置。jekyll是将分散在各个目录下的html文件拼接起来运行。

3. 上图的page布局也可以在_layouts目录下找到：

   ```html
   ---
   layout: default
   # The page layout
   ---
   {% raw %}
   <div class="row">
     <div class="col-12 col-lg-11 col-xl-8">
       <div id="page" class="post pb-5 pl-1 pr-1 pl-sm-2 pr-sm-2 pl-md-4 pr-md-4 mb-md-4">
       {% if page.dynamic_title %}
         <h1 class="dynamic-title">{{ page.title }}</h1>
         <div class="post-content">
           {{ content }}
         </div>
       {% else %}
         {{ content }}
       {% endif %}
       </div> <!-- #page -->
     </div><!-- .col-12 -->
   
     {% include panel.html %}
   
   </div>
   
   {% if site.disqus.comments and page.comments %}
   <div class="row">
     <div class="col-12 col-lg-11 col-xl-8">
       <div class="pl-1 pr-1 pl-sm-2 pr-sm-2 pl-md-4 pr-md-4">
   
       {% include disqus.html %}
   
       </div> <!-- .pl-1 pr-1 -->
     </div> <!-- .col-12 -->
   </div> <!-- .row -->
   {% endif %}
   {% endraw %}
   ```

   关于Jekyll的更详细用法可以参考[官方网站](http://jekyllcn.com/docs/home/)

   ps: 

   - GitHub Page 的（一种）输入是 markdown 文件，输出是 HTML/CSS/JS 文件。

   - 如果 markdown 文件包含代码块，且代码块中包含花括号 { 或 }，尤其是包含 { % 或 { { 符号组合时，GitHub Page 会报错。

   - 在代码前后添加{ % raw % }和{ % endraw % }（去掉符号间空格）