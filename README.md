# 个人主页（yihui.org 版式复刻 · Hugo 版）

Hugo 站点：`content/` 里写 Markdown，推送后 GitHub Actions 自动编译成网页。
**你只需要写 md，不用碰 HTML。** 本地已有 Hugo 0.152.2，敲一句就能实时预览。

视觉语言复刻自 <https://yihui.org>（Hugo 的 hugo-xmin 主题）：
Palatino 衬线正文、992px 版心、四周一圈印刷裁切线、虚线链接、居中页脚、深色模式反相。

---

## 一、目录结构

```
Website/
├── hugo.yaml                     站点配置（标题、作者、语法高亮开关）
├── content/                       ← 你以后只改这里
│   ├── _index.md                 首页正文
│   ├── about/index.md            关于页
│   └── blog/
│       ├── _index.md             日志列表页的标题与说明
│       └── example/index.md      文章模板（复制这个目录 = 新文章）
├── layouts/                      版式模板，不用动
│   ├── _default/baseof.html      页头 + 导航 + 页脚的骨架
│   ├── _default/home.html        首页
│   ├── _default/list.html        列表页（按年份自动归档，加文章不用改它）
│   ├── _default/single.html      文章页
│   ├── partials/nav.html         导航菜单
│   ├── partials/footer.html      页脚
│   └── 404.html                  找不到页面时的兜底页
├── static/                       原样搬到网站根目录的文件
│   ├── css/
│   │   ├── style.css             主版式（版心、导航、裁切线、表格、代码、深色模式）
│   │   ├── fonts.css             字体栈（只用系统字体，不加载网络字体）
│   │   └── custom.css            个性化覆盖，想调外观优先改这里
│   └── images/                   放图片（现在为空）
├── archetypes/default.md         新文章的默认抬头
├── .github/workflows/hugo.yaml   推送后自动编译部署
└── public/                       编译产物，已忽略，不进仓库
```

---

## 二、本地预览

在 `E:\Website` 下开一个终端：

```bash
hugo server
```

浏览器打开 <http://localhost:1313>。改任何文件保存即刷新，不用重启。

只想编译不预览：

```bash
hugo --gc
```

产物在 `public/`，可以直接双击 `public/index.html` 看（此时链接是 `/blog/` 这种绝对路径，双击打不开子页面，正常，用 `hugo server` 就对）。

---

## 三、发一篇新文章

### 1. 建文件

```bash
hugo new content blog/2026-sde-prevalence/index.md
```

**目录名就是网址**：上面这条会生成 `https://你的域名/blog/2026-sde-prevalence/`。
所以用英文短名或拼音，别用中文、别用空格。

手写也行：复制 `content/blog/example/` 整个目录，改目录名，再改里面的 `index.md`。

### 2. 改抬头（front matter）

```yaml
---
title: "天津市中学生症状性干眼与近视共患分析"
date: 2026-11-05
author: "张献伟"
tags: ["干眼", "近视"]
draft: false
---
```

- `title` 是页面上显示的大标题，随便写中文
- `date` 决定它在列表里排哪一年、哪一天。**日期写未来，Hugo 默认不生成**，要发就写今天或过去（想发预告文见第四节）
- `draft: true` = 只在本地 `hugo server -D` 能看到，线上不出现。想「先写好、以后再发」就用它
- `tags` 只作记录，不会生成标签页（配置里关掉了）

### 3. 写正文

就是普通 Markdown。几个约定：

| 写法 | 效果 |
|---|---|
| `## 小标题` | 居中、底下一条细线。**站内只用二级**，别跳到 `###` 以外 |
| `### 小小标题` | 小型大写字距居中，当更细的分隔用 |
| `> 引文` | 楷体 + 左侧竖线 |
| 一段包在 `<span class="lead">…</span>` 里 | 灰字导语 |
| 一行包在 `<span class="todo">…</span>` 里 | 浅黄底，标记「还没填」 |
| `![说明](/images/x.jpg)` | 图片放 `static/images/`，路径写 `/images/x.jpg` |

代码块、表格、列表都在 `content/blog/example/index.md` 里摆了样例，照着抄。

### 4. 推送

```bash
git add -A
git commit -m "新增：干眼与近视共患分析"
git push
```

或者在 GitHub Desktop 里写一句 Summary → **Commit to main** → **Push origin**。
推完打开仓库的 **Actions** 页能看到「部署站点」在跑，绿勾之后一两分钟线上就是新版。

---

## 四、发布（已配置完成）

仓库：<https://github.com/jashiwe/jashiwe.github.io>（仓库名与用户名同名，所以站点在**根路径**）
站点：**<https://jashiwe.github.io/>**

Pages 的 Source 已设为 `GitHub Actions`，之后**每次 push 自动编译发布**，不用再做任何设置。

### 以后更新

```bash
git add -A
git commit -m "新增：某篇文章"
git push
```

或在 GitHub Desktop 里写 Summary → **Commit to main** → **Push origin**。
推完看 <https://github.com/jashiwe/jashiwe.github.io/actions>：「部署站点」绿勾后 1–2 分钟线上生效。

### 万一 Actions 红了

2026-10-30 首次配置时踩过一次：报 `Resource not accessible by integration`，原因是仓库当时还没启用 Pages，工作流无权自己创建站点。**现在已启用，不会再出现**。真遇到了，就去 Settings → Pages 确认 Source 还是 `GitHub Actions`，然后点最近一次失败的重跑 **Re-run all jobs**。

其他常见红叉：`content/` 里某篇 front matter 的 YAML 写错（缩进、引号），或 `date` 用了未来时间。报错行号会直接指向那个 md 文件。

### 想改回子路径（不推荐）

只有当仓库改名成别的名字时，站点才会落到 `/仓库名/` 下。站内链接由 Hugo 的 `relURL` 生成，会自动带上前缀；此时把 `hugo.yaml` 的 `baseURL` 一并改成新地址即可（workflow 里本来就会自动覆盖，改它只为本地编译的绝对链接正确）。

---

## 五、填内容：找浅黄色的地方

所有还没填的位置都套了 `class="todo"`，页面上是**浅黄底**，一眼能找出来。填完两种处理：

- **省事**：不管它，黄色底不影响观感
- **干净**：把 `static/css/custom.css` 里的 `.todo { background: lightyellow; }` 删掉，黄色全部消失（这条规则**只有一处**，删了就真没了）

### 还缺的信息（页面上是浅黄底）

- [x] 首页三段自我介绍（`content/_index.md`）——已按简历写好
- [x] 「关于」页研究方向、学术背景、论文 19 篇、课题、成果、软著——已按简历填好
- [ ] **参编论著的书名 / 出版社 / 年份**（`content/about/index.md`「其他成果」最后一条）
- [ ] **联系邮箱**（同上「联系」一节）
- [ ] **单位地址**（写到科室一级即可，不用门牌号）
- [ ] 首页那句引文是我替你起草的，换成你自己的话或整段删掉
- [ ] 页脚版权年份是动态的（`now.Year`），不用管

> 涉及单位、职务、成果数量这类可能被外人当真的信息，宁可空着也别凑。这站是给人看的门面，不是台账。

---

## 六、几个容易踩的坑

**1. 目录名 = 网址，改名 = 换链接**
已经有外部链接的文章，别改目录名；要改就加一行 `aliases: ["/blog/旧名字/"]` 做跳转。

**2. 中文目录名会让网址变成一串 `%E4%B8%AD%E6%96%87`**
Hugo 不会自动转拼音。目录名用英文；标题写中文没问题。

**3. 图片放 `static/images/`，引用写 `/images/xxx.jpg`**
放 `content/` 里也行，但那样路径跟着文章走，以后搬文章会断。统一放 `static/images/` 最稳。

**4. 日期写成未来，文章不生成**
Hugo 默认只发布 `date` ≤ 当前时间的页面。想提前写好、定点发，用 `draft: true` 然后到日子改日期推送。

**5. 文件名和 URL 大小写**
GitHub Actions 跑在 Linux 上，区分大小写；本地 Windows 不区分。本地能开、线上 404，九成是大小写对不上。

**6. 线上样式没更新**
浏览器缓存。Ctrl+F5 强刷；还不对就去 Actions 看最近一次是不是红的。

**7. Hugo 版本漂移**
`hugo.yaml` 不用管版本，`.github/workflows/hugo.yaml` 里锁了 `0.152.2`，和你本机一致。以后升级 Hugo，两处一起改，否则本地预览正常、线上报错。

---

## 七、和 yihui.org 的差异（我主动改的）

复刻的是版式和气质，不是逐字照搬：

| 位置 | yihui.org | 这里 | 原因 |
|---|---|---|---|
| 中文正文字体 | 思源宋体（CDN 子集字体） | 系统宋体回落链 | 不加载外部字体，国内打开零延迟。想还原原味就装「思源宋体」，装完自动生效。`fonts.css` 末尾也备了网页字体方案，取消注释即可。 |
| 代码字体 | 楷体 | 等宽字体 | 楷体写代码可读性太差，这条不跟。 |
| 代码高亮 | 有 | 关掉（`hugo.yaml` 里 `codeFences: false`） | Chroma 会塞一堆行内色值，盖掉这套「细边框 + 淡阴影」的样式。想要彩色语法高亮，把那一行删掉。 |
| 引文竖线 | 鼠标移上去才出现 | 默认就显示 | 骨架期得让人看见版式。想恢复原样，在 `custom.css` 里加：`.cn blockquote:not(:hover) { border-left-color: transparent; }` |
| 图片深色模式 | 整站反相 | 整站反相 + 图片二次反相 | 原站照片会变负片，这里还原成正常颜色。 |
| 大标题花饰 | `➶` | `➶`（同原站） | 主题原版字符是 `❧`，但那个字形会压在中文标题上。想换：改 `style.css` 里 `h1.title::before` 的 `content`。 |
| 站点语言 | 中英双语（`/cn/`、`/en/`） | 只有中文 | 现在用不上双语，多一套目录维护成本翻倍。以后要加，在 `hugo.yaml` 里加 `languages` 段。 |
| 后台 | blogdown + R | 纯 Hugo | 不装 R 也能写，Markdown 直接推。 |

没搬过来的部分：侧边注（sidenotes）、歌词排版、打赏区、RSS 订阅、目录（TOC）自动生成——属于内容形态而非版式，需要时再说。

---

## 八、样式速查

想自己微调，改 `static/css/style.css` 顶部这几个变量最省事：

```css
:root {
  --body-width: 992px;   /* 版心总宽 */
  --text-width: 620px;   /* 文章正文宽 */
  --border-color: #ddd;  /* 分割线、引文竖线、裁切线颜色 */
}
```

`custom.css` 里还留了一个 `.dropcap` 类——给任意段落加上，就是 yihui 首页那种首字下沉。用法：`<p class="dropcap">…</p>`。
