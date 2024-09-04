---
title: "Mozc Ut2 を Install したよ。"
date: 2019-03-31T17:45:32+09:00
description: "Linux Mint に Mozc Ut2 をインストールしました。"
categories: ["linux"]
tags: ["mozc"]
image: "/images/origami-japan_640.webp"
imageDescription: "鶴の折り紙"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## これまでと急に内容変わりますが・・・
Hugo に関連しませんけど、技術的な話ということでこちらに書いていきます。  

### そもそも Mozc って何？
{{% blockquote author="Wikipedia" link="https://ja.wikipedia.org/wiki/Google_日本語入力" %}}
Mozcとは、Google 日本語入力のオープンソース版である。これは2010年5月11日にGoogle日本語入力をGoogle Chrome OSに移植するために必要な部分をオープンソースしたものである。
{{% /blockquote %}}

まぁ、簡単に言うと Linux で使える Google 日本語入力 です。  
この便利な Mozc ですが、Linux で普通にインストールされるものって、バージョンが古かったり、カタカナ辞書が付いていなかったりするんですよ。  

元々インストールされていたのは、Ver.2.20 で、新たに入れるのは Ver.2.23 です。そして、追加辞書も付いています。

自分で、カタカナ辞書を用意してユーザー辞書に追加もしていましたが、変換時はユーザー辞書が優先されるので、普通にカタカナを入れたいのに、いちいち英語になったりして、面倒だったりね。

なんか、Windows に比べて変換効率も悪いので、なんとかならないかとネットで調べていたところ、最新の Mozc を Linux にインストールする方法が見付かったので、やってみました。

ちなみに、昔は KAKASI（KAnji KAna Simple Inverter）とか使っていましたよ。

### 最新版 Mozc のインストール
先程書きました通り、普通にインストールするとバージョンが古いので、「ソフトウェアの管理」（Linux Mint）は使いません。  

**ソースを持ってきまして、それをビルドして使えるようにします。**

**いきなり難しそうだ（笑）**

#### ソースをゲット
ソースはこちらにありますよ。  
[mozc ut for ubuntu](https://ja.osdn.net/users/sicklylife/pf/mozc_ut_for_ubuntu/wiki/FrontPage)

今回は `mozc-2.23.2815.102+dfsg~ut2-20171008d+20181201.tar.xz` を利用しました。

ダウンロード後、解凍しておきます。

#### ビルド
解凍したところに `build_mozc_plus_utdict` というファイルができますので、これを実行します。  
でも、足りないパッケージなどがあるので、事前に入れておきます。

```cmd
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install -y devscripts debhelper libibus-1.0-dev pkg-config libxcb-xfixes0-dev libgtk2.0-dev python-dev gyp protobuf-compiler libprotobuf-dev qtbase5-dev libqwt-qt5-dev libgwengui-qt5-dev libuim-dev libzinnia-dev fcitx-libs-dev gettext desktop-file-utils ninja-build
```

入れ終わったら、ビルドします。

```cmd
$ sudo ./build_mozc_plus_utdict
```

しばらく、ハードディスクがカリカリと頑張ってくれます。  

運悪くビルドに失敗したら、ログに理由が書いてあると思いますので、**ネットで頑張って調べます（笑）**

#### 事後処理
無事にビルドが終了したら、インストール済みのパッケージを削除します。

```cmd
$ sudo apt-get remove mozc-server mozc-utils-gui
```

fcitx-mozc を使っているので、関連ファイルをインストール。

```cmd
$ sudo dpkg -i mozc*.deb fcitx-mozc*.deb
```

結果、こうなります。
<img src="/images/mozc-ver223.webp" alt="Mozc について" style="width:auto;">

以上です。

#### 参考サイト
今回の作業は、下記、2サイト様を参考にさせていただきました。  
ありがとうございます。

[Linux Mint 18 : 「Mozc UT2 Dictionary」「郵便番号」「ニコニコ大百科」「顔文字」で辞書を強化する](http://baker-street.jugem.jp/?day=20170407)  

[Mozc UT2をインストールする - Ubuntu 18.04編](https://sicklylife.jp/ubuntu/1804/mozc_ut2.html)

