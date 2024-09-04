---
title: "Hugo Update V0.122.0"
date: 2024-02-09T21:22:29+09:00
description: "Update後の修正"
categories: ["homepage"]
tags: ["hugo","theme","pagermod"]
image: "images/hugo-logo-wide.svg"
imageDescription: "Hugoのイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo v0.122.0

1年振りくらいにHugo本体のアップデートを実施しました。  
アップデート後、特別な問題は出ていないように思っていましたが、しばらくしておかしなところに気付きました。  

[gohugoio/hugo: The world’s fastest framework for building websites.](https://github.com/gohugoio/hugo)  

## 記事の更新日とかがおかしい

Pagermod というテーマを使っています。  
[adityatelange/hugo-PaperMod: A fast, clean, responsive Hugo theme.](https://github.com/adityatelange/hugo-PaperMod/)  

記事タイトルの下に更新日、読むのにかかる時間等を入れていますが、ここで使っていたHTMLタグ `span` や、スペースを表示する `&nbsp;` なんかがそのままページに表示されていました。  
たぶん、この1年間の間に記述方法が変わったのでしょう。修正しました。  

### post_meta.html

```html
<!-- 元々の内容 -->
{{- if not .Date.IsZero -}}
{{- $scratch.Add "meta" (slice (printf "<span title='%s'>Posted %s / Updated %s</span>" (.Date) (.Date | time.Format (default "2006-01-02" site.Params.DateFormat)) (.Lastmod.Format (default "2006-01-02")))) }}
{{- end }}

{{- with ($scratch.Get "meta") }}
{{- delimit . "&nbsp;·&nbsp;" -}}
{{- end -}}
```

```html
<!-- 修正後の内容 -->
{{- if not .Date.IsZero -}}
{{- $scratch.Add "meta" (slice (printf "Posted %s / Last %s" (.Date | time.Format (default "2006-01-02")) (.Lastmod.Format (default "2006-01-02")))) }}
{{- end }}

{{- with ($scratch.Get "meta") }}
{{- delimit . " " -}}
{{- end -}}
```

これで大丈夫そうです。  
