---
title: "Hugo 最新版搭建"
date: 2022-08-14T23:24:28+08:00
tags: ["hugo"]
draft: false
description: "hugo v0.101.0 2022-06-16 最新版"
cover:
  image: "https://thelinuxexperiment.com/blog/wp-content/uploads/2020/03/hugo.png" # image path/url
  alt: "hugo-logo" # alt text
  caption: "Hugo logo" # display caption under cover
  relative: false # when using page bundles set this to true
---

> 前言:
> 2020 年碩士期間搭建了 Hexo 來記錄所學，當時對於 npm 家族比較習慣，近期開始接觸 golang，順勢也把荒廢的 blog 搬移過來，希望接下來能夠整理一些近期 kafka、AWS EMR、BigQuery 所學的知識...

# Hugo 安裝最新版本

> 如果你也像我一樣，apt install hugo 都卡在 0.68.0 左右的版本的話，可以參考
> 這邊是使用 ubuntu 20.04，要記得挑符合自己 cpu 版本的

## 1. 移除 apt 所安裝的 hugo

```bash
sudo apt update
sudo apt install curl wget
sudo apt remove hugo
```

## 2. 下載最新 hugo.deb

```bash
curl -s https://api.github.com/repos/gohugoio/hugo/releases/latest \
 | grep  browser_download_url \
 | grep Linux-64bit.deb \
 | grep -v extended \
 | cut -d '"' -f 4 \
 | wget -i -
```

## 3. 安裝他，改成你下載的 filename

```bash
sudo dpkg -i hugo*_Linux-64bit.deb
```

## 4. 完工

```bash
hugo version
```
