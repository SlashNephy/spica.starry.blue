---
title: "nginx で localhost を access.log から除外する"
date: 2017-01-31T15:56:09+09:00
draft: false
toc: false
images:
tags:
  - nginx
---

`nginx.conf` などで

```
map $remote_addr $loggable {
    127.0.0.1 0;
    default 1;
}
```

`$loggable` 変数を定義しておきます。`1` がログ対象という対応になります。

アクセスログを取りたいところで

```
access_log /path/to/access.log if=$loggable;
```

のように `if` 引数を付けてあげれば OK。

ドキュメント: [Module ngx_http_log_module](https://nginx.org/en/docs/http/ngx_http_log_module.html)
