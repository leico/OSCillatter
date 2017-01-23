---
title: OSCillatter

---

# OSCillatter

## Sound & Recording Magazine 2017年３月号 Maxで作る自分専用パッチ補足ページ

### 制作の経緯とか動機とか目標とか

__利用方法等をさっさと知りたい方は読み飛ばして下の方にあります。__

---

> 何かネタないでしょうか？

とお話がきたからなのですが、それじゃあ味気なさすぎるのでこのコラムの依頼をきっかけになぜエクスターナルを作ったかとか書きます。

<blockquote class="twitter-tweet" data-conversation="none" data-lang="ja">
  <p lang="ja" dir="ltr">
  自分が寄稿している今回のサンプルパッチはちょっと特殊で、音をどのようにハックするかになりがちなプログラミング言語Maxで、<br>
  既存の枠組みをどのようにハックするかに挑戦し、その一例を提示しています。<br>
  売り上げで収入増えたりしませんが、ちょっと異質なMaxを是非ご覧ください。
  </p>&mdash; leico (@1_0101) <a href="https://twitter.com/1_0101/status/821175657195454465">2017年1月17日</a>
</blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

何を提供するか/作るか迷いましたが、今回は音じゃなく音周辺を魔改造することにしました。
やっていることはこの2つです。

* できない機能をエクスターナルで実現する
* 普通思いつかない利用方法で可能性を拡げる

何をどうするかはサンレコのWebか雑誌で確認ください。
ご覧になってる方の創作や発想のヒントになれば幸いです。


#### 前身の作品の話

<iframe width="560" height="315" src="https://www.youtube.com/embed/ckrZhHWd8f4" frameborder="0" allowfullscreen></iframe>

これが __ray.twitteroauth__ の元です。私の別のプロジェクトで [__ray.sniff~__](https://cycling74.com/project/ray-sniff-record-our-web-field/)
というのがありまして、それと[Twitter Streaning API](https://dev.twitter.com/streaming/overview)を組み合わせTwitterをベースに様々な情報を送り合ってみんなで演奏しよう。
という試みです。動画は途中バージョンのものでHEAD<sup>[1](#1)</sup> はもう少し変化しています。

この時もTwitter Streaming APIを利用していました。
Twitterに貼られる画像や[SoundCloud](https://soundcloud.com/)の音源、
誠に残念ながら[今は亡きVine](https://vine.co/)動画、[gmaps.js](https://hpneo.github.io/gmaps/)による位置情報表示をWebブラウザが担当し、
OSCでMaxを制御する部分はPHPサーバから[Open Sound Control for PHP](http://opensoundcontrol.org/implementation/open-sound-control-php)
を利用してMaxにOSCデータを送っていました。

……これHEADの話かもしれません。この動画の時点ではPHPでのTwitter Streaming APIのテストがうまく動かなくて、
C言語と[cURL](https://curl.haxx.se/)を使った方法では動いていたので、
PHPからC言語のプログラムを[exec](http://php.net/manual/ja/function.exec.php)
で呼び出し、出力をPHPで受け取り、OSCはosc.phpでMaxへ送るようにしていた気がします。

最終的に、__ray.sniff~__ からの通信データの音、SoundCloudの音源、
Vineのループ音源とループ映像/投稿された画像によるVJ---みんなで寄ってたかって
行うセッション。全てのデータ共有が映像へ音へ反映され、コミュニケーション即ち音源となり得る場を作り出す試み。
それがこのプロジェクトです。

#### ray.twitteroauthつくることになってしまった話

OSCtterのTwitter Streaming API部分をMaxオブジェクトにしたのがray.twitteroauthです。

__これができたら誰でもTwitterでOSCをツイートして遠隔地にあるMaxパッチを、ほぼリアルタイムに制御することができるんですよ。
アドレスさえ知ってたら会場／配信関係なく観客全員がライブに参加できるんですよ？ 何か専用アプリ入れる必要もなしに。すごくないですか？__ 

というのが製作動機かつ、掲載のお話をいただいた時に推した部分です。

最初は[maxurl](https://docs.cycling74.com/max7/maxobject/maxurl)でできるんじゃないかと思っていました。
maxurl、ヘルプ見ていただくと分かるのですが、さっき登場したcURLをMaxで使えるようにしたオブジェクトなんです。
[こんなサンプル](https://cycling74.com/2014/06/09/use-maxurl-to-create-a-realtime-instagram-collage/#.WIYCDZK5yA0)
([日本語はこっちが詳しい](http://mirror.boy.jp/?p=1969))
がアップロードされているし、これでいけんじゃね？　使ったことあるから大体の使い方わかるし。って簡単に考えてました。

まー無理でしたね。maxurlが使いにくいのと、[Twitter OAuth](https://dev.twitter.com/oauth)
突破用[REST API](https://www.amazon.co.jp/dp/4774142042)ヘッダの準備が無理でした。

なので、前回同様いろんなライブラリを引っ張ってきて継ぎ合わせて、
前回のC言語のプログラムを流用してMaxオブジェクトを作ろうってなったんです。

### 制作ぐだぐだ話

__ここ飛ばして大丈夫です。__

まー動かないっすわ。Twitterの仕様が変わったのか、それともライブラリ使い方間違っているのか。
開発と同時期に[Twitter DevelopperサイトのOAuth Testが落ちてるし](https://twittercommunity.com/t/404-sorry-that-page-does-not-exist-with-button-oauth-test/77056/3)。

あと[libOAuth](https://sourceforge.net/projects/liboauth/)([Github](https://github.com/x42/liboauth))
の開発が止まっているのか最新の[OpenSSL](https://www.openssl.org/)に対応していない。

他のライブラリならよかったのだけれど、OpenSSLの更新追従できてないのはマズいだろう、
と思ったのでOpenSSL最新のものに対応した[libOAuth](https://github.com/leico/liboauth)を作ったり。

動くようになってMaxに移植したらMax自体がフリーズして戻ってこなくなり、
cURLのドキュメント読んで[内部処理を変更](https://curl.haxx.se/libcurl/c/multi-single.html)したり。

そんなことをしていたら原稿の締め切り直前までプログラムが動かないという。
無事期限内にデータを渡すことができてほんとよかった。

### 制作したエクスターナルの紹介

今回実は２つエクスターナルを作っています。
ざっくり説明すると以下

* ray.twitteroauth
  * TwitterOAuth認証を行うためのオブジェクト
* ray.OSCiter
  * 複数のOSCメッセージを含んだ文字列からjson毎に切り出して出力
    * before
    ```
    /aaa/bbb 1 /aaa/aaa 3 /ccc/x 200
    ```
    * after
      * `/aaa/bbb 1`
      * `/aaa/aaa 3`
      * `/ccc/x 200`

### 現状の注意点

#### macOS 64bitのみで動作します。

32bit版、及びWindows版は間に合いませんでした。

#### GETメソッドしか動きません

* [sample.json](https://dev.twitter.com/streaming/reference/get/statuses/sample)
* [user.json](https://dev.twitter.com/streaming/reference/get/user)

しか対応してないです。自分のタイムラインか全ツイートの1%は取得できますが、
ハッシュタグや発信地域でフィルタリングすることができません。

### [macOS] Maxを64bitで起動する方法

![Maxについて]({{site.github.url}}/images/macOSMaxVersion/00_aboutMax.png "Maxについて")

ツールバーの`Max -> About Max`でMaxのバージョンが確認できます。

__インストール時の初期設定ではMaxは32bitで動作しています。__

__過去開発されたエクスターナルの多くが32bitのみ対応で放置されているからです。__

__求ム:エクスターナルのコントリビューター__

Maxのバージョンの変更方法です。Finderでアプリケーションフォルダを開き、
`Max.app`で右クリック、情報を見るを選択します。

![情報を見る]({{site.github.url}}/images/macOSMaxVersion/01_openInfo.png "情報を見る")

`Max.appの情報`ウィンドウ中央辺り、

![Max.appの情報]({{site.github.url}}/images/macOSMaxVersion/02_info.png "Max.appの情報")

- [x] 32ビットモードで開く

となっているので

- [ ] 32ビットモードで開く

と変更します。

__この状態でMaxを起動した時32bitのままの場合は、macOSを再起動させてみてください。__

これで64bitモードで動作します。

### Twitter Streaming APIを使うために必要な準備

参考

* [Twitter Developersでアプリを作成し、APIキー、アクセストークンを取得する手順](http://wplogs.com/twitter-apps/)

#### 電話番号を登録する

![電話番号登録画面]({{site.github.url}}/images/twitterApps/00_phoneNumber.png "電話番号登録画面")


> Twitterアプリを作成するには、まず電話番号の登録が必要です。
> 
> アプリを作成するTwitterアカウントにログインして、「設定」メニューの「モバイル」を開きます。
> 
> 以下のように、電話番号を入力します。
> 
> * 「国／地域」…「日本」
> * 「電話番号」…先頭の「0」が「+81」に変わるので、それ以降の番号を入力
>
> 電話番号を入力したら、その電話番号宛にショートメール（SMS）が送信されるので、
> そこに記載されている認証用コードを入力します。
>
> ---
>
> - [Twitter Developersでアプリを作成し、APIキー、アクセストークンを取得する手順](http://wplogs.com/twitter-apps/)

#### Twitter Appsでアプリ作成

[https://apps.twitter.com/](https://apps.twitter.com/)にアクセスします。

![Twitter Application Management]({{site.github.url}}/images/twitterApps/01_createApp.png "Twitter Application Management")

画面の __Create New App__ をクリックします。

#### アプリの概要を記述して同意

![App Description]({{site.github.url}}/images/twitterApps/02_appDescription.png "App Description")

画像の内容を、各々の目的や領域に合わせて記述。利用許諾に同意して`Create your Twitter Application`でアプリケーションを作成。

作成成功すると作成したアプリの`Details`タブにジャンプする。
よく他の人と名前被りが発生して作成できない。

#### 各種キーを取得

必要となるのは

* Consumer Key(API Key)
* Consumer Secret(API Secret)
* Access Token
* Token Secret

の4種類。

自分のアカウントのキーに関しては`Keys and Access Tokens`タブで確認、取得が可能。

![API Key and Token]({{site.github.url}}/images/twitterApps/03_apiKeys.png "API Key and Token")

Access Token、Token Secretはページ下部、Create my access Token をクリックすると取得できる。

![Token and Secret]({{site.github.url}}/images/twitterApps/04_token.png "Token and Secret")

以上。


### 役立つかもしれない情報

#### Maxエクスターナルオブジェクトとは

連載で既にいくつか出てきてますが、特に解説もなかったと思うので少し紹介します。
Maxに元から入っているオブジェクトで大抵のことは実現することができます。が、それだけでは実現不可能なことが __まれによく__ あります。
そういう問題に直面した際、Maxには他のプログラミング言語でオブジェクト制作する機能があるんです。

その機能を使って、個々人が作ったMaxオブジェクト、それがMaxエクスターナルオブジェクトです。
__パッチではないです__ 。内部は C/C++ 、最近では [FAUST](http://faust.grame.fr/) というものを使って記述されていたり。
DAWで言うところのAUやVSTのようなものです。Maxパッチと同様、そしてVSTと同様にこちらも世界中で制作され共有されてます。

このオブジェクト、デフォルトのMaxでは実現困難な処理を行うものが数多く存在し、速度も高速ですが、OSやCPUに依存します。
macOS用はmacOSで、Windows用はWindowsで制作しなければなりません。
またMaxが32bit版なのか64bit版なのかということも問題になってきます。
32bit用で64bit用は動きませんし、64bit用で32bit用は動きません。

#### `ray.`って何?

[名前空間](https://ja.wikipedia.org/wiki/%E5%90%8D%E5%89%8D%E7%A9%BA%E9%96%93)として使っています。
他のエクスターナル製作者とオブジェクト名が被ってしまうとMaxで呼び出せなくなるので、
自分の製作したオブジェクトには`ray.`を付けています。

インスパイア元はIAMASの[赤松正行](http://akamatsu.org/aka/profile/)教授です。

* [赤松教授が製作したエクスターナル](http://akamatsu.org/aka/max/objects/)

### その他

#### ray.twitteroauth/ray.OSCiter利用ライブラリ

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


---

<a name="1">1</a>
: git用語で最新のデータのこと。
