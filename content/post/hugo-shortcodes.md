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
※`%` のところは、前を `<` で、後ろを `>` でも良いみたいです。  

ショートコード名は、別途用意している HTML のファイル名（拡張子の `.html` を除いたもの）になり、これを読み込み、実行します。

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

### 開始タグと終了タグを入れる
元となる文章「何かしらの文字列」の前後に開始タグと終了タグを入れる場合はこのようにします。

{{% gist k-kaz-git 59f1832bfd28998120c2935d5f5dc88a %}}

html側では、「何かしらの文字列」を取得するため `{{.Inner}}` を使い、下記のように記述します。

```html
<span style="color:red">{{.Inner}}</span>
```

### 特定の文字列を置換する
Netlify のほうで使っているテーマ Cupper の中に、特定の文字列を置換する Shortcodes がありました。  
使う機会もありそうなので、こちらに書いておきます。  

#### 概要
`[[[` と `]]]` で挟まれた文字列に対して、ハイライトを付けるという Shortcodes です。

`[[[` という文字列を `<span class='highlight'>` に置換し、  
`]]]` という文字列を `</span>` に置換します。

#### html の書き方

```html
{{ $code := .Inner | htmlEscape }}
{{ $code := replace $code "[[[" "<span class='highlight'>" }}
{{ $code := replace $code "]]]" "</span>" }}
```

行数 | 内容
:---: | ---
1行目 | `{{.Inner}}` で範囲指定した文字列を読み込んで、`$code` に代入します。
2行目 | `$code` を読み込み、`[[[` という文字列があれば `<span class='highlight'>` に置換し、再度 `$code` に代入します。
3行目 | `$code` を読み込み、`]]]` という文字列があれば `</span>` に置換し、再度 `$code` に代入します。

#### 記事の書き方
こんな感じで。
{{% gist k-kaz-git 469318917a46ba32a810e5cac8c96817 %}}

#### 結果
```html
何かしらの<span class='highlight'>文字列</span>です。
```

---
これくらい覚えておけば、ある程度自作も出来そうですね。  
また何かあれば追記します。

ちなみに、Youtube や Instagram の埋め込みについても、Hugo 標準の Shortcodes で、出来るようになっています。

## Shortcodes用のHTMlについて

Shortcodes で利用する html の記述方法は紹介しましたが、ちょっとアレンジしたものを紹介します。  

### if文（条件分岐）

本文から受け取った値によって、1つの html 内で出力方法を変化させる方法です。  

実際に行ったケース。  
テキストの文字色を変化させる Shortcodes で、内容によって改行するものと、改行しないものを用意したくなりました。  

- 改行するもの → 段落要素（p）
- 改行しないもの → 行内コンテナ（span）

Shortcodes での記述はこちら。
{{% gist k-kaz-git 4bcefb1b31bf0e94de53a361846a887b %}}
上の type="p" が段落で、下の type="s" が行内となります。  

fr という html ファイルを読み込んでいます。その中身はこちらです。

```html
{{- $han := .Get "type" -}}
{{ if $han | eq "p"}}
<p class="fr" style="font-size: {{.Get "size"}}%;">{{.Get "text"}}</p>
{{ else if $han | eq "s"}}
<span class="fr" style="font-size: {{.Get "size"}}%;">{{.Get "text"}}</span>
{{ end }}
```

ちょっと解説。

```html
{{- $han := .Get "type" -}}
```

$han という変数に本文中から引っ張ってきた `type` の中身を代入します。  

```html
{{ if $han | eq "p"}}
<p class="fr" style="font-size: {{.Get "size"}}%;">{{.Get "text"}}</p>
{{ else if $han | eq "s"}}
<span class="fr" style="font-size: {{.Get "size"}}%;">{{.Get "text"}}</span>
{{ end }}
```

$han の中身が "p" だったら、`<p class ...` の行を実行します。  
$han の中身が "s" だったら、`<span ...` の行を実行します。  


`eq` というのは比較したものが `同じ` という意味です。
