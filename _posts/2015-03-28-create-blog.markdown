---
layout:     post
title:      "使用Github Pages和Jekyll创建个人博客"
date:       2015-03-28 22:46:00
author:     "codeburger"
header-img: "img/post-bg-01.jpg"
---

<p>Github Pages 依靠 Github 上项目的某些特定分支来工作。Github Pages 分为两种基本类型：用户/组织的站点和项目的站点。搭建这两种类型站 点的方法除了一小些细节之外基本一致。</p>

<h2 class="section-heading">用户和组织的站点</h2>

<p>用户和组织的站点被放置在一个特殊的专用仓库中，在该仓库中只存在 Github Pages 的相关文件。这个仓库应该根据用户/组织的名称来命名， 例如： <a href="https://github.com/mojombo/mojombo.github.io">@mojombo 的用户站点仓库</a>的用户站点仓库 应该被命名为<code>mojombo.github.io</code> 。</p>

<p>仓库中<code>master</code>分支里的文件将会被用来生成 Github Pages 站点，所以请 确保你的文件储存在该分支上。</p>

<h2 class="section-heading">项目的站点</h2>

<p>不同于用户和组织的站点，项目的站点文件存放在项目本身仓库的<code>gh-pages</code>分支中。该分支下的文件将会被 Jekyll 处理，生成的站点会被 部署到你的用户站点的子目录上，例如<code>username.github.io/project</code>（除非指定了一个自定义的域名）。</p>

<p>Jekyll 项目本身就是一个很好的例子，Jekyll 项目的代码存放在<code>master</code>分支 ， 而 Jekyll 的项目站点（就是你现在看见的网页）包含在同一仓库的<code>gh-pages</code>分支中。</p>

<h2 class="section-heading">安装Jekyll</h2>

<p>Github上的<a href="https://help.github.com/articles/using-jekyll-with-pages/#installing-jekyll"><code>Jekyll帮助</code></a>，对Jekyll的安装描述的很详细</p>

<h2 class="section-heading">创建站点</h2>

<p>自己创建站点可以参考<a href="http://jekyllcn.com/docs/pages/"><code>Jekyll文档</code></a>和<a href="https://help.github.com/articles/using-jekyll-with-pages/#configuring-jekyll"><code>Github帮助</code></a>
</p>

<p>也可以到<a href="http://jekyllthemes.org/"><code>Jekyll Theme</code></a>下载主题使用</p>

<a href="#">
    <img src="{{ site.baseurl }}/img/jekyll-theme.png" alt="Jekyll Theme">
</a>