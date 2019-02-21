---
title: "Hugo で使える Markdown の記法"
date: 2019-02-21T11:43:14+09:00
description: "自分用によく使う Markdown をまとめます。"
categories: [ホームページ]
featuredImage: "/images/computer-Programming_640.webp"
featuredImageDescription: "Programming Markdown"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## はじめに
Markdown には、基本となるものから、機能拡張されたものまであり、使用する環境によって微妙に使えるコマンド、使えないコマンドがあるようです。  
この前、外部リンクを開くための記述をネットで探して入れてみましたが、残念ながらココでは動かず・・・。

ということで、**Hugo で使える Markdown を、自分のためにまとめておきます。**

Markdown の中で HTML を書いちゃえばなんとでもなりますが（笑）

## Hugo で使える Markdown 記法
普段、よく使うものだけ。  
正式な記法であったり、もっと辞書的なものをお求めの方は、[Google先生](https://www.google.co.jp)にお尋ねください。m(_ _)m

### 見出し（h）
`#` の数で `h1 ～ h5` を表し、その右に見出しの文字列。

    # 見出し（h1）
    ## 見出し（h2）
    ### 見出し（h3）
    #### 見出し（h4）
    ##### 見出し（h5）

### 改行（br）
行末にスペースを2つ付けます。  
見えませんけど、想像してください（笑）  
実際に改行していなくても、スペース2つが間に入っていれば、続けて書いていてもそこで改行されます。まぁ、`<br>` と同じです。

    改行前の文章  
    改行後の文章

### 強調（strong）
`**` で文字列を挟む。

    ** hoge **

### 画像挿入（img）
タイトルは省略可能。

    ![代替えテキスト](/iamges/hoge.webp)
    ![代替えテキスト](/iamges/hoge.webp "タイトル")

### リンク（a href）
残念ながら、`target` は指定できない。  
どうしてもやりたい場合は、HTML で書く。

#### テキストリンク
タイトルは省略可能。

    [hoge](https://hoge.com)
    [hoge](https://hoge.com "タイトル")

#### 画像リンク
タイトルは省略可能。

    [![代替えテキスト](/iamges/hoge.webp "タイトル")](https://hoge.com)
    [![代替えテキスト](/iamges/hoge.webp)](https://hoge.com)

### コード（code）
文中に差し込むコードは ` （バッククォート）で挟む。

    文字列 `コード` 文字列

### 整形済みテキスト（pre）
単純にコードを記述する場合は、タブで行頭を開ける。  
コードの前後にあたる行は空行でないとうまく読み取れないことあり。  
また、特定のフォーマットでシンタックスハイライトを付ける場合は、```html とかにする。  
htmlの部分は、php、md・・・いろいろ入れられるので、ケースに合わせて使用する。

    [hoge](https://hoge.com)
    [hoge](https://hoge.com)

### 水平線（hr）
`***` もしくは `---` で水平線が書けます。  
前後に文字列が入ってはいけません。

    ***

### 取り消し線（s）
`~~` で取り消す文字列を挟みます。

    ~~取り消す文字列~~


---
またよく使うものが出てきたら追記します。


ここに分かりやすく書いてあった。  
[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet "Markdown Cheatsheet")
