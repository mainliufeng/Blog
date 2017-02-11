---
layout:     post
title:      "Quick way using MongoDB for your Java program"
date:       2015-04-06 19:07:00
author:     "mainliufeng"
header-img: "img/post-bg-01.jpg"
---

<p>这是一个简单有效的MongoDB Java编程入门指南。这里会涉及到环境、框架、API、代码，但不会讨论MongoDB的特性、优缺点，也不会讨论高级内容。现在是2015年4月，3月17号，MongoDB发布了3.0.1 Release，所以下面所有的内容都是基于MongoDB 3.0.1和Java驱动3.0.0-SNAPSHOT的。</p>

<h2 class="section-heading">首先要有一个运行起来的MongoDB服务</h2>

<p>我的开发环境是一台13寸Macbook Pro，安装MongoDB有很多中方式可以选择，可以下载<a href="http://www.mongodb.org/">MongoDB官网</a>（啊，这里给出个官网的链接，方便使用Baidu而找不到官网的同学们使用）的dmg安装包来安装，可以使用<a href="http://brew.sh/">Home Brew</a>来安装。</p>

<p>上面两中方式都可以使用，但是我想尽量让我的系统保持干净，而且正好有个工具能实现我这个小小的愿望，所以这里还有个第三种方式：使用Docker创建个容器。如果你会使用Docker，这也是最省事儿的方式。</p>

<p>如何使用Docker创建MongoDB容器，可以参考<a href="https://registry.hub.docker.com/_/mongo/">MongoDB官方Docker文档</a>，或者直接使用<code>docker pull mongo</code>下载Docker最新镜像，和<code>docker run -d -P --name a-mongo-container mongo</code>运行容器。</p>

<p>在Mac上使用Docker需要安装boot2docker、VirtualBox（boot2docker依赖）和docker，也就是说到官网下载VirtualBox安装，然后用brew安装bootdocker和docker。</p>

