
---
title: "搭建自己的 Blog"
description: "记录我使用 Astro 和 Cloudflare Pages 搭建个人博客的过程。"
pubDate: "2026-07-06"
tags: ["Astro", "Cloudflare Pages", "Blog"]
draft: false
---
# 搭建自己的 Blog

| 项目             | VitePress + GitHub Pages | Astro + Cloudflare Pages<br />   |
| ---------------- | ------------------------ | -------------------------------- |
| 首次部署         | 很简单                   | 稍微多一步                       |
| 绑定域名         | 简单                     | 更简单，尤其域名在 Cloudflare 时 |
| 自动部署         | GitHub Actions           | Cloudflare Git 集成              |
| SSL 证书         | 自动                     | 自动                             |
| DNS 理解成本     | 中等                     | 较低                             |
| 后期扩展         | 一般                     | 更强                             |
| 适合漂亮个人博客 | 可以，但有限             | 更适合                           |

# Astro + Cloudflare Pages

## 准备环境

你需要先准备：

```plain
Node.js 22.12.0+
Git
GitHub 账号
Cloudflare 账号
一个代码编辑器，比如 VS Code
```

在终端检查：

```plain
node -v
npm -v
git -v
```

## 更新 Ubuntu 软件源

```plain
sudo apt update
sudo apt upgrade -y
```

安装基础工具：

## 配置 Git

先检查 Git：

```plain
git -v
```

配置用户名和邮箱：

```plain
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"
```

查看配置：

```plain
git config --global --list
```

---

## 在 WSL 安装 Node.js

不要优先用 Ubuntu 自带的 `apt install nodejs`，版本可能旧。推荐用 **nvm** 管理 Node。

执行：

```plain
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

然后重新加载环境：

```plain
source ~/.bashrc
```

安装 Node LTS：

```plain
nvm install --lts
nvm use --lts
```

检查：

```plain
node -v
npm -v
```

能看到版本号就可以。

## 项目不要放在 C 盘路径里

不要把博客项目放在：

```plain
/mnt/c/Users/xxx/Desktop
```

虽然可以用，但速度慢，权限和换行符也容易出问题。

推荐放在 WSL 自己的目录：

```plain
mkdir -p ~/projects
cd ~/projects
```

以后你的博客项目就放这里。

## 创建 Astro 博客项目

进入项目目录：

```plain
cd ~/projects
```

创建 Astro 项目：

```plain
npm create astro@latest my-blog
```

进入项目：

```plain
cd my-blog
```

启动本地开发：

```plain
npm run dev
```

终端会显示类似：

```plain
http://localhost:4321
```

你可以直接在 Windows 浏览器打开：

```plain
http://localhost:4321
```

WSL 里的服务，Windows 浏览器一般可以直接访问。

### 报错：sh: 1: astro: not found

说明没有astro依赖：执行一下命令就行

```plain
npm instal
```

## 接 GitHub

### 先去 GitHub 创建仓库

打开 GitHub，右上角点 `+`：

```plain
New repository
```

仓库名填写：

```plain
my-blog
```

注意：

```plain
Owner: zhq-coder
Repository name: my-blog
```

然后下面这些先不要勾选：

```plain
不要勾选 Add a README file
不要勾选 .gitignore
不要勾选 license
```

保持空仓库，然后点击：

```plain
Create repository
```

### 方式一：用 HTTPS，最简单

先在 GitHub 创建一个仓库，比如：

```plain
my-blog
```

然后在 WSL 里：

```plain
cd ~/projects/my-blog

git add .
git commit -m "init astro blog"

git branch -M main
git remote add origin https://github.com/你的GitHub用户名/my-blog.git
git push -u origin main
```

第一次 push 可能会让你登录 GitHub。

### 方式二：用 SSH，推荐长期使用

在 WSL 里生成 SSH Key：

```plain
ssh-keygen -t ed25519 -C "你的GitHub邮箱"
```

一路回车即可。

查看公钥：

```plain
cat ~/.ssh/id_ed25519.pub
```

复制输出内容，然后去 GitHub：

```plain
GitHub
→ Settings
→ SSH and GPG keys
→ New SSH key
```

粘贴进去。

测试：

```plain
ssh -T git@github.com
```

看到类似：

```plain
Hi xxx! You've successfully authenticated
```

就成功了。

然后仓库地址用 SSH：

```plain
cd ~/blog/my-blog

git add .
git commit -m "init astro blog"

git branch -M main

git remote remove origin
git remote add origin git@github.com:你的github账号/my-blog.git

git push -u origin main
```

## 部署到 Cloudflare Pages

进入 Cloudflare 后台：

```plain
compute
Workers & Pages
→ Create
→ Pages
→ Connect to Git
→ 选择 GitHub
→ 选择 my-blog 仓库
```

配置：

```plain
Framework preset: Astro
Build command: npm run build
Build output directory: dist
Production branch: main
```

然后点击：

```plain
Save and Deploy
```

部署成功后会给你一个地址：

```plain
https://你的项目名.pages.dev
```

以后你只需要在 WSL 里写文章，然后：

```plain
git add .
git commit -m "add new post"
git push
```

Cloudflare Pages 会自动更新网站。

# VitePress + GitHub Pages

## 安装Node.js，然后执行：

```plain
mkdir my-tech-blog
cd my-tech-blog

npm init -y
npm install -D vitepress
npx vitepress init
```

## 本地运行

```plain
npm run docs:dev
```

## 修改首页

打开：

```plain
docs/index.md
```

可以写成这样：

```plain
---
layout: home

hero:
  name: "你的博客名称"
  text: "博客的内容"
  tagline: "博客的简介"
  actions:
    - theme: brand
      text: 查看项目
      link: /projects/
    - theme: alt
      text: 论文笔记
      link: /papers/

features:
  - title: Binary Security
    details: 二进制分析、逆向工程、漏洞检测、Fuzzing。
  - title: AI for Security
    details: LLM、深度学习、安全自动化分析。
  - title: Research Notes
    details: 论文阅读、实验复现、研究想法整理。
---
```

## 新建栏目

创建这些文件夹：

```plain
mkdir -p docs/projects
mkdir -p docs/papers
mkdir -p docs/notes
mkdir -p docs/blog
mkdir -p docs/resume
```

创建几个页面：

```plain
touch docs/projects/index.md
touch docs/papers/index.md
touch docs/notes/index.md
touch docs/blog/index.md
touch docs/resume/index.md
```

例如 `docs/projects/index.md`：

```plain
# Projects

## AI-assisted Binary Analysis System

一个面向 ELF/PE 二进制文件的 AI 辅助分析系统，计划支持：

- 函数提取
- 字符串提取
- 危险 API 识别
- 调用图分析
- LLM 函数摘要
- 风险评分
- 漏洞分析报告生成

## Security Situation Awareness System

基于 OpenResty、ModSecurity、FastAPI、LLM 的安全态势感知系统。
```

# 部署到 GitHub Pages

## 1. 创建 GitHub 仓库

如果你的 GitHub 用户名是 `yourname`，可以创建：

```plain
yourname.github.io
```

GitHub Pages 官方 quickstart 也是建议创建 `username.github.io` 这种仓库名来作为用户站点。

---

## 2. 推送代码

```plain
git init
git add .
git commit -m "init blog"

git branch -M main
git remote add origin git@github.com:yourname/yourname.github.io.git
git push -u origin main
```

---

## 3. 添加 GitHub Actions

创建文件：

```plain
.github/workflows/deploy.yml
```

内容如下：

```plain
name: Deploy VitePress site to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci || npm install

      - name: Build
        run: npm run docs:build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/.vitepress/dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

VitePress 官方部署文档也建议在 `.github/workflows` 中创建部署 workflow，并在 GitHub Pages 设置里选择 GitHub Actions 作为部署来源。

---

## 4. 打开 GitHub Pages

进入仓库：

```plain
Settings → Pages → Build and deployment → Source → GitHub Actions
```

等待 Actions 跑完后，你的网站地址一般是：

```plain
https://yourname.github.io
```

GitHub 官方说明，Pages 站点发布后可能需要一小段时间才能访问，文档中提到变更发布可能需要最多约 10 分钟。

---

# 五、使用自己的域名

以后可以买一个域名，比如：

```plain
qemuvi.com
qemuvi.dev
qemuvi.net
qemuvi.xyz
```

然后绑定到 GitHub Pages 或 Cloudflare Pages。

GitHub Pages 支持在仓库设置和 DNS 中配置自定义域名；官方也建议先验证域名，避免子域名接管风险。

如果你用 GitHub Pages，常见配置是：

```plain
www.example.com  CNAME  yourname.github.io
```

如果用 Cloudflare Pages，直接在 Cloudflare 里连接 GitHub 仓库，然后绑定域名会更顺滑。Cloudflare Pages 的 Git 集成支持连接 GitHub/GitLab，push 后自动部署。
