---
title: "KlabExpertCamp #6 参加記"
description: "KlabExpertCamp #6の感想とか"
slug: 2023KlabExpertCamp
date: 2023-09-10T01:00:00+09:00
draft: false
image: "nameplate.webp"
---

# KlabExpertCamp #6

## KlabExpertCampとは？

![Kawaiiネームプレートを頂いた！！](nameplate.webp)

Klab株式会社さんで実施されている学生向けインターン(というよりはワークショップ?)イベントです．今回参加した第6回はオンラインで開催され，5日間の日程でTCP/IPプロトコルスタックを自作，という内容でした．

ハードなスケジュールではありましたが，[山本さん](https://twitter.com/pandax381)を始めとする講師の方々の手厚いサポートもあり，なんとか実装を終えることができました．(山本さんの強力な人力デバッギングと目grepには感動しました．) 講義で使用された資料，サンプルコード等は，山本さんの固定ツイートから見ることができます．

プロトコルスタック自作に興味のある方は，是非次回の開催情報を確認してみてください！ExpertCamp運営・採用担当の[Kの27乗さん](https://twitter.com/oktillion27)のtwitterでも最新情報が発信されています．


## 最終的に何ができた？

{{< tweet user="salty_poison" id="1696764537276624987" >}}

{{< tweet user="salty_poison" id="1696795924608266261" >}}

TCPの3Way-Handshakeでセッションを確立し，その上でペイロードを送受信できるプログラムが完成しました！
RFC793をベースにしたソケットAPIっぽいものができるので，サクっとインチキhttpサーバを建ててみた．

## 日程の詳細とか

### Day 1

プロトコルスタック全体のアーキテクチャ解説，割り込み処理，dummyデバイスの実装をしました．
以降の講義においては，まず座学での解説があったのち，山本さん自作のプロトコルスタック[(microps)](https://github.com/pandax381/microps)をベースにした[スケルトン](https://github.com/pandax381/microps/tree/bdbf73b0b2decec29c0f58e4c5187c8dc21014fb)に対してロジックを実装する，という流れで進みました．
参加者にTwitterで見たことある人も何人かいて勝手に親近感を覚えていた．

{{< tweet user="salty_poison" id="1694532438163382452" >}}

[code](https://github.com/tsukudani0412/microps/tree/387d89e8ed42fab98003afc3161c9d16b8c44344)

### Day 2

論理IPインターフェースの実装，IPパケット入出力の実装，ICMP入出力の実装．
IP，ICMPができたので，loopback経由でpingが打てるように！！

{{< tweet user="salty_poison" id="1695060484796379403" >}}
色分けがないとログを読めない人種なので，気合と筋肉で色付け．

[code](https://github.com/tsukudani0412/microps/tree/3ff7a8964721cbff8d8b8a86c32b58aa578b4b47)
[diff](https://github.com/tsukudani0412/microps/compare/387d89e...3ff7a89)

### Day 3

イーサネットデバイス，ARP (Address Resolution Protocol)の実装を行いました．
ARPの実装により，自インターフェース以外との通信が可能になった！

余談ですが，ARPのRFC(RFC826)はかなり読みやすかったので，入門にオススメです．

{{< tweet user="salty_poison" id="1696085656676745279" >}}
感動の瞬間である．

[code](https://github.com/tsukudani0412/microps/commit/73cfbc385c47bd13ab96026f3526c014ef17bf07)
[diff](https://github.com/tsukudani0412/microps/compare/3ff7a89...73cfbc3)

### Day 4

IPルーティング，UDPの入出力を実装．割り込み処理周りはかなり厄介で，気付かぬうちにバグが潜んでいることもしばしば…

IPルーティングによって，自サブネット外のホストとの通信が可能になりました！
UDPの実装によってUDP echo serverが動作するようになりました．いよいよプロトコルスタック感が出てきた感じ．

{{< tweet user="salty_poison" id="1696359189168439412" >}}

{{< tweet user="salty_poison" id="1696441205591278031">}}

[code](https://github.com/tsukudani0412/microps/commit/be0c0c41387505ae948e9a39d68f2d629bfd5ff6)
[diff](https://github.com/tsukudani0412/microps/compare/73cfbc3...be0c0c4)

### Day 5

いよいよ最終日！
TCPを実装．UDPと比較するとかなり複雑でしたが，なんとか実装を終えることができました．

{{< tweet user="salty_poison" id="1696764537276624987" >}}

{{< tweet user="salty_poison" id="1696795924608266261" >}}
TCPでの通信が可能になったので，[インチキHTTPサーバ](http://microhttp.sakasausagi.com)を動作させてみた．

{{< tweet user="salty_poison" id="1696834484623278584" >}}

{{< tweet user="pandax381" id="1696849739998966088" >}}
かなりパンチの強い内容で笑ってしまった．

### Day 6?

Day 5で実装したTCPは完全なものではなく，特にセッション切断まわりの実装が不十分(時間的に全て実装するのはムリ)だったので，RFCとmicropsのmasterブランチを参考に改修を行いました．

ついでにdhclientもどきを実装．

{{< tweet user="salty_poison" id="1699085421496680532" >}}

{{< tweet user="salty_poison" id="1700191974329037129" >}}



## まとめ

大変学びのある1週間となりました．通常，プロトコルスタックはOSによって隠蔽されているため，ユーザから触れることができるのはソケットAPI等の抽象化されたものです．ソケットの先，もう少し深い領域に触れることのできるまたとない機会でした．
今後，時間を見つけて自作OSへの移植もしてみたいです．

このような素晴しいイベントを企画，開催してくださったKLab株式会社の皆さまに感謝申し上げます．ありがとうございました．

{{< figure src="certificate.webp" title="修了証も頂けて大変嬉しい" width="80%" >}}


Tsukudani.
