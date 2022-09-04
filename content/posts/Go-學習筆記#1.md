---
title: "Go 學習筆記#1"
date: 2022-09-04T23:27:44+08:00
tags: ["golang", "gin", "melody"]
draft: false
description: "奇怪的知識又增加了"
cover:
  image: "https://www.dailybtc.cn/wp-content/uploads/2020/01/frc-b7677fa97ea4699b778cab2861df5975.jpg" # image path/url
  alt: "golang" # alt text
  caption: "golang" # display caption under cover
  relative: false # when using page bundles set this to true
---

# Golang 簡易聊天室

[f416720001/simple_chat](https://github.com/f416720001/simple_chat)

這周末學習了與MongoDB，使用了 mgo、mongo-go-driver 這兩個套件去學習，但...上手還需要一陣子，太久沒使用強型別，很多地方都還需要適應。

所以，這次先帶來 [Go into Web!-iThome](https://ithelp.ithome.com.tw/articles/10239615) 上所分享的小專案來進行學習，也能更快去熟悉語法及debug、build、log等開發技巧，話不多說，讓我們開始吧。

## 知識點們

### melody

> https://github.com/olahol/melody
> Minimalist websocket framework for Go

是一個2018年已經棄坑(喂~)的 websocket framework，是 based on gorilla/websocket 上封裝的一個 package，更容易使用，以外如果要找還有在維護、Star還夠多的messaging server 可以參考 [centrifugal/centrifugo](https://github.com/centrifugal/centrifugo)

回到正題，而因為它是 websocket 框架，網站的靜態文件還是得找個人來幫忙掛載，在 melody 的官方教學文件上，就是與 [gin](https://github.com/gin-gonic/gin) 一起配合，就讓我們來看看以下的 code~

```golang
package main

import (
	"github.com/gin-gonic/gin"
	"gopkg.in/olahol/melody.v1"
	"net/http"
)

func main() {
  // 實例 gin、melody
	r := gin.Default()
	m := melody.New()

  // 把index.html掛載到 /
	r.GET("/", func(c *gin.Context) {
		http.ServeFile(c.Writer, c.Request, "index.html")
	})

  // melody's websocket 則是掛載到 /ws
	r.GET("/ws", func(c *gin.Context) {
    // gin的 response 與 request 轉手交給 melody 去 handle
		m.HandleRequest(c.Writer, c.Request)
	})

  // 那麼 melody 透過 handlemessage，當 message 進來時該如何處理，這邊的寫法是，有訊息進來將會告知所有 sessions
	m.HandleMessage(func(s *melody.Session, msg []byte) {
		m.Broadcast(msg)
	})

  // melody handleconnect 使用者連線，這邊我們定義websocket連線網址長得像
  // ws://localhost:5000/ws?id=Guest755
  m.HandleConnect(func(session *melody.Session) {
    // 取得url上的param id
    // := 冒號賦值，不用預先定義直接賦值
		id := session.Request.URL.Query().Get("id")
    // 廣播新的Message，這邊有個 GetByteMessage 會做 JSON encoding
		m.Broadcast(NewMessage("other", id, "加入聊天室").GetByteMessage())
	})

  // melody handleconnect 使用者離開後，傳訊息通知其他使用者
	m.HandleClose(func(session *melody.Session, i int, s string) error {
		id := session.Request.URL.Query().Get("id")
		m.Broadcast(NewMessage("other", id, "離開聊天室").GetByteMessage())
		return nil
	})

  // gin server run on port 5000
	r.Run(":5000")
}
```


### Struct 結構資料

非常像C++的struct，我目前使用起來可以說是一模一樣，可以定義一結構化的資料，使用已經定義好的Struct方式如下:
`Message{Event: "ABC", Name: "ABC", Content: "ABC", TimeStamp: time.Now()}`

看起來有點繁瑣吧，這邊提供一個func NewMessage，將一些param mapping到定義好的struct，可以更方便的使用。
```golang
type Message struct {
	Event   string `json:"event"`
	Name    string `json:"name"`
	Content string `json:"content"`
	TimeStamp    time.Time `json:"timestamp"`
}

func NewMessage(event, name, content string) *Message {
	return &Message{
		Event:   event,
		Name:    name,
		Content: content,
		TimeStamp: time.Now(),
	}
}

func (m *Message) GetByteMessage() []byte {
	result, _ := json.Marshal(m)
	return result
}
```