---
title: 把玩python
date: 2020-07-18 21:20:47
tags:
---

# 套件
---
## [pytube3](https://github.com/get-pytube/pytube3)
### 下載youtube影片模組
#### 2020-07-18
``` bash
// 注意別用以下這個安裝，會報錯誤
// 錯誤 ImportError: cannot import name 'quote' from 'pytube.compat'
pip install pytube
// 請用
// pip install pytube3
// 測試環境python3.7.6 pytube3 9.6.4
```
### 此刻遇到問題
> KeyError: 'url' and KeyError: 'cipher' ,please someone update this code or some procedure to fix my code
>> 解決方法
>> https://github.com/nficano/pytube/issues/661#issuecomment-650766403

### 接著安裝ffmpeg
> https://www.wikihow.com/Install-FFmpeg-on-Windows
> pip install ffmpeg-python

### 此刻遇到問題
> Python FileNotFoundError: [WinError 2] 系統找不到指定的文件。
>>在lib文件夾中找到subprocess.py
>> 1、搜索class Popen(object):
>> 2、將__init__中的shell=False修改為shell=True
>> 再運行問題解決。
>> https://blog.csdn.net/u014094184/article/details/80085336

### 合併 video.mp4 and audio.webm
https://stackoverflow.com/questions/61777314/how-to-use-ffmpeg-to-merge-webm-audio-opus-and-mp4-video-h-264-into-one-mp4
https://blog.shikoan.com/pytube-ffmepg-concat-video-audio/
https://www.coderbridge.com/@Eterna-E/0baeb8bf25e543ed8462bd742cd1946f

### youtube-dl-gui 最後還是這個最好用
> 下載連結 https://mrs0m30n3.github.io/youtube-dl-gui/
> 或是 pip install youtube_dl
> command 範例 (1080P+最高音質)
> youtube-dl -f 137+251 https://www.youtube.com/watch?v=RDINtdz4OYQ

## pandas
### 中文繪圖
```python
from pylab import mpl
mpl.rcParams['font.sans-serif'] = ['Microsoft YaHei']  
mpl.rcParams['axes.unicode_minus'] = False
```
> Pandas有以下類型的圖可以繪製
> 折線圖df.plot()
> 柱狀圖df.plot(kind='bar')
> 橫向柱狀圖df.plot(kind='barh')
> 直方圖df.plot(kind='hist')
> KDE圖df.plot(kind='kde')
> 面積圖df.plot(kind='area')
> 圓餅圖df.plot(kind='pie')
> 散佈圖df.plot(kind='scatter')
> 六角形箱體圖df.plot(kind='hexbin')
> 箱形圖df.plot(kind='box')

### \ufeff ?????
> 這個是檔案編碼時，有時會出現的隱藏字元
> 網路上大家都說 utf-8 改成 utf-8-sig 就好
> 但我還是遇到這問題，於是新增一個 if 去排除掉這個問題@@

## csv 寫入
```python
import csv

output = open(filepath, 'a', newline='', encoding='utf-8-sig')
writer = csv.writer(output)

writer.writerow(['date','num','sp','dex'])
writer.writerow([f_date, d['num'], d['sp'], d['dex']])
```

## 正規化
### 去除html標籤
```python
import re
reg = re.compile(r'<[^>]+>',re.S)

html = 'hi<br>'
nohtml = reg.sub('',html)
print(nohtml)
```