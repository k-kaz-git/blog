---
title: "コメント欄を削除"
date: 2019-02-18T12:23:37+09:00
description: "使わないので、コメント欄を消しました。"
categories: [ホームページ]
featuredImage: "/images/speech-bubbles_640.webp"
featuredImageDescription: "会話のイメージ"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## コメント欄を削除
使わないので、コメント欄は削除しました。  
テーマ内の single.html にタグにあった `{{ template "_internal/disqus.html" . }}` を、ばっさり削除。

## テーマはコピーしてから修正
念の為、テーマはコピーしてから、そのコピー先を修正しました。
テーマ内の theme.toml を開いて、name を適当なものに変更。
config.toml でテーマを選択しているので、そこのテーマ名も同じものにしました。