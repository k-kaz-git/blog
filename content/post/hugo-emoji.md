---
title: "Hugoで絵文字を使えるようにする"
date: 2023-02-09T08:40:05+09:00
description: "絵文字で雰囲気を柔らかく"
categories: [
    "homepage",
    "coding"
    ]
tags: [
    "hugo",
    "theme"
    ]
image: "images/hugo-emoji.webp"
imageDescription: "emoji"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugoの絵文字

テーマの設定を変更することで利用できるようになります。（たぶん）

### Configファイルで設定追加

設定ファイルの `config.toml` に下記を追加します。  

```toml
enableEmoji = "true"
```

これだけです。  

`config.yml` の場合はこちら。  

```yml
enableEmoji: true
```

参考  
[Configure Hugo | Hugo](https://gohugo.io/getting-started/configuration/#enableemoji)  


### 使い方

記事に `：smile：` という感じのコードを入れるだけです。  
展開しないように全角のコロンで書きましたが、実際は半角で書きますよ。  

半角コロンを使って書くと → :smile:  

### チートシート

絵文字コード一覧をありがたく使わせていただきましょう。  

[🎁 Emoji cheat sheet for GitHub, Basecamp, Slack & more](https://www.webfx.com/tools/emoji-cheat-sheet/)  

作成いただいた方、ありがとうございます。  
