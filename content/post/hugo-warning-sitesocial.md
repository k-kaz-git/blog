---
title: "Hugo Warning .Site.Social"
date: 2024-08-07T08:44:45+09:00
description: "全然気付いていなかった・・・"
categories: ["homepage"]
tags: ["Hugo","Theme"]
image: "/images/hugo-warning-sitesocial.webp"
imageDescription: "WARNING"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## WARN deprecated

`hugo server`を実行すると、  

「WARN deprecated: .Site.Social was deprecated in Hugo v0.124.0 and will be removed in a future release. Use .Site.Params instead.」  

というメッセージが出ていることに気付きました。  

とくに問題が出て困っているわけでもありませんが、気持ち悪いので直しましょう。  

## 意味

「WARN deprecated: .Site.Social was deprecated in Hugo v0.124.0 and will be removed in a future release. Use .Site.Params instead.」  

「`.Site.Social`は、v0.124.0で非推奨となり、そのうち抹消される運命です。`.Site.Params`を使いましょう」という感じでしょうか。  

現在 v0.131.0 を使っているので、今頃気付くなよって感じです。  

## 探す

当サイトのデータが入っているフォルダーに対して検索を実行しました。  

結果、ありませんでした。  

え～と思いながらも、検索文字列を若干変えつつ、該当箇所を発見しました。  
`.Site.Social` でダメでしたが、`site.social`で見付けました。  

### ヒットしたファイル

- themes\PaperMod\layouts\partials\templates\opengraph.html
- themes\PaperMod\layouts\partials\templates\twitter_cards.html

当サイトで利用していないと思われるファイルですが、直さないとWARNINGが出続けたままです。  

更新の多いテーマでしたらその中で修正が入るんでしょうけど、PagerModは安定しきっている（と好意的に捉える）ので、昨年2月から更新がありません。  

公式の修正を待つよりも、自分でやるべきでしょう。  

## 修正

### その前に

テーマ内のファイルを修正した場合、テーマの更新があると上書きされて戻っちゃう可能性があります。  
そのため、修正前にこの2つのファイルを下記へ複製します。  

layouts\partials\templates

同じファイルが存在することになりますが、こちらのフォルダーが優先されるのでオッケーなのです。  

### opengraph.html

最後の行を修正します。

`{{- with site.`Social`.facebook_admin }}<meta property="fb:admins" content="{{ . }}" />{{ end }}`  

↓ こうします。  
`{{- with site.`Params`.facebook_admin }}<meta property="fb:admins" content="{{ . }}" />{{ end }}`  

### twitter_cards.html

最後から3番目の行を修正します。

`{{ with site.`Social`.twitter -}}`  

↓ こうします。  
`{{ with site.`Params`.twitter -}}`  

## 結果

WARNINGは出なくなりました。  

正しい修正なのかは分かりません。  

一応、`hugo server` でWARNINGが出ないこと、無事にサーバーが起動してサイトの表示がされることまでは確認しました。  

参考にされる場合は自己責任でお願いします。  
ダメだったら複製したファイルを消すことで戻せます。  
