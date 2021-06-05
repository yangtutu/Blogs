---
title: 我的博客是怎么来的？
date: 2021-06-05
description: 使用 Hexo 和 Next 主题创建个人博客
---

## 前提条件

已安装了 nodejs 和 npm

## 安装 Hexo

``` bash
npm install hexo-cli -g
``` 

## 创建博客

- 初始化

``` bash
hexo init blog-holder
cd blog-holder
npm install
``` 

- 在 `source` 和 `themes` 目录底下添加 `.gitkeep` 文件，保证这两个目录会提交到 Git 仓库


## 创建 Git 仓库单独保存博客文章 (Markdown) [可选]

创建一个独立的项目单独保存博客文章，也可以直接保存到 `source/_posts` 目录下

把 Git 仓库的文章克隆到 `source/_posts`，在 `blog-holder` 目录下执行：

``` bash
git clone {你的 git 地址} source/_posts
``` 

## 引入 Next 主题

在 `blog-holder` 目录下执行：

``` bash
git clone https://github.com/theme-next/hexo-theme-next themes/next
``` 

## 查看效果

执行：

``` bash
hexo server
``` 

打开 http://localhost:4000/ 即可查看效果
