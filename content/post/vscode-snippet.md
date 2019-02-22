---
title: "VSCode のスニペットを使ってみる"
date: 2019-02-22T08:44:41+09:00
description: "スニペットを使って、入力速度を劇的にアップするのと、覚えきれない Markdown を調べずに使う。"
categories: [ホームページ]
featuredImage: "/images/women-jump-happy_640.webp"
featuredImageDescription: "女性が喜んでジャンプするイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## スニペットがあるじゃないか。
完全に頭から抜け落ちていました。  
今使っているエディタ Visual Studio Code（以下、VSCode）には **スニペット** が機能として搭載されています。

> スニペット（英語: snippet）とは、「断片」という意味である。情報処理の分野ではよく使う短いプログラムコードを統合開発環境から呼び出す機能の事である。また、呼び出される短いコードの事をスニペットと呼ぶ場合もある。  Wikipediaより。

## こんな感じのものです。
<img src="/images/VSCode-snipet-01.gif" alt="snipet の例をアニメーションで" style="width:640px;">

## Markdown でスニペットを使いたい
なんか初期状態ではスニペットが有効になっていないんですよね。  
設定の中に沢山項目があって、そのどれかを有効にすれば良いのかもと思いつつ、面倒なので設定ファイル `settings.json` に直接書き込んでいます。

### settings.json に有効化のための記述を追加
設定ファイルの保存先が、環境（OS）によって違うので、下記の方法が一番楽チンな気がする。

` ファイル → 基本設定 → 設定 → 拡張機能 → settings.json で編集 ` を選択。  

拡張機能のところは選ばなくても良いんですが、これを選ぶとすぐに `settings.json` が沢山出てくるのでそうしています。

ファイルを開いたら、以下を追加しています。  
元々 `｛｝` があるので、その中に入れるようにすると正しく認識してくれます。

```json
[settings.json]

{

"[markdown]":  {
    "editor.wordWrap": "on",
    "editor.quickSuggestions": true,
    "editor.snippetSuggestions": "top"
    }

}
```

### スニペットを登録する
Markdown用のスニペットを登録します。

#### 設定用ファイル Markdown.json を開く
`Markdown.json` というファイルに追記していきます。

`ファイル → 基本設定 → ユーザースニペット → Markdown.json` を選択。

ファイルが開かれるとこんな感じで、サンプルがコメントアウトした状態になっています。 

```json
［Markdown.json］

{

// Place your snippets for markdown here. Each snippet is defined under a snippet name and has a prefix, body and 
// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
// same ids are connected.
// Example:
// "Print to console": {
// 	"prefix": "log",
// 	"body": [
// 		"console.log('$1');",
// 		"$2"
// 	],
// 	"description": "Log output to console"
// }

}
```

サンプルは残しても消しても結構ですが、最初と最後の `{}` は、残してください。  
尚、ここに書かれているサンプルの意味が理解できなくて、ちょっと迷いました。（英語にとことん弱い人）  

#### 標準的なスニペットの登録方法

```json
"スニペット名": {
    "prefix": "キーになる文字列",
    "body":[ "呼び出される文字列" ],
    "description": "このスニペットの説明文"
    }
```

* **スニペット名**  
重複しなければ自由に付けて大丈夫です。  

* **キーになる文字列**  
この文字列を入力（途中でも）することでスニペットが起動します。  

* **呼び出される文字列**  
上記、キーになる文字列をこちらの文字列で置き換えます。  

* **このスニペットの説明文**  
スニペットの候補とともにこの説明文が表示されるので、分かりやすいコメントを書いておきましょう。

例えば、下記のように登録したとします。

```json
"insart image":{
    "prefix": "img",
    "body": [ "![hoge のイメージ画像](/images/hoge.webp)" ],
    "description: "画像を挿入する"
    }
```

そうすると、Markdown での記述の際に  
img と打った時点（もしくは i を入れた時点）で、上記のスニペットが画面に登場し、`Enter` or `TAB` を押すことで置き換わります。

<img src="/images/VSCode-snipet-01.gif" alt="snipet の例をアニメーションで" style="width:640px;">

候補が複数出て、2行目以降を選択したいときは、カーソルキーで選択してください。  
日本語の変換候補と似た感じです。

#### 一歩進んだスニペットの登録方法
先程の登録だと、ベタ打ちというか決め打ちというか、登録したものを呼び出して終わりでした。いつも使うバナーを呼び出すとかなら OK ですね。  

ただ、実際にコードを書いていくにあたっては、呼び出したものを修正したいという場面が多々あります。  

そんなときは `$1` や `$2` を使用します。  
先程と違うのは、`"body"` の中だけです。

```json
"insart image":{
    "prefix": "img",
    "body": [ "![$1 のイメージ画像](/images/$2.webp)" ],
    "description: "画像を挿入する"
    }
```

こんな感じになります。

<img src="/images/VSCode-snipet-02.gif" alt="snipet の例をアニメーションで" style="width:640px;">

”のイメージ画像” は固定ですが、その前は毎回違うという場合。  
上記のように `$1` を入れてあげると、呼び出し後に `$1` の部分にカーソルが挿入されるため、そのまま文字列の入力が可能です。

さらに、`TAB` を押すことで `$2` にカーソルが移動するので、画像のファイル名を入れることができると。  

便利になってきましたね。

#### もう一歩進んだスニペットの登録方法
先程のにちょっと工夫をすると、分かりやすさをプラスすることができます。  
私のように Markdown が頭に入っていない人向け・・・かもしれません。

こちらも `"body"` の中だけが変わっています。

```json
"insart image":{
    "prefix": "img",
    "body": [ "![${1:画像のタイトル} のイメージ画像](/images/${2:ファイル名}.webp)" ],
    "description: "画像を挿入する"
    }
```

はい、少しややこしくなりました（笑）

動作そのものは先程と同じですが、表示の仕方が変わります。  
とりあえず、画面をみていただきましょう。

<img src="/images/VSCode-snipet-03.gif" alt="snipet の例をアニメーションで" style="width:640px;">

お分かりになりましたか。

`${1:hoge}` という書き方をすると、`$1` としてカーソルが入る部分に、`hoge` という文字列を仮表示してくれます。

上記の場合は、`${1:画像のタイトル}` ですので、`$1` のところに `画像のタイトル` という文字列が仮表示されていて、何かしらの文字を入力すると消えてくれます。

こうしておけば、そこに何を書けば良いのかが一目瞭然。しかも、勝手に消えてくれるので、自分で削除する手間も要りません。  

一番最初の標準的なスニペットから、少しだけ手を入れる必要がありますが、それ以上に見返りは大きいので、是非お試しください。

### スニペットの使い方
すでにここまでのところで書いてしまったので、省略（笑）

Markdown の形式が選択されている状態（ウィンドウの右下に出ています）で、登録したスニペットのキーとなる文字列を入力して、候補から選択してください。

### 注意点
スニペットを複数登録する際は、1スニペット毎に最後の `}` を `},` としてください。  
1スニペット毎にカンマで区切る必要があります。