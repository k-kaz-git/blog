---
title: "さくらインターネットでHugoを使いたい"
date: 2023-10-27T12:44:27+09:00
description: "苦労したのよ"
categories: ["homepage"]
tags: ["hugo"]
featuredImage: "/images/hugo-sakura.webp"
featuredImageDescription: ""
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## さくらとHugo

ネット上に全然情報が無くて途方に暮れていたとき、神様に出会いました。  

[さくらレンタルサーバーでも自動デプロイがしたい！【GitHubActions】](https://zenn.dev/mamezou/articles/23a221c7287cf6)  

あなたのおかげで救われました。  
本当にありがとうございます。  

### 手順

1. Hugo でコンテンツ作成
1. GitHub へ Push
1. GitHub Actions にて FTP-Deploy-Action を実行 ※さくらサーバーへアップロード

この3番目がポイントですね。  
GitHub Actions を利用することで、さくらサーバーにFTP経由でデータをアップロードしてくれます。  
詳細は上記神様サイトをご覧ください。

[GitHub Actions について - GitHub Docs](https://docs.github.com/ja/actions/learn-github-actions)  

## 実は・・・

10ヶ月ほど前に実現していて、ブログのほうには書いていたんですけど、こちらに書くのを忘れていました・・・。  

噂のブログ → [天才現る](https://k-kaz.net/blog2/post/2022/12/27/)  
