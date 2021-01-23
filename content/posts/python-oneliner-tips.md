---
title: "Python ワンライナーまとめ"
date: 2017-11-18T22:42:19+09:00
draft: false
toc: false
images:
tags:
  - python
---

* 内包表記内の定数宣言に `for` ループを使用

```python
for i in range(10):
    x = "variable"
    do(i, x)
```

から

```python
[do(i, x) for i in range(10) for x in ["variable"]]
```

* `import` 文は `__import__(package_name)` で代用

```python
import sys
```

から

```python
sys = __import__("sys")
```

* 積極的な内包表記の活用
```python
for i in range(10):
    for j in range(10):
        for k in range(10):
            vector(i, j, k)
```

から

```python
[vector(i, j, k) for i in range(10) for j in range(10) for k in range(10)]
```


以上をまとめて

http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0008&lang=jp の例

```python
[print(sum([int(a + b + c + d == int(x)) for a in range(10) for b in range(10) for c in range(10) for d in range(10)])) for sys in [__import__("sys")] for x in sys.stdin]
```
