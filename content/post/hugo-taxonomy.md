---
title: "Hugo のタグとかカテゴリとか"
date: 2020-11-05T12:57:30+09:00
description: "タグとかカテゴリとかの一覧を表示したい"
categories: ["Homepage"]
tags: ["Hugo" , "tags" , "categories"]
featuredImage: "images/labels-4924676_800.webp"
featuredImageDescription: "tags"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo のタグとかカテゴリとか
この前、[k-kaz が自作テーマで遊ぶサイト](k-kaz-0.netlify.app) に [タグとかカテゴリとか](https://k-kaz-0.netlify.app/hugo-tag/) という記事を書きました。

タグとかカテゴリのリンクををクリックしたときに、そのグループだけではなく、**全記事が出てきちゃう！** というものです。  
自作テーマだから、うまく作り込めていないんだと思っていましたが、**このサイトも同じじゃないか。**

以前は正しく動作していたはずなので、Hugo のバージョンを上げたとき、仕様変更によって動作が変わったと思われます。

## ちょろっと調べた
`\layouts\_default` に `taxonomy.html` と `terms.html` を置くと、良いらしい。  
中身の書き方はいまいち分かっていないので、ここには書きませんが、とりあえず [k-kaz が自作テーマで遊ぶサイト](k-kaz-0.netlify.app) では、動作しました。

ただのリンクが並ぶだけで、味も素っ気もないんですが。  
本当はタグやカテゴリのタイトルを出したり、トップページと同じようなレイアウトにしたいんだけど。

また時間のあるときに挑戦しよう。  
あと、このサイトも直さなきゃ。（時間のあるときに）


