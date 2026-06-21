# Quarto Book — 从零到上线：完整操作指南

> 目标成果：像 https://gz.xueheng.site/lian26june/ 一样的在线书籍网页

---

## 你需要准备的材料

| 材料 | 用途 | 怎么获取 |
|------|------|----------|
| **Quarto CLI** | 核心渲染引擎（Markdown → HTML 网页） | https://quarto.org/docs/download/ |
| **VS Code**（推荐） | 写 `.qmd` 文件的编辑器 | 已装 |
| **Git** | 版本控制 | 已装 |
| **GitHub 账号** | 托管代码 + 免费部署网页 | https://github.com |
| **你的 Markdown 内容** | 你要写什么？课程讲义？项目文档？研究笔记？ | 你自己准备 |
| **模板仓库**（可选） | 快速起步，不用从零搭 | https://github.com/Xueheng-Li/quarto-book-template |

---

## 第一步：安装 Quarto CLI

```bash
# 方式一：下载安装包（推荐 Windows 用户）
# 打开 https://quarto.org/docs/download/ → 下载 Windows .msi → 双击安装

# 方式二：用 conda（本机已有 miniconda）
conda install -c conda-forge quarto

# 验证安装
quarto --version
```

---

## 第二步：创建一个 Book 项目

```bash
# 在你想要的位置执行（建议在 A3/quarto-book/ 下）
cd "d:/AI-gent/学员素材包A1-A4全/学员素材包/A3/quarto-book"

# 用 Quarto 命令创建 book 项目
quarto create-project --type book my-book
```

这会在 `quarto-book/my-book/` 下生成标准结构。

---

## 第三步：理解文件结构

```
my-book/
├── _quarto.yml          ← 🔑 书的「身份证」+「目录」
├── index.qmd            ← 封面/前言
├── chapters/            ← 正文各章（一个文件 = 一章）
│   ├── intro.qmd
│   └── summary.qmd
├── references.bib       ← 参考文献（可选，BibTeX 格式）
└── styles.css           ← 自定义样式（可选）
```

---

## 第四步：配置 `_quarto.yml`（关键！）

```yaml
project:
  type: book
  output-dir: docs          # ⚠️ 设为 docs，GitHub Pages 才能识别

book:
  title: "你的书名"
  author: "你的名字"
  date: "2026-06-21"
  # 章节列表 — 按你想要的顺序排列
  chapters:
    - index.qmd
    - chapters/01-引言.qmd
    - chapters/02-第一部分.qmd
    - chapters/03-让知识活起来.qmd
    - chapters/04-总结.qmd

format:
  html:
    theme: cosmo            # 可选: cosmo, flatly, darkly, litera 等
    toc: true               # 右侧显示本节目录
    search: true            # 全文搜索
    code-fold: true         # 代码块可折叠
```

---

## 第五步：写章节内容（.qmd 文件）

`.qmd` = Quarto Markdown，100% 兼容标准 Markdown，额外支持：

```markdown
# 第三章 让知识活起来

## 3.1 知识管理的新范式

正文内容...

### 公式

$$
y = \beta_0 + \beta_1 x + \epsilon
$$

### 代码块

```python
import pandas as pd
print("hello")
```

### 提示框（Callout Block）

::: {.callout-tip}
## 核心要点
这是需要特别注意的内容。
:::

### 交叉引用

见图 @fig-demo 和表 @tbl-data。

### 脚注

这是一个重要的观点^[支持这个观点的参考文献]。
```

---

## 第六步：本地预览

```bash
cd my-book
quarto preview
```

浏览器自动打开 `http://localhost:4173`，**边写边看效果**。

---

## 第七步：构建（生成最终 HTML）

```bash
quarto render
```

渲染结果在 `docs/` 目录下（就是你要部署到网上的静态文件）。

---

## 第八步：部署到网页（免费）

### 方案 A：GitHub Pages（推荐，你看到的 xueheng.site 就是这个）

```bash
# 1. 在 GitHub 上新建一个仓库（例如 my-book）

# 2. 关联并推送
cd my-book
git init
git add -A .
git commit -m "初始化 Quarto Book"
git branch -M main
git remote add origin https://github.com/你的用户名/my-book.git
git push -u origin main

# 3. 去 GitHub 仓库 → Settings → Pages
#    Source: Deploy from a branch
#    Branch: main, 文件夹: /docs
#    点 Save

# 4. 等待 1-2 分钟，访问：
#    https://你的用户名.github.io/my-book/
```

### 方案 B：自动化部署（每次 push 自动构建）

创建 `.github/workflows/publish.yml`：

```yaml
on:
  push:
    branches: [main]
name: Render and Publish
permissions:
  contents: write
  pages: write
jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
      - uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 方案 C：用自己的域名

在 `docs/` 目录下创建 `CNAME` 文件，里面写你的域名：

```
gz.yourname.site
```

然后在你的域名 DNS 里添加一条 CNAME 记录指向 `你的用户名.github.io`。

---

## 完整流程速查

```
安装 Quarto → 创建项目 → 配置 _quarto.yml → 写 .qmd 章节
    → quarto preview（本地看效果）→ quarto render（构建）
    → git push → GitHub Settings → Pages 开启 → 上线！
```

---

## 参考资源

| 资源 | 链接 |
|------|------|
| 模板仓库 | https://github.com/Xueheng-Li/quarto-book-template |
| 成品示例 | https://gz.xueheng.site/lian26june/ |
| Quarto 官方书文档 | https://quarto.org/docs/books/ |
| Quarto 中文文档 | https://aidoczh.com/quarto/docs/books/book-structure.html |
| 连享会中文教程 | https://lianxhcn.github.io/quarto_book/ |
| GitHub Pages 部署 | https://quarto.org/docs/publishing/github-pages.html |

---

## 你现在可以做的事

- [ ] 安装 Quarto CLI
- [ ] `quarto create-project --type book` 创建项目
- [ ] 规划你的章节结构（准备写什么内容？）
- [ ] 开始写第一章 .qmd
- [ ] `quarto preview` 看效果
- [ ] 推送到 GitHub 上线
