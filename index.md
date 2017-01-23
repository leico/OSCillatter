---
title: OSCillatter

---

# OSCillatter

## Sound & Recording Magazine 2017年３月号 Maxで作る自分専用パッチ補足ページ

### 制作の経緯

> 何かネタないでしょうか？

とお話がきたからなのですが、それじゃあ味気なさすぎるのでこのコラムの依頼をきっかけになぜエクスターナルを作ったかとか書きます。
利用方法等をさっさと知りたい方は読み飛ばして下の方にあります。

#### 前身の作品の話

<iframe width="560" height="315" src="https://www.youtube.com/embed/ckrZhHWd8f4" frameborder="0" allowfullscreen></iframe>

これが __ray.twitteroauth__ の元です。私の別のプロジェクトで [__ray.sniff~__](https://cycling74.com/project/ray-sniff-record-our-web-field/)
というのがありまして、それと[Twitter Streaning API](https://dev.twitter.com/streaming/overview)を組み合わせTwitterをベースに様々な情報を利用してみんなで演奏しよう。
という試みです。動画は試作バージョンのもので HEAD <sup>[1](#1)</sup> はもう少し変化しています。

## 制作したエクスターナル

* ray.twitteroauth
  * TwitterOAuth認証を行うためのライブラリ
* ray.OSCiter
  * 複数のOSCメッセージを含んだ文字列からjson毎に切り出して出力
    * before
    ```
    /aaa/bbb 1 /aaa/aaa 3 /ccc/x 200
    ```
    * after
      * /aaa/bbb 1
      * /aaa/aaa 3
      * /ccc/x 200

## 利用ライブラリ

* [zlib](https://github.com/leico/zlib-xcode)
    * libz
* [curl](https://github.com/leico/libcurl-xcode)
    * libcurl
* [OpenSSL](https://github.com/leico/openssl-xcode)
    * libcripto
    * libssl
* [libOAuth](https://github.com/leico/liboauth-xcode)
    * libOAuth
* [libTwitterOAuth](https://github.com/leico/libTwitterOAuth)
    * libTwitterOAuth
* [picojson](https://github.com/kazuho/picojson)

## エクスターナルがエラーになる

### [macOS] Maxを64bitで起動する方法

## Twitter Streaming APIを使うために必要な準備

## ray って何?

---

[1](#1)
: git用語で最新のデータのこと。
