---
title: VueJS的那些事
date: 2020-07-03 20:59:49
tags: vue
---
[[toc]]

# Vuex
![vuex](https://miro.medium.com/max/1400/1*KIoNyRO6s_52W68Y-0usJw.png)

## 幾個重點
### component想要做寫入資料，要透過 commit 去 mutations 呼叫函示來寫入
```javascript=
this.$store.commit('SETDAY', val)
```

---
# Router
## Error 訊息
``` bash
vue-router.esm.js?8c4f:2062 Uncaught (in promise) 
Error: Avoided redundant navigation to current location: "/path/1".
```

## 解決之道
### 這裡要特別注意，如果在切換路徑時(帶數字編號那種)，可能會超過，或是數字沒超過但呼叫到router(以下這種)，使得變更路徑重複的問題，建議用if else 把限制條件寫好。
``` javascript
this.$router.replace({
    params: {
        day: day + 1,
    },
});
```

# vue + windows10 ubuntu wsl
``` bash
安裝指令紀錄
## 更新apt-get
$ sudo apt update
$ sudo apt-get update
## 安裝node.js+npm
$ sudo apt-get install nodejs
$ sudo apt-get install npm
$ npm install npm -g
$ npm list -g
## 安裝git
$ sudo apt-get install git
## 安裝vue + vue-cli
$ npm install vue
$ yarn global add @vue/cli-init
## 安裝webpack
$ npm install webpack -g
## 新建專案
$ vue init webpack PROJECT_NAME
## 打開專案資料夾
$ cd PROJECT_NAME
$ npm install
## 執行
$ npm run dev
```

# npm run build 遇到各種難題
``` javascript
## 問題?建立完後，把dist資料夾部屬上去，一片空白
## 解決。vue cli3 專案根目錄新增 vue.config.js 這個檔案，裡面新增下面這一串
module.exports = {
  publicPath: "./"
};

# 所有template css vue 檔案中的路徑、還有import時、還有 axios.get 時請注意，麻煩以 ./ or @/ (==./src)開頭
## 圖片就以 ./img 為開頭，然後圖片丟到 ./public/img 即可

```