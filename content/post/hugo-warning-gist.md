---
title: "Hugo Warning gist"
date: 2025-02-09T21:44:45+09:00
description: "全然気付いていなかった・・・"
categories: ["homepage"]
tags: ["Hugo","Theme"]
image: "/images/hugo-warning-sitesocial.webp"
imageDescription: "WARNING"
dropCap: false
displayInMenu: false
displayInList: true
comments: true
draft: false
---
## 気付くのが遅い

Hugo Serverを立ち上げるときにワーニングが出ていることに気付きました。  
v0.143.0から出ていたようなので、2週間くらい気付いていなかったわ。  

```cmd {name="ワーニング"}
WARN  The "gist" shortcode was deprecated in v0.143.0 and will be removed in a future release.
See https://gohugo.io/shortcodes/gist for instructions to create a replacement.
```

## 内容確認

ワーニングの中にある[リンク](https://gohugo.io/shortcodes/gist/)で内容を確認しました。  

> Hugoであらかじめ用意されていたgist表示のショートコードがv0.143.0から非推奨になりました。  
> いずれ機能が削除されて動かなくなるから、自分のテーマ内に gist.html というファイルを用意してね。  

とのことです。  

## 対応

### gist.htmlを作る

リンク先のソースコードは2行あるんですが、うしろの1行だけ使えば大丈夫です。  
こんな感じ。  

```html {name="layouts/shortcodes/gist.html"}
<script src="https://gist.github.com/{{ index .Params 0 }}/{{ index .Params 1 }}.js{{if len .Params | eq 3 }}?file={{ index .Params 2 }}{{end}}"></script>
```

以上です。  

もともと使っているコードがそのまま利用できるようになっていますので、gistのショートコードがある各ページを触る必要はありません。  

### ちょっとだけ補足

もともとのコード → `｛｛＜ gist user number ＞｝｝`  
これの `user` 部分が `｛｛ index .Params 0 ｝｝` に代入され、`number` 部分が `｛｛ index .Params 1 ｝｝` に代入されますので、結果として正しいURLができあがるという感じです。  
