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
というのがありまして、それと[Twitter Streaning API](https://dev.twitter.com/streaming/overview)を組み合わせTwitterをベースに様々な情報を送り合ってみんなで演奏しよう。
という試みです。動画は途中　バージョンのもので HEAD <sup>[1](#1)</sup> はもう少し変化しています。

この時もTwitter Streaming APIを利用していました。
Twitterに貼られる画像や[SoundCloud](https://soundcloud.com/)の音源、
誠に残念ながら[今は亡きVine](https://vine.co/)動画、[gmaps.js](https://hpneo.github.io/gmaps/)による位置情報表示をWebブラウザが担当し、
OSCでMaxを制御する部分はPHPサーバから[Open Sound Control for PHP](http://opensoundcontrol.org/implementation/open-sound-control-php)
を利用してMaxにOSCデータを送っていました。

……これHEADの話かもしれません。この動画の時点ではPHPでのTwitter Streaming APIのテストがうまく動かなくて、
C言語と[cURL](https://curl.haxx.se/)を使った方法では動いていたので、
PHPからC言語のプログラムを[exec](http://php.net/manual/ja/function.exec.php)
で呼び出し、出力をPHPで受け取り、OSCはosc.phpでMaxへ送るようにしていた気がします。

#### ray.twitteroauthつくることになってしまった話

OSCtterのTwitter Streaming API部分をMaxオブジェクトにしたのがray.twitteroauthです。

__これができたら誰でもTwitterでOSCをツイートして遠隔地にあるMaxパッチを、ほぼリアルタイムに制御することができるんですよ。
アドレスさえ知ってたら会場／配信関係なく観客全員がライブに参加できるんですよ？ 何か専用アプリ入れる必要もなしに。すごくないですか？__ 

というのが製作動機かつ、掲載のお話をいただいた時に推した部分です。

最初は[maxurl](https://docs.cycling74.com/max7/maxobject/maxurl)でできるんじゃないかと思っていました。
maxurl、ヘルプ見ていただくと分かるのですが、さっき登場したcURLをMaxで使えるようにしたオブジェクトということもあって。
[こんなサンプル](https://cycling74.com/2014/06/09/use-maxurl-to-create-a-realtime-instagram-collage/#.WIYCDZK5yA0)
([日本語はこっちが詳しい](http://mirror.boy.jp/?p=1969))
がアップロードされているし、これでいけんじゃね？　使ったことあるから大体の使い方わかるし。って簡単に考えてました。

まー無理でしたね。maxurlが使いにくいのと、[Twitter OAuth](https://dev.twitter.com/oauth)
突破用[REST API](https://www.amazon.co.jp/dp/4774142042)ヘッダの準備が無理でした。

なので、前回同様いろんなライブラリを引っ張ってきて継ぎ合わせて、
前回のC言語のプログラムを流用してMaxオブジェクトを作ろうってなったんです。

まー動かないっすわ。


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

<a name="1">1</a>
: git用語で最新のデータのこと。
