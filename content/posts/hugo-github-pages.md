---
title: "使用 Hugo 创建个人网站并部署到 GitHub Pages"
date: "2026-04-02"
description: "手把手教你用 Hugo 快速搭建个人博客网站，并自动化部署到 GitHub Pages"
tags: ["Hugo", "GitHub", "教程"]
categories: ["技术"]
---

## 前言

最近用 Hugo 搭建了这个博客，顺便记录一下完整的搭建过程，方便以后重装系统时参考。

## 什么是 Hugo

[Hugo](https://gohugo.io/) 是一个用 Go 语言编写的静态网站生成器，速度极快，主题丰富，非常适合搭建个人博客、项目文档等。

## 安装 Hugo

### macOS

```bash
brew install hugo
```

### 验证安装

```bash
hugo version
```

## 创建新站点

```bash
hugo new site my-blog
cd my-blog
```

## 选择主题

Hugo 有大量免费主题可选，这里推荐几个：

- [Paper](https://github.com/nanxiaobei/hugo-paper) - 简洁纸张风格
- [LoveIt](https://github.com/dillonzq/LoveIt) - 现代感强

以 Paper 为例：

```bash
git submodule add https://github.com/nanxiaobei/hugo-paper.git themes/paper
echo 'theme = "hugo-paper"' >> hugo.toml
```

## 创建第一篇文章

```bash
hugo new posts/hello-world.md
```

编辑文章内容后，运行：

```bash
hugo server
```

访问 `http://localhost:1313` 预览。

## 部署到 GitHub Pages

### 创建 GitHub 仓库

在 GitHub 上创建一个新仓库，例如 `my-blog`。

### 推送代码

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/my-blog.git
git push -u origin main
```

### 配置 GitHub Actions

在 `.github/workflows/deploy.yml` 中添加：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3

      - name: Build
        run: hugo --gc --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/deploy-pages@v4
```

### 启用 Pages

1. 进入仓库 **Settings**
2. 左侧菜单选择 **Pages**
3. **Source** 选择 **GitHub Actions**

提交代码后，网站就会自动部署了。

## 总结

Hugo + GitHub Pages 是一个零成本的博客解决方案：
- Hugo 本身免费
- GitHub Pages 免费托管
- GitHub Actions 免费自动化部署

唯一需要付出的就是一点学习成本和折腾的时间。

好了，就这么多。
