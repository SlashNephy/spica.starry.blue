---
title: "TVTest の Discord Rich Presence プラグインを fork した"
date: 2021-05-30T18:34:28+09:00
draft: false
toc: false
images:
tags:
  - TVTest
  - Discord
  - C++
---

Fork した TvTestRPC は [こちら](https://github.com/SlashNephy/TvTestRPC) です。

---

[TvTestRPC](https://github.com/noriokun4649/TvTestRPC) というプラグインが既にもう開発されていたのですが, 番組が切り替わったタイミングで Presence が更新されなくなるバグに気付きました。  
原因としては TVTest が番組切り替わりのイベントを提供しておらず, 更新が走らないことが要因でした。タイマー処理を回すことで対処しました。

それと放送局ロゴを追加したかったので, Fork して独立することにしました。というのも Discord Rich Presence はアプリケーション ID ごとに予め使用するロゴ画像をアップロードする必要があるからです。  
この fork では新しいアプリケーション ID に切り替えています。

![Discord Developer Portal](/img/tvtestrpc-assets.png)

将来的にはもっといろいろな局に対応したいのですが, 地方局を調べるのは大変なので [Issue](https://github.com/SlashNephy/TvTestRPC/issues/new/choose) からロゴ追加リクエストを随時受け付けています。お気軽にどうぞ (というか報告おねがいします！)

全国共通の NHK 総合と教育, BS はすべてロゴを追加済です。関東広域や一部の地方局の地上波も既に対応済です。

---

まとめると, 機能は本家さんバージョンのものを引き継いでいますが, 以下の相違点があります。

- TvtPlay プラグインと連携し, ファイル再生時にも経過時間を表示できるようになっています。
  - シーク位置を加味して経過時間を計算しています。
- 東京近辺の地上波だけでなく, NHK や BS, 様々な地域の地上波のロゴ表示にも対応しています。
- 視聴中の番組が終了したときに Presence が更新されないバグを修正しました。
- 全角文字を半角に変換するオプションを追加しています。
- サブチャンネル (TOKYO MX2 等) でも Presence を表示できます。

<div align="center">
  <img src="/img/tvtestrpc-large-image.png" title="Large image text">
  <br>
  <img src="/img/tvtestrpc-small-image.png" title="Small image text">
</div>

Details = `サービス名`  
State = `番組名`  
Large Image Text = `番組説明` (空欄のときは表示されません)  
Small Image Text = `TVTest と TvTestRPC プラグインのバージョン`  

---

ところで C++ は普段あまり書かないので, 文字列操作にかなり詰まりました。  
TVTest の API が提供する文字列は `wchar_t*` ですが, Discord RPC が受け付ける文字列は `const char*` なので相互変換とかフォーマットがしんどかったです。ワイド文字列, なにそれおいしいの？
