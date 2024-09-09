---
title: "HugoでMermaid記法を利用する"
date: 2023-04-17T12:55:42+09:00
description: "少しの設定で利用可能です。"
categories: ["homepage"]
tags: ["Hugo","Theme","mermaid"]
image: "images/mermaid.webp"
imageDescription: ""
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Mermaid記法

テキストだけでフローチャートやガントチャートが描けちゃうという魔法の記法です。  
Markdownの中に書いてあげればオッケーです。  

### 公式サイト

[Mermaid | Diagramming and charting tool](https://mermaid.js.org/)

### 見本

見ていただいたほうが早いと思うので、ご覧ください。  
~~こんな感じのフローチャートがテキストだけで表現できます。~~  
本サイトでは設定を行っていなかったので、見せられなかった（笑）  
こちらで  → [HugoでMermaid記法 | k-kaz が好きなことを書く](https://k-kaz.net/blog2/post/2023/04/13/#%e8%a6%8b%e6%9c%ac)  

あとは公式サイトにいろんなパターンが出ています。  
[About Mermaid | Mermaid](https://mermaid.js.org/intro/)

## Hugoで使う

残念ながらHugoの純正機能だけで、Mermaid記法には対応しません。  
専用のJavaScriptを読み込むことと、テンプレート等の準備で利用可能となります。  

### Hugoのドキュメントと利用手順

ここを見ればオッケー → [Diagrams | Hugo](https://gohugo.io/content-management/diagrams/#mermaid-diagrams)  

1. Mermaid用のテンプレートを作成します。
    - サンプルでは `layouts/_default/_markup/render-codeblock-mermaid.html`
1. 記事を表示させるテンプレート内にコード（※1）を追記します。
    - `layouts/_default/single.html` とかで、`.Content` の呼び出しよりも後に追記します。
1. 記事内でMermaid記法が利用可能になります。

※1 追記するコード  

```Markdown
{{ if .Page.Store.Get "hasMermaid" }}
  <script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.esm.min.mjs';
    mermaid.initialize({ startOnLoad: true });
  </script>
{{ end }}
```

### 使えるようになりました

たったこれだけで使えるようになるんですね。  
拍子抜けするくらい楽でした。  
