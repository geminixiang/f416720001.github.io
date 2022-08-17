---
title: "Docker timezone 時區問題"
date: 2022-08-15T10:24:28+08:00
tags: ["python", "trick", "docker"]
draft: false
description: "奇怪的知識又增加了"
cover:
  image: "/docker.jpg" # image path/url
  alt: "python-challenge" # alt text
  caption: "Python challenge" # display caption under cover
  relative: false # when using page bundles set this to true
---

# Docker Timezone 時區問題

## 方法一： 將本機的時間帶到 image 當中

when `docker run some-image`

```
-v /etc/localtime:/etc/localtime:ro
```

docker-compsoe.yml

```yml
services:
  some-container:
    volumes:
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
```

## 方法二(推薦)： 設定環境變數

when `docker run some-image`

```
-e "TZ=Asia/Taipei"
```

docker-compose.yml

```yml
sevices:
  some-container:
    environment:
      TZ: Asia/Taipei
```

# 例外

如果使用到 alpine 版本

則需要自行重新 build image

Dockerfile 先添加以下兩行，再進行上述操作

```dockerfile
RUN apk update && \
    apk add -U tzdata
```
