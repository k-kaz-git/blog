---
title: "Hugo を Netlify で動かすよ。"
date: 2019-03-07T14:44:55+09:00
description: "Hugo を Netlify で動かしてみたときの記録です。"
categories: ["homepage"]
tags: ["Hugo","netlify"]
image: "/images/hugo-netlify-network_640.webp"
imageDescription: "地球とネットワークのイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 追記 2024-02-07

もう5年も前の記事なんですね。時が経つのは早いです。  

Hugoのバージョンを上げたので、Netlifyで指定しているHugoのバージョンも変えとかなきゃと設定を見たんですが、項目が無くなっていました。  
今ではバージョン指定しなくて良いのかもしれません。（試さず書いちゃうけど）  

今は設定のBuild & deployにあるBuild Settingsが該当項目みたいですが、Build commandを「hugo」、Publish directoryを「public」にすれば良いみたい。  
一応、ドキュメントがありましたので置いておきます。  

[Framework integrations | Netlify Docs](https://docs.netlify.com/integrations/frameworks/#hugo)  

ということで、以下の記事は古いですよということを言いたかった。  

## Hugo を Netlify で動かす
ネットでは簡単に出来ると書いてありましたけど、けっこうハマったので、そのあたりも書こうと思います。

結論としては、動きました。  
[k-kaz が好きなことをするサイト](https://k-kaz.netlify.com)

## まずは Hugo をゲットする
### ダウンロード
公式サイトから、OSにあった最新版を落としてきてください。  
Sass を使いたい場合は extended版 が必要になりますが、ここでは普通のタイプで良いです。  
※理由は後ほど・・・

[Releases gohugoio/hugo](https://github.com/gohugoio/hugo/releases)
### パスを通す
解凍したら、どこに居ても、Hugo を実行できるようにするため、パスを通します。  
やり方は・・・簡単なのでネットでご覧ください。  
Windows の場合は、環境変数のところを弄ります。

## ローカルサイトを用意する
### サイトの雛形を展開する
今後、作業をする場所に移動して、下記コマンドを実行します。

`hugo new site hoge-site`

`hoge-site` というディレクトリができて、その中に必要なファイルが展開されます。

### テーマの準備
テーマサイトから好きなテーマを選択します。  
[Hugo Themes](https://themes.gohugo.io)

テーマを選ぶと、その中にインストール（ダウンロード）の仕方が書かれていますので、それに従います。

ちなみに私の選んだ `aether` で説明しますと、

`git clone https://github.com/josephhutch/aether.git themes/aether`

これを実行することでダウンロードしてくれます。  

テーマのダウンロードは、テーマフォルダに移動してからコマンドを実行するパターンが多いです。  
ただ、このテーマはルートで実行するようにコマンド指定されていますのでご注意ください。  
※`themes/aether` とダウンロード先を指定しているので、テーマ内で実行すると `themes/themes/aether` となり、重複してハマります。そして気付きにくい。

#### テーマ内の .git を削除
テーマの中に .git というフォルダがあります。  
あとで面倒なので、削除します。

##### 補足［2019-03-10］
他のテーマを使ったときに気付いたんですが、サブモジュール化すれば削除しなくて良いみたいです。  
というか、そのほうが良いみたいですね。テーマのアップデートできるし。  

別テーマですけど、こんな感じです。

```git
git submodule add https://github.com/zwbetz-gh/cupper-hugo-theme.git themes/cupper-hugo-theme
git submodule update --remote --merge
```

### 設定ファイルの修正
ルートにある `config.toml` が設定ファイルです。  
エディタで開いて、最低限これらを書いてください。  
baseURL はあとで、実際の公開アドレスに直せばよいです。
```toml
baseURL = "https://hoge/"
languageCode = "ja"
title = "k-kaz"
theme = "aether"
```

## サイトを確認する
### まずはローカルで確認する
ローカルサイトのルートで下記を実行します。

`hugo server -D`

ブラウザで `localhost:1313` にアクセスすると、テーマと同じ画面（記事無し）が出てきます。  
実行時にエラーになったり、ブラウザで表示しないときは何かが間違っています。

## GitHub の準備
### GitHub でリポジトリを作る
[GitHub](https://github.co.jp/) にログインして、右上にある "＋" をクリックし、「New repository」を選びます。

* Repository name   ：好きな名前を付けてください。
* Description       ：好きに書いてください。（空欄でも可）
* Public/Private    ：お好きな方に。
* README            ：作らなくて良いです。

GitHub Pages は、Public しか選べませんが、Netlify は Private も選べます。  
私はせっかくなので、Private としました。

### リモートリポジトリとローカルリポジトリを繋げる
リポジトリ作成後に出てくる画面で、2番目にあるものを実行しますが、ちょっと注意。  
いきなり実行すると、リポジトリが無いですよと怒られます。  
最初にこちらを実行してください。

`git init`

イニシャライズをします。  
これをすると、.git というフォルダが作成されます。  
※さっき、テーマで消したアレだ！

その後、画面に出ていたコマンド＋アルファを実行します。

`git remote add origin https://github.com/hoge.git`  
`git add .`  
`git commit -m "First Commit"`  
`git push -u origin master`


尚、ssh接続が可能になっている場合は、

`git remote add origin git@github.com:hoge.git` 

となります。

これでリポジトリが繋がり、ローカルにあったファイルがすべて GitHub に反映されたと思います。（空フォルダは反映しないっぽい）

## Netlify
いよいよきました。

### Netlify にログインする
新しくアカウントを作ってもよし、GitHub やその他のサービス経由にしてもよし。

### 設定をするんですが・・・
GitHub と連携し、どのリポジトリを使うか設定します。  
先程用意したリポジトリを選択しましょう。

あとは自動的にビルドして公開できます・・・のはずが、ここから苦難の道のり（笑）

あまりに苦戦したため、細かい手順は忘れました・・・。  
でも、ローカルサーバーで動いていて、下記のハマりどころだけ注意すれば、大丈夫だと思います。  

私はそのハマりどころに、**長い時間を盗まれました。**

## ハマりどころ
いくつかの問題が絡んでいたので、解決するのに苦労しました。  
分かっちゃえばどうってことないんでしょうけどね。
### Deploy できないよー
GitHub にアップしたのはソースの状態ですので、Netlify の中でビルドをして、ブラウザが認識できる形式にします。  
でも、ここでエラーが何度も出ました。

#### バージョン指定
Hugo のどのバージョンでビルドするのか指定します。  
流れの中でこの設定画面が出なかったので、未指定のままビルドをして、ずっとハマっていました。

設定箇所は、上部メニューの「Settings」 → 左メニューの「Build & deploy」 → 「Build environment variables」です。

Key `HUGO_VERSION`  
Value `0.54.0`

と入れます。

尚、普段は extended版 を使っているので、`0.54.0/extended` と指定しましたが、フリーズしました（笑）

#### Hugo の extended版 には暫定対応
ということです。  
もしも、extended版 を使いたい場合は、回避方法が用意されているので、ネットで検索してみてください。  
私は、そこまでして Sass に拘る必要がないので、素直に css でいくことにしました。

#### テーマの問題
テーマが Sass を使っているケースがあります。  
気分を変えようと思って選択したテーマがそうでした・・・。  
エラーログのところにやたらとテーマの名前が出るので確認したらビンゴ。  

尚、`aether` は css なので大丈夫です。

ダウンロード前に分かれば良いんですけど、詳細ページに書かれていなければ落として確認するしかないのかな。  
テーマ内に `assets` というフォルダがあると、その中に Sass の `.sass` や `.scss` があるかと思います。

### その他
#### 記事が表示されない
ローカルでは表示されているのにアップしたら記事だけ表示されないとアタフタ。  
なんと下書きモードのままアップしていたのでした。  
※ローカルでは、下書きも表示するよう `hugo server` に `-D` のオプションを付けていた。

記事の上部にある `draft: true` を `draft: false` にして、もう一度アップしましょう。

こんな基本的なミスをするなんて。  
そして、基本的過ぎると、そこに気付くのが遅れますね（苦笑）