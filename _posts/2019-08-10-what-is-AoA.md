---
layout:     post
title:      "Bert加AoA有用吗"
date:       2019-08-10 13:59:00
author:     "mainliufeng"
header-img: "img/AoA-head.png"
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### Bert

Bert大家应该知道，就是Google的《*BERT: Pre-training of Deep Bidirectional Transformers forLanguage Understanding*》这篇文章，Google凭借这篇文章刷了很多榜，并且发布了中文和多语言的与训练模型，使得没有太高算力的小公司也可以用这个与训练模型实现比较大的提升；现在Bert在工业界算是应用比较多了。

### AoA

之前看SQuAD发现有一个*BERT + DAE + AoA*，一直没有找到*AoA*是什么意思，前几天在知乎的一篇文章上面发现*AoA*是*Attention-over-Attention*的缩写，Google一下找到了另一个知乎的文章和[Attention-over-Attention Neural Networks for Reading Comprehension](https://arxiv.org/pdf/1607.04423.pdf)这篇论文，知乎的文章不说了，介绍的不是特别详细没看懂，所以直接看了论文。

那么AoA到底是什么东西，简单的说就是下面这张图：

<a href="#">
    <img src="{{ site.baseurl }}/img/AoA.png" alt="AoA">
</a>

我们先看左边红框的部分（图是论文里边的，框是我画的）。

输入就是两个Tensor，一个Document一个是Query，为什么有两列，是因为论文中AoA这层前面接的是一个双向的RNN，两列分别是正向和反向RNN的隐藏状态。

然后就是Document和Query点积得到一个相似度矩阵，这个矩阵大小是D乘Q，*M*的每一列是Document的每一个词，每一行是Query的每一个词，矩阵的每一项在论文中叫*Pair-wise Matching Score*：

$$M(i,j) = h_{doc}(i)^{T} \cdot h_{query}(j)$$

接下来就是重点了，如图，分别对列、行进行softmax，得到了*query-to-document* attention和*document-to-query* attention；

### 对列softmax，*query-to-document* attention

对于Query中每一个时刻*t*的词，将这一列的score取softmax，也就是这个Query中当前这个词，对Document中每一个词关注的概率：

$$\alpha(t) = softmax(M(1,t),...,M(\|D\|,t))$$

$$\alpha = [\alpha(1),\alpha(2),...,\alpha(|Q|)]$$

### 对行softmax，*document-to-query* attention

对称的，对于Document中每一个时刻*t*的词，将这一行的score取softmax，也就是这个Document中当前词，对Query中每一个词关注的概率：

$$\beta(t) = softmax(M(t,1),...,M(t,\|Q\|))$$

$$\beta = [\beta(1),\beta(2),...,\beta(|D|)]$$

### 对比Attention Is All You Need

[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)中，的注意力公式是：

$$Attention(Q,K,V) = softmax(\frac{QK^{T}}{\sqrt{d_{k}}})V$$

这个在论文中叫*Scaled Dot-Product Attention*，如果去掉*Scaled*和*V*，就变成了：

$$softmax(QK^{T})$$

和上面的*query-to-document* attention、*document-to-query* attention其中一个是一样的；至于和哪个一样的，如果我们把*K*换做用*D*表示，而且softmax默认的维度是-1，也就是*D*；这样这个公式和上面的*query-to-document* attention就是一样的。

也就是说，*Attention Is All You Need*、*Transformer、Bert这些模型都只有* query-to-document* attention

### Attention over attention

上面图中右边的红框，是模型分成两个Attention之后的处理，直接把*document-to-query* attention在列上取了平均值，每一列是一个Document，相当于是对Document中每一个单词，对Query的attention取了平均数，也就是整个文档对Query中每一个词的Attention；大小是Q。

然后用，*query-to-document* attention和上面的*整个文档对Query的Attention*相乘，得到最终的*“attended  document-level  attention”*，这个相当于Query中每一个词，对Document的*query-to-document* attention，不是直接取平均数，而是根据整个文档对Query中每个词的Attention来计算一个加权平均数。

### AoA有没有用

Bert上加AoA有没有提升：[有一篇论文](http://web.stanford.edu/class/cs224n/reports/default/15845024.pdf)中写到*However, we found that adding AoA did not improve the performance ofBERT. We thought this is because the BERT Transformer already uses bidirectional self-attention, sono additional attention is needed for BERT.*；

Bert加AoA为什么没有提升：这个上面已经说出来了，Bert没有AoA但是Bert的输入是两个句子的拼接做self attention，相当于query to document和document to query已经都有了，原来以为是简单粗暴，现在看还是比较巧妙的。

但是哈工大和讯飞确实用AoA刷了几次SQuAD2.0，这个也没有找到代码，或许姿势不一样？或许有一些不是很大的提升？这个“BERT + DAE + AoA”的模型已经用了很多次了，或许只是没有做去掉AoA的尝试，在其他地方做了改进。
