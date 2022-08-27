---
title: "Cpp簡易api教學"
date: 2022-08-27T16:47:28+08:00
tags: [""]
draft: false
description: "奇怪的知識又增加了"
cover:
  image: "https://blog.eduonix.com/wp-content/uploads/2015/12/C-7-Decision-Making-740X296.png" # image path/url
  alt: "cpp" # alt text
  caption: "cpp" # display caption under cover
  relative: false # when using page bundles set this to true
---

---

# 前言
近期在公司接觸到其他團隊的C++專案，遇到與其他語言(GO)/系統串接的情況。
有人提議將使用 stdin 讓兩個語言做溝通(實際效果也不錯，下次再分享)，
但習慣使用webapi的我，突然發覺自己不曾使用過cpp去架設過api...
於是就有了這篇
週末閒來無事，google了一下，找到 github 上擁有 7.6k stars 的 `cpp-httplib`。
引用時也很方便，直接將 `httplib.h` 拉進自己的專案內就可以使用，真舒爽!


# Library
## httplib  [🔗](https://github.com/yhirose/cpp-httplib)
> A C++11 single-file header-only cross platform HTTP/HTTPS library.
> It's extremely easy to setup. Just include the httplib.h file in your code!
> 
> NOTE: This library uses 'blocking' socket I/O. If you are looking for a library with 'non-blocking' socket I/O, this is not the one that you want.

開頭就提及，此 library 是 blocking socket I/O，對於 I/O 密集的 api 可能就不太適合了。

### 簡易範例
> NOTE: 記得建一個資料夾 `lib`，將 [httplib.h](https://raw.githubusercontent.com/yhirose/cpp-httplib/master/httplib.h) 放進去，這樣就可以使用了，真是方便呢!!

```c++
// main.cpp
#include <stdio.h>
#include <iostream>
#include "lib/httplib.h"
using namespace httplib;

void hello_world(const Request& req,Response& resp) {
  printf("%s [%s] %s, %s, %s %i \n", 
            ctime(&my_time),
            req.method.c_str(),
            req.path.c_str(),
            req.remote_addr.c_str(),
            req.version.c_str(),
            req.remote_port);
  resp.set_content("<html><h1>Hello world.</h1></html>","text/html");
}

int main() {
  Server svr;
  svr.Get("/api/hello", hello_world);
  svr.listen("0.0.0.0", 8000);
  system("pause");
  return 0;
}
```

輸入 `g++ -g main.cpp -o svr -lpthread && ./svr` 編譯並執行 

(如果沒安裝 g++)
```bash
sudo apt update
sudo apt install g++
```

安裝完成測試一下

```bash
g++ --version

>>> g++ (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

最後使用瀏覽器開啟 [localhost:8000/api/hello](http://localhost:8000/api/hello)
即可看到 Hello world.

---

## nlohmann/json  [🔗](https://github.com/nlohmann/json)
> based on c++11

### 使用範例
NOTE: 將 [json.hpp](https://github.com/nlohmann/json/releases/download/v3.11.2/json.hpp) 放進 `./lib` 當中，即可使用，也是很方便的~

```cpp
#include <lib/json.hpp>
using json = nlohmann::json;

void json_callback(const Request& req, Response& resp) {
  json result{
        {"msg", "ok"},
        {"zh-tw", "你好"},
        {"data", 123}};
  resp.set_content(result.dump(), "text/plain");
}

int main() {
  Server svr;
  svr.Get("/api/json", json_callback);
  svr.listen("0.0.0.0", 8000);
  system("pause");
  return 0;
}
```

最後使用瀏覽器開啟 [localhost:8000/api/json](http://localhost:8000/api/json)
即可看到 Hello world.


以上就是這次的簡易教學，相關code有放置在 [github-demo](https://github.com/f416720001/cpp-webapi)，可以git clone 下來把玩。