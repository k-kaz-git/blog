---
title: "Hugo で外部サイトを新しいタブで開きたい。"
date: 2020-10-19T12:28:18+09:00
description: "さっと出来ちゃうよ。"
categories: ["coding"]
tags: ["hugo"]
featuredImage: "images/eagle-1753002_1280.webp"
featuredImageDescription: "リンク先へ飛ぶ"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo の外部リンク
Hugo って、html の a タグ（リンクを開くときに使います）をすべて同一タブで開きます。  
ということは、今見ているページが、リンクをクリックした瞬間に、リンク先のページで上書きされるということ。  

自ドメイン内であればそのような挙動で全然良いのですが、外部サイトを開くときは、やっぱり新しいタブで開いてもらいたい。

## Hugo v0.62 以降
直接 html を書いちゃうとか、方法はいくつかあるみたいですけど、現状は下記のやり方を公式で推奨している様子。

### リンクを開くときの挙動を指定
`/layouts/_default/_markup/render-link.html` というファイルを用意。

中身は下記だけあれば良し。
```html
<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank"{{ end }}>{{ .Text }}</a>
```

#### 詳しくは公式サイトにて
[Markdown Render Hooks](https://gohugo.io/getting-started/configuration-markup/#markdown-render-hooks)
