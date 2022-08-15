---
title: "Docker private registry搭建"
date: 2022-08-15T09:24:28+08:00
tags: ["docker", "registry"]
draft: false
description: "簡易搭建及使用"
cover:
  image: "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.ytimg.com%2Fvi%2F-t8pvV8kIVw%2Fmaxresdefault.jpg" # image path/url
  alt: "Docker-registry" # alt text
  caption: "Docker Registry" # display caption under cover
  relative: false # when using page bundles set this to true
---

1. Docker-compose.yml
> 無密碼，無SSL

```yml
 version: '3'

 services:
   registry:
     image: registry:2.8.1
     ports:
       - "21010:5000"
     environment:
       REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
     volumes:
       - ./auth:/auth
       - ./data:/data
```

2. Chagne your images name
```bash
docker tag alpine localhost:21010/my-alpine
```
3. Push it
```bash
docker push localhost:21010/my-alpine
```

after all,
4. Pull your image from private registry
```bash
docker pull localhost:21010/my-alpine
```


Ref. https://gabrieltanner.org/blog/docker-registry/