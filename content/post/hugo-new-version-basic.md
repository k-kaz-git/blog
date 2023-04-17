---
title: "Hugoの最新版を入れてみよう"
date: 2023-04-17T15:58:50+09:00
description: "基本編"
categories: ["linux"]
tags: ["hugo"]
featuredImage: "images/hugo-logo-wide.svg"
featuredImageDescription: "Hugoのイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 共通

### 使用中のバージョンを確認

```text
hugo version
```

これを実行すると、現在使用しているバージョンが表示されます。  
ちなみに先程実行したところ、  

```text
hugo v0.80.0
```

と出ました。  

### 最新版のダウンロード

Hugoのリリース一覧はこちら。  
[Releases · gohugoio/hugo](https://github.com/gohugoio/hugo/releases)  

1番上に最新版が出ていますので、この中の「Assets」から欲しいものをダウンロードします。  
表示されていない場合は「Show all ...」を選択すると、隠れているものが出てきます。  

## Windows編

### ダウンロードするファイル

私は拡張版を使っているので`hugo_extended_バージョン_windows-amd64.zip`を落とします。  
通常版の場合は、「extended」が付かないものを落としてください。  

### ハッシュ値比較

「Assets」の中に`hugo_0.111.3_checksums.txt`というファイルがあり、この中にハッシュ値が記載されています。  
ダウンロードしたファイルと、上記ファイル内のハッシュ値が同一か確認しましょう。  

### 解凍

zipファイルを解凍すると、3つのファイルが出来上がります。  

1. hugo.exe
1. LICENSE
1. README.md

### インストール

Windowsの場合は、`hugo.exe`を置き換える（上書き）だけで完了します。  
先程解凍した3つのファイルを既存の`hugo.exe`を置いてある場所に上書きで入れて終わりです。

### 確認1 バージョン

```text
hugo version
```

最新版のバージョンが表示されていればオッケーです。  

### 確認2 Hugo Sever

```text
hugo server
```

これを実行して、エラーが出ないことを確認します。  
何かしらのエラーが出たときはネットで調べて対応します。  
意味不明の場合は、いったん元のバージョンに戻す（古いのを上書き）でも良いです。  

## Linux編

また後で書きます。

## トラブルシューティング

### 参考になれば・・・

以前、一気にバージョンを上げたときに問題が出て、四苦八苦した記事がこちら。  
[そうだ Hugo の最新版を使おう！ · k-kaz が Hugo で遊ぶサイト](./hugo-new-version)  

最新版では古い機能が使えなくなっていたので、その対応をしました。  

### 旧バージョンの入手方法

リリースのページに移動します。  
[Releases · gohugoio/hugo](https://github.com/gohugoio/hugo/releases)  

上部メニュー（Code、Issues ...）のすぐ下で、「Releases」が青くなっていると思います。
その横にある「Tags」を選択してください。  

リリース一覧が簡易表示されます。  
希望するリリースの「Downloads」を選択することでゲットできます。  
