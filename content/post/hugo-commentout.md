---
title: "Hugo コメントアウト"
date: 2024-06-26T12:55:18+09:00
description: "表に出したくないものってありますよね"
categories: ["homepage","coding"]
tags: ["hugo","theme"]
image: "/images/hugo-commentout.webp"
imageDescription: "コメントアウト"
dropCap: false
displayInMenu: false
displayInList: true
draft: true
---
## 勘違い

テンプレートファイルはホームページ作成の定番 `html` で作られているので、`<!-- hoge -->` でコメントアウトできると思っていましたが、とんだ勘違いでした。  

テンプレートの一部を削ることになり、でもあとからまた使うかもしれないからコメントアウトしておこうと `<!-- hoge -->` をしましたが、隠れることなくしっかりと表に出てきました。（hogeがコメント部分ね）  

## テンプレートのコメントアウト

公式サイトにちゃんと書いてありました。  
:link: [comments](https://gohugo.io/templates/introduction/#comments)  

`{{/* hoge */}}` これが正式なコメントアウトとなります。  

このやり方で、安心して隠しましょう。  
