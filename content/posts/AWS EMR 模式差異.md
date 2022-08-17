---
title: "AWS EMR 模式差異"
date: 2022-08-17T15:55:28+08:00
tags: ["aws", "emr"]
draft: false
description: "奇怪的知識又增加了"
cover:
  image: "https://i1.wp.com/big-data-demystified.ninja/wp-content/uploads/2020/07/emr.jpg" # image path/url
  alt: "aws-emr" # alt text
  caption: "AWS EMR" # display caption under cover
  relative: false # when using page bundles set this to true
---

# Mode
> 1. Client mode
> 2. Cluster mode

## 差異點
---
> driver 放置的位置

| |driver放置於 |
| ---|---|
|Client|Master|
|Cluster|Slave(Worker)其中一台|


## 影響層面
---

- 1. Memory 設定
> 啟動Job時，是否要指定memory的使用量

 
||Memory設定|
|---|---|
|Client|都可|
|Cluster|指定比較優，因為driver預設2GB|

- 2. 機器壞掉時影響層面

| | EC2壞掉影響層面 | worker記憶體量 |
| ---|---| --- |
| Client | 千萬不能壞Master，需要額外的重啟流程 | 優 |
| Cluster | 壞帶有driver的worker時，master會協助重啟 | 少|

## 小結論
Client 適合中小型

| |適合情境 |
| ---|---|
|Client| Master/Slave 在同一網路內 |
|Cluster| Master/Slave 在不同網路內 |