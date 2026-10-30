---
title: "{{ replace .File.ContentBaseName "-" " " }}"
date: {{ .Date }}
author: "张献伟"
tags: []
draft: false
---

<!-- 标题会取第一行的 title。文件名用英文短名或拼音，别用中文——它会变成网址的一部分。 -->

这里写第一段。想让这段显示成灰字导语，就在整段外面包一层：

<!-- <span class="lead">这一段是导语。</span> -->

## 小标题

正文。
