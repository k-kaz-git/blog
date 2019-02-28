---
title: "GitHub を使ってみる"
date: 2019-02-22T15:49:24+09:00
description: "GitHub を有効活用しようと思います。"
categories: ["coding"]
tags: ["github"]
featuredImage: "/images/network-server_640.webp"
featuredImageDescription: "ネットワークとサーバーのイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## せっかくなので
GitHub をもう少し活用できないかなと思いまして、こちらにやってみたことを書いていこうと思います。 

後日、同じことをやるときに **絶対忘れている** という自信があるので、備忘録的に。

## ssh接続【GitHub】
ssh接続用に、公開鍵と秘密鍵を作成します。  
出来上がった公開鍵の中身を GitHub に登録しておきます。

参考：[GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

### https 接続から、ssh 接続に切り替える
`$ git remote -v` で表示されたアドレスが `https://github.com/` の場合は、`git@github.com:` に変えることで ssh 接続にできます。これで、毎回、ユーザー名とパスワードを聞かれなくなります。

切替用コマンド `$ git remote set-url origin git@github.com:hogehoge.git`  
確認用コマンド `$ ssh -T github`

## 各種設定ファイルの共通化
Linux と Windows で共通して使うアプリが多く、その設定をそれぞれで行うのも面倒だし、間違えることもあるので、設定ファイルを使い回すようにしたいのです。

GoogleDrive、DropBox、OneDrive 等々に、コピーした設定ファイルを保存して、相手先で取り出して上書きするっていうのがこれまでのやり方なんですが、何度も設定変更を行うようなものだと、嫌になります。
### そこで GitHub
GitHub を使えば、push と pull するだけでファイルの行き来ができる。  
さらにバージョン管理まで出来ちゃうじゃないか。
### やり方
いくつか作業が続きます。

#### シンボリックリンクの作成
フォルダ及びファイルの置き場所を、本来ある場所から、GitHub で管理する場所に変更します。  
これを Linux と Windows 両方やっておけば、いつでも最新で同じ内容の設定ファイルが使えます。  
※`push` と `pull` を忘れないように。

Linux なら `ln` コマンドで。  
Windows なら `mklink` コマンドで。

Windows でちょっとハマった。  
Win10 の PowerShell では、直接 `mklink` が実行できないんです。  
`cmd /c mklink` というようにすると動きます。  
※mklink は、cmd の中にあるコマンドのため

#### リモートリポジトリの作成
GitHub でリモートリポジトリを作成します。
私は web で行い、Private モードとしておきました。（最近、無料ユーザーでも Private を使えるようになった）
#### ローカルリポジトリの作成
ここから先は、私が苦手な（よく分かっていない）ところです。  
まず、リポジトリとするディレクトリに移動しておきます。  

`git init`  
`git add .`  
`git commit -m "コメント"`
#### リモートリポジトリへプッシュ
`git remote add origin git@github.com:ユーザー名/リモートリポジトリ名.git` 

※最後の `.git` を忘れがちなので注意  
※git@github.com にしているのは ssh で接続するため
`git push -u origin master`  

#### 相手側で、リモートリポジトリからのデータ取得
初回のみ、クローンで丸々ゲットしてきます。  

`git clone git@github.com:ユーザー名/リモートリポジトリ名.git 展開するディレクトリ`

それ以降は、`git pull` でOKです。

よし、いろいろなデータをバージョン管理してみようではないか。  


## Hugo で使っているローカルリポジトリの場所を変更
設定ファイルを1ヶ所にまとめていくうちに、Hugo で作ったサイトのリポジトリも同じところに移動したくなったので、心配もあるけどやってみます。
### やり方
リポジトリを起きたい場所に移動して、  

`git clone git@github.com:ユーザー名/リポジトリ名 変更先ディレクトリ`  

`hugo server` を実行したときに元の場所を見に行かないか心配しましたが、`変更先ディレクトリ` の中で実行すれば、Hugo はその中を見に行くようです。

特に Hugo の設定ファイルを弄るとか必要はありませんでした。

結果から見れば、超簡単な作業でした。

## プッシュしたけど、1つ前の内容に戻したい［2019-02-25］
リモートリポジトリにプッシュしたあと、「あっ、やっぱり前のほうが良かった」と思ったときの手順です。  
ネットで調べたら、間違えたものも履歴として残す方法もありましたが、個人で使う分にはゴミになるだろうと思いまして、シンプルに「先程プッシュしたものは、無かったことにする」という方法を採用します。
### やり方
プッシュしたディレクトリに移動して、

`git reset --hard HEAD^` をして、  
`git push -f origin master` です。

上記は、あくまでも個人ユースの場合に使うコマンドのようです。  
**チームで管理しているときは、別の方法をとってください。**  
（未熟な私にはまだ分からないけど）

## リポジトリから除外したいファイルやフォルダがある［2019-02-26］
VSCode の設定ファイルをリポジトリに入れて共通使用としたんですけど、不要なファイルもあるんですよね。  
タブの状態（何のファイルを開いているか）は要らないし、というか、ファイルの置き場や所有しているファイルも違うし・・・。

### やり方
#### .gitignore に除外するものを書きますよ
`git` を実行するディレクトリに `.gitignore` というテキストファイルを用意します。  
この中にリポジトリから除外するファイルやフォルダを書き込むと、そのあたりを考慮してリポジトリを作成してくれます。  

VSCode で不要なディレクトリを書き込みました。
```.gitignore
[.gitignore]

VSCode/User/globalStorage/
VSCode/User/workspaceStorage/
```
#### すでにリポジトリが出来上がっている場合
git から情報を消し、改めて add → commit → push します。

`git rm -r --cached .`  
`git add .`  
`git commit -m "コメント"`  
`git push origin master`  

相手側はどうすればいいのかな。

普通に `git pull` でやっても怒られたので、ディレクトリ毎、まっさらにして  

`git clone git@github:ユーザー名/リポジトリ名.git`  

で、解決したことにしちゃった（笑）

## VSCode の設定ファイルを共通で使う［2019-02-28］
上のほうにやり方を長々と書きましたけど、プラグインがあるらしいぞ。  

[VSCode(Visual Studio Code)の設定を同期させる拡張機能「Setting Sync」が便利](https://www.karelie.net/vscode-setting-sync/)


