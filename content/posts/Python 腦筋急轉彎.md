---
title: "Python 腦筋急轉彎"
date: 2022-08-15T10:24:28+08:00
tags: ["python", "trick"]
draft: false
description: "奇怪的知識又增加了"
cover:
    image: "https://i.redd.it/rq9dw6jzy9d31.png" # image path/url
    alt: "python-challenge" # alt text
    caption: "Python challenge" # display caption under cover
    relative: false # when using page bundles set this to true
---

# 題目: 讓以下 function 回傳 True

> 原文: https://www.reddit.com/r/Python/comments/cje5yh/short_python_challenge_make_this_return_true/

```python3
def check(x):
    if x+1 is 1+x:
        return False
    if x+2 is not 2+x:
        return False
    return True
```

---

## 解答一: -7

在 Python 中，整数 -5 到 256 會預先分配到記憶體，

此時 -7 + 1 得到的 -6，恰好在這範圍之外，

才會出現 -6 is not -6 的情況

## 解答二: 自定義 class

透過實現 **add**，讓他可以與 integer 互動，
但其實就是拿來判斷 integer 後，回傳適當的 boolean (有點作弊 XDD)

```python3
class Test(int):
    def __add__(self, v):
        if v == 1:
            return 0
        else:
            return v
```
