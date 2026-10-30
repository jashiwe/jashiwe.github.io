---
title: "文章模板"
date: 2026-10-30
author: "张献伟"
tags: ["示例"]
draft: false
---
<span class="todo">【导语一段】用一句话说清这篇要讲什么。这一段在版式上是灰字，像是给人的一句提示。把下面这段 <code>lead</code> 的写法记住就行。</span>

<span class="todo">【正文】这一页是模板。复制整个 example 目录、改目录名，就是一个新文章页。下面把常用元素都摆了一遍，留着需要的、删掉不需要的。</span>

## 小标题

正文段落。<span class="todo">【注意】站内标题用二级（两个 #），不要跳级；三级标题在这套版式里是小字号居中，当作更细的分隔用。</span>

行内代码写法：`OSDI-12`，链接写法：[世界卫生组织](https://www.who.int/)。

## 列表

- 无序列表第一条
- 第二条，可以内嵌说明

1. 有序列表第一条
2. 第二条

## 引文

> <span class="todo">引文用楷体，左侧一道竖线。适合放原文摘录、标准条文、或者别人的一句话。</span>

## 表格

| 项 | 值 |
|:---|:---|
| 示例 | 待确认 |
| 示例 | 待确认 |

## 代码块

```r
# 统计分析的代码可以直接贴，等宽字体、带边框和淡阴影
library(survival)
fit <- coxph(Surv(time, status) ~ exposure + age + sex, data = dat)
summary(fit)
```

<span class="todo">【结尾】不必强行收束，讲完了就停。</span>
