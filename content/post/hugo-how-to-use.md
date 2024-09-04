---
title: "Hugo の使い方"
date: 2019-02-20T16:15:13+09:00
description: "Hugo の Tips 的なものをこちらにまとめます。"
categories: ["homepage"]
tags: ["hugo"]
image: "/images/writing-memo-note-pen_640.webp"
imageDescription: "ノートにメモメモ"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 参考サイト
[Google先生](https://www.google.co.jp/)  
もう、検索し過ぎて、どこのサイトを参考にしたのか分からなくなりました。

## サイト作成
$ `hugo new site サイト名`

## フォルダ構成（例）
サイト作成をすると下記のようなフォルダ構成が作られる。  
（me）と書いたところは、必要になったら自分で作成するフォルダなので、サイト名直下のフォルダまでが自動生成かな。
```txt
    サイト名/
        ├ archetypes/   記事のテンプレートを入れるところ
        ├ content/      記事を入れるところ
        │   └ post/     （me）
        │   └ blog/     （me）
        ├ layouts/      テンプレートとなる HTML を入れるところ
        │   └ _default  （me）
        │   └ partials  （me）
        ├ public/       自動生成された公開ファイルが入るところ
        ├ static/       素材関係を入れるところ
        │   └ css/      スタイルシートを入れるところ（me）
        │   └ images/   画像ファイルを入れるところ（me）
        ├ theme/        テーマファイルを入れるところ
        │   └ aether/   aether というテーマのフォルダ（me）
```
## 記事作成
$ `cd サイト名`  
サイト名$ `hugo new post/hoge.md`

`new` のあとにフォルダを指定することで、`content` 以下に自動でフォルダが作成され、その中に記事用のファイルが作られる。

記事は、テンプレートからタイトル、作成時間等は自動で代入されるが、記事本文や下書き状態から公開状態への切り替えなどは自身で行う。
Markdown 記法となるため、対応したエディタを使うと良い。

ちなみに [k-kaz](https://k-kaz.net "k-kaz.net") は [VSCode](https://code.visualstudio.com/ "Visual Studio Code") で編集している。

## テーマインストール
テーマサイトの中から、好きなタイプのものを選択する。
選択後、テーマの詳細が表示されるので、自分が使いたい機能が搭載されているか、事前にチェックする。（最悪、テーマを使わず、自力で機能搭載）

そのページにはインストールの仕方も書いてあるが、`サイト名/theme/` の中にきちんと入れること。

## テーマの適用
ルート（サイト名直下）にある `config.toml` を開き、`theme = ""` にインストールしたテーマ名を入れる。  
今回の場合は `theme = "aether-kaz"` となる。

## テーマ及びスタイルシート等の扱い
テーマ内のファイルは直接修正をせず、ルートにある各フォルダにファイルをコピーして修正をする。  
それらファイルはテーマ内のものよりも優先される。  
どこに置くかは、テーマ内を確認する。
```txt
    HTML    → layout/_default/
    HTML    → layout/partials/
    CSS     → static/css/
    IMAGE   → static/images/
```
## 記事の公開
### 記事内の修正
`draft: true` を `draft: false` に変更する。  
下書き → 公開 という感じ。
### GitHub Pages へアップ【GitHub】
サイト名$ `hugo`  
サイト名$ `git add -A`  
サイト名$ `git commit -m "コミット内容"`  
サイト名$ `git push origin master`

一番上で、`public` の中身を生成し、その後 `git` のコマンドでリポジトリサーバーへアップをしている。毎回行うのは面倒なので、スクリプトを組んでおくと楽。

また、ここでは簡単に書いているが、現在のサイトについては下記のように公開用（`public`）とソース管理用で、2回に分けてアップしている。

サイト名$ `hugo`  
サイト名$ `cd public`  
サイト名/public$ `git add -A`  
サイト名/public$ `git commit -m "コミット内容"`  
サイト名/public$ `git push origin master`  
サイト名/public$ `cd ..`  
サイト名$ `git add -A`  
サイト名$ `git commit -m "コミット内容"`  
サイト名$ `git push origin master`

## ssh接続【GitHub】
ssh接続用に、公開鍵と秘密鍵を作成する。  
公開鍵の中身を GitHub に登録しておく。

参考：[GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

### https 接続から、ssh 接続に切り替える
`$ git remote -v` で表示されたアドレスが `https://github.com/` の場合は、`git@github.com:` に変えることで ssh 接続にできる。  

切替用コマンド `$ git remote set-url origin git@github.com:hogehoge.git`  
確認用コマンド `$ ssh -T github`


---
こちらも随時更新していきます。