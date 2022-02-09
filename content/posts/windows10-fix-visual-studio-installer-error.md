---
title: Windows 10 で Visual Studio インストーラーが「セットアップ中に問題が発生しました」で終了する問題
date: 2016-10-26T15:24:09+09:00
draft: false
toc: false
images:
tags:
  - windows10
  - visualstudio
---

イベント ビューアーで詳細なログを参照したところ

> 'System.Windows.Media.FontFamily' のタイプ初期化子が例外をスローしました

とありました。

最近インストールしたフォントに問題があると考え `Avenir Next.ttc` を削除したところ、問題が解決しました。
