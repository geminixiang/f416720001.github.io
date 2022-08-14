---
title: Docker那些事
date: 2020-09-17 19:46:58
tags:
---

# Docker ps
列出所有執行中的container

# Docker run

# Docker attach
重點，使用在已執行的container
ex.
docker attach attach recursing_wiles
docker attach 3c6847fabc75
[注意] 2020.09.17 這裡可能會沒顯示任何東西，只要隨意按一鍵，就會跳出 root@flkwae21k:/host/home ，只要顯示這行就進入bash了

[此外] Ctrl+P + Ctrl+Q 退出
ref. https://philipzheng.gitbook.io/docker_practice/container/enter

# Jupyter Password
1. 手動生成 呼叫python shell (離開用 exit())
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:......'

2. 打開 jupyter_notebook_config.json
vim ~/.jupyter/jupyter_notebook_config.py

3. paste
c.NotebookApp.ip = '*'
c.NotebookApp.password = u'sha1:67c9e60bb8b6:......'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888

# Jupyter Lab
pip3 install jupyterlab
jupyter lab

# Rename
docker rename CONTAINER_NAME NEW_NAME