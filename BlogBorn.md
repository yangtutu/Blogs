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


## 发布到阿里云 OSS

- 在 OSS 中使用自定义域名设置静态网站托管

  https://help.aliyun.com/document_detail/67323.html?spm=a2c4g.11186623.6.730.50c023eb1ZuMnl

- 下载和安装命令行工具 ossutil

  https://help.aliyun.com/document_detail/120075.htm?spm=a2c4g.11186623.2.4.cece7a74wf2KXC#title-mjl-h5v-cvl



- 准备初始化脚本 `init.sh`， 存放在 `blog-holder` 目录下

``` bash
#!/bin/bash

npm install

git clone {你的 git 地址} source/_posts

git clone https://github.com/theme-next/hexo-theme-next themes/next
``` 

- 执行初始化脚本 `init.sh`

``` bash
bash init.sh
``` 

- 准备编译脚本 `build.sh`， 存放在 `blog-holder` 目录下

``` bash
#!/bin/bash

cd /root
rm -rf public/

yarn global add hexo-cli

hexo clean
hexo generate
``` 

- 使用 Docker 编译项目

``` bash
docker run --rm -i -v $PWD:/root/ node:16-alpine sh /root/build.sh
``` 

- 准备 OSS 上传脚本 `upload.sh`， 存放在 `blog-holder` 目录下

``` bash
#!/bin/bash

BUCKET_NAME="{Your bucket name}"

ossutil64 rm oss://$BUCKET_NAME --include "*" --recursive --force --all-type
ossutil64 cp ./public oss://$BUCKET_NAME --recursive --force
``` 

- 执行 OSS 上传脚本 `upload.sh`，将博客上传到 OSS

``` bash
bash upload.sh
``` 
