---
title: Windows 10 の "PC" のライブラリを隠す
date: 2020-06-12T20:46:33+09:00
draft: false
toc: false
images:
tags:
  - windows10
---

Windows 10 から "PC" にライブラリが表示されるようになりました。

![Windows 10 の PC](/img/windows10-pc.png)

ここで表示されるライブラリが邪魔だという方も少なくないと思います。

これらのフォルダはレジストリを編集することで隠すことが可能です。以下、レジストリを操作するため自己責任でおねがいします。

`Win + R` で `regedit` を起動し、次のキーに移動します。
```
\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace
```

`NameSpace` 以下にはライブラリの表示をコントロールするキーが含まれています。これらのキーをリネーム (または削除) することでフォルダを非表示にできます。

レジストリキーとフォルダの対応は以下の表の通りです。

|フォルダ名 |  Guid  |
|-----------|-----------------------|
|3D オブジェクト|{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}|
|ダウンロード|{088e3905-0323-4b02-9826-5d99428e115f}|
|デスクトップ|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|ドキュメント|{d3162b92-9365-467a-956b-92703aca08af}|
|ピクチャ|{24ad3ad4-a569-4530-98e1-ab02f9417aa8}|
|ビデオ|{f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}|
|ミュージック|{3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}|

![Windows 10 の PC](/img/windows10-pc-clean.png)

すっきりしました。(それにしてもドライブが多すぎる...)

どうやら、これらのキーは大型 Update で初期化されるようです。May 2020 Update (version 2004) を適用したところ、表示状態に戻ってしまいました。
