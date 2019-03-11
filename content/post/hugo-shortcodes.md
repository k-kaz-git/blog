---
title: "Hugo の Shortcodes を使ってみる。"
date: 2019-03-11T17:40:19+09:00
description: "Shortcodes を使うと便利そうなので、ちょっと勉強します。"
categories: ["homepage"]
tags: ["hugo" , "Shortcodes"]
featuredImage: "/images/computer-Programming_640.webp"
featuredImageDescription: "Shortcodes を覚えよう。"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Shortcodes って何？
Hugo では、簡潔に Markdown が書けるように、Shortcodes というものが用意されています。  
記事中でよく使う表現なんかを、別ファイルとして HTML で用意しておき、必要に応じて呼び出す感じ。

[Hugo 本体で用意されているもの](https://gohugo.io/content-management/shortcodes/)、各テーマの中に用意されているもの、自分で用意するもの（自作）の3つがあります。

他のテーマで気に入った Shortcodes があれば、それを別テーマに移植するのも手。  
スタイルシートが絡みますので、ちょっとした編集は必要になります。  
たまに、画像も使っていたりするので、それも見落とさずに必要があれば持っていきましょう。

テーマ内の Shortcodes はとても勉強になるので、よく眺めておきましょう（笑）  
ちなみに置き場所は下記の通りです。

テーマの Shortcodes  
`/theme/hoge-theme/layouts/shortcodes/`

自作の Shortcodes  
`/layouts/shortcodes`

## 使い方
### 基本形
ごく標準的な書き方。

{{% gist k-kaz-git 83aa5e9919d9bab4fd3d83ee6532af6b %}}

※Markdown でショートコードを書くと、バッククォートで囲んでも実行しちゃう・・・。ということで、gist を利用。この gist 読み込みも Shortcodes で行っています。

ショートコード名は、別途用意している HTML のファイル名（.html を除いたもの）になり、これを読み込み、実行します。

例えば、red.html というファイルに下記が記述されていたとします。

```html
<span style="color:red;">赤い字ですよ。</span>
```

Markdown の記事で、

{{% gist k-kaz-git 57c9a781cf43625d44270dd2e0c42494 %}}

と書きますと、

<span style="color:red;">赤い字ですよ。</span>

が出力されます。

### 変数を使う
また、`{{.Get 0}}` という変数を使って、値の代入も可能です。

color.html というファイルに下記が記述されていた場合、

```html
<span sytle="color:{{.Get 0}};">文字の色を変えます。</span>
```
Markdown の記事で、

{{% gist k-kaz-git b2afd861241a83822fed711f581bcc9e %}}

と書けば、`{{.Get 0}}` に `blue` が代入されて

```html
<span style="color:blue;">文字の色を変えます。</span>
```

<span style="color:blue;">文字の色を変えます。</span>

というように。

パラメーターは複数指定できますので、`{{.Get 0}}` `{{.Get 1}}` というように、数字を増やしていけばいくつでも対応できます。

### 変数を分かりやすく文字にする
数字だけだと数が増えた時にゴチャゴチャとしますので、分かりやすいように文字指定にするほうが良いかもしれません。

{{% gist k-kaz-git 61b0ee861abfe4fc7973975bbc3bc4aa %}}

`{{.Get "src"}}` `{{.Get "title"}}` で、それぞれの値を取得することができます。

---

まだ続きます。  
Shortcodes で、html を弄りだすと、Go の基本的な勉強も必要だと感じます。
