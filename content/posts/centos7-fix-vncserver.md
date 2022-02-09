---
title: CentOS 7 で VNCServer が起動しない問題
date: 2016-09-22T16:56:52+09:00
draft: false
toc: false
images:
tags:
  - centos7
  - vncserver
---

VNC に割り当てているディスプレイ番号が `1` なら

```shell
sudo rm /tmp/.X11-unix/X1
```

として、一時ファイルを削除しましょう。
