---
title: "シンタックスハイライトを自分好みの色にする"
date: 2019-03-14T17:44:58+09:00
description: "シンタックスハイライトの色をスタイルシートで変更してみます。"
categories: ["homepage"]
tags: ["css" , "saas" , "aether"]
image: "/images/colorful-balloons-640.webp"
imageDescription: "カラフルな風船"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## シンタックスハイライトの色を変更したい
グレーの背景でやさしい表現も良いんですが、**やっぱり背景は黒** ですよね。  
どうも、黒好き星人です。

黒になったらどんな感じになるんだろうと、ワクワクしながらブラウザのデベロッパーツールで黒くしてみたら、**文字が見にくい。**

そりゃそうです。  
やさしいグレーの背景に合わせた文字の色ですから。

さぁ、文字色も含めてカスタマイズです。

### シンタックスハイライトって何？
{{% blockquote author="wikipedia" link="https://ja.wikipedia.org/wiki/シンタックスハイライト" %}}
シンタックスハイライト (英: syntax highlighting, 構文強調) とは、テキストエディタの機能であり、テキスト中の一部分をその分類ごとに異なる色やフォントで表示するものである。シンタックスカラーリング (syntax coloring) とも。
この機能により、プログラミング言語やマークアップ言語といった構造化された言語において、その構造や構文上のエラーが視覚的に区別しやすくなるため、ソースコードの記述が容易となる。
{{% /blockquote %}}

## 結果からお見せすると、こんな感じです

### 変更前
<img src="/images/css-code-highlight-before.webp" alt="コードのハイライト 修正前" style="width:auto;">
### 変更後
<img src="/images/css-code-highlight-after.webp" alt="コードのハイライト 修正後" style="width:auto;">

## Hugo におけるシンタックスハイライト
Go の機能を使う方法と、highlight.js を使う方法の2種類が主流のようです。  
私は aether で採用されていることもあり、後者で行うことにしました。

## 私のやり方はイレギュラーかも・・・
私のやり方はちょっとイレギュラーな形かもしれません。  
highlight.js のテーマを変更してみたものの、一部しか反映されなかったんです・・・。

### ということで k-kaz のやり方
aether の中を覗いていたら、シンタックスハイライト用と思われるスタイルシートを発見しました。  

`themes/aether/static/css/xcode.css`

もしかしたら、この影響でうまく反映されなかったのかもしれません。

**というか、絶対そうだ。**

**よく見付けた、私！！**

ここで指定されているスタイルを自分好みに変えていきます。  
基本的には色だけ変えていけば良いのですが、スタイルの組み合わせもご自由に。

`xcode.css` みたいに、スタイルシート専用のファイルを用意しても良いし、既存のファイルに追記する形でも良いです。

私は `assets/sass/hoge.scss` に追記しましたが、新規で用意する場合は `static/css/hoge.css` として `<head>～</head>` で読み込むなり、CSS内で読み込むなりして反映させましょう。

### いまのスタイルシートはこんな感じ
{{% memo メモ %}}
2019-03-15 時点でのスタイルです。  
気分でチョコチョコ変えていきますので、「今と違うじゃんか！」と怒らないでください。
{{% /memo %}}
```css
/* 背景真っ黒！！
   プログラム毎に class が変わるのでワイルドカードで */
code[class*="language-"] {
  background: #000 !important;
  color: #fffaf0;
}

/* ここから下がコード用のスタイル */
.hljs-comment,
.hljs-quote {
  color: #90ee90;
}

.hljs-keyword,
.hljs-selector-tag,
.hljs-literal {
  color: #ffff00;
}

.hljs-name {
  color: #4682b4;
}

.hljs-variable,
.hljs-template-variable {
  color: #660;
}

.hljs-string {
  color: #ff7f50;
}

.hljs-regexp,
.hljs-link {
  color: #080;
}

.hljs-title,
.hljs-tag,
.hljs-symbol,
.hljs-bullet,
.hljs-meta {
  color: #c0c0c0;
}

.hljs-number {
  color: #7cfc00;
}

.hljs-section,
.hljs-class .hljs-title,
.hljs-type,
.hljs-attr,
.hljs-built_in,
.hljs-builtin-name,
.hljs-params {
  color: #87ceeb;
}

.hljs-attribute,
.hljs-subst {
  color: #87ceeb;
}

.hljs-formula {
  background-color: #eee;
  font-style: italic;
}

.hljs-addition {
  background-color: #baeeba;
}

.hljs-deletion {
  background-color: #ffc8bd;
}

.hljs-selector-id,
.hljs-selector-class {
  color: #ff8c00;
}

.hljs-doctag,
.hljs-strong {
  font-weight: bold;
}

.hljs-emphasis {
  font-style: italic;
}
```
---

## せっかくなので、正しいやり方も紹介
いつか、自分が使うことになるかもしれないしね。
### Go 機能を使う場合
今は、Go に搭載されている機能で、シンタックスハイライトが可能になっています。  

#### config.toml で有効化

```toml
pygmentsCodefences = true
pygmentsUseClasses = true
```

#### スタイルの選択

`hugo gen chromastyles --style=hoge > syntax.css`

`hoge` のところを好きなテーマにしてあげると、そのスタイルシートが `syntax.css` に出力され、これを `<head>～</head>` の中で読み込んであげれば良いです。

テーマは下記を参考にしてください。  
[Pygments style gallery!](URLhttps://help.farbox.com/pygments.html)

monokai って結構有名ですよね。  
いろんなエディタのテーマで紹介されます。

### highlight.js を使う場合
私が使っている aether ではこちらを採用しています。  

#### スタイルの選択
こちらにもテーマが沢山あって、下記から選ぶことができます。  
[highlight.js demo](URLhttps://highlightjs.org/static/demo/)

#### 設定の仕方
下記を `<head>～</head>` に入れてあげましょう。

```html
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.15.6/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.15.6/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

`9.15.6` は現在の最新バージョンです。ここは値が変わるので、使用する場合は公式サイトで確認をお願いします。  
尚、一番上にあるファイル名 `default.min.css` を好きなテーマに変えてあげると、スタイルが反映されます。

#### aether の場合
`themes/aether/layouts/partials/scripts.html` で、上記を読み込んでいるので、そこを変えてあげれば良いはずなんですけど、1行目（テーマ）が見当たらず。もしかしたら、そこを省略して、自前のスタイルシートを使っていたのかなと思えてきました。

ということで、最初に紹介した **スタイルシートで調整** という方法をとったんですが、aether の場合はそれが正解っぽいですね。  

他のテーマならこの手順で良いかと思います。
