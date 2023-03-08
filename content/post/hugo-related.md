---
title: "Hugoで関連記事を表示する"
date: 2023-03-08T12:54:23+09:00
description: "Relatedって機能です"
categories: [
    "homepage"
    ]
tags: [
    "hugo",
    "theme"
    ]
featuredImage: "/images/hugo-related.webp"
featuredImageDescription: "related"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 関連記事を表示する

Hugoには関連記事を表示する機能が備わっています。  

これ → `{{ $related := .Site.RegularPages.Related . | first 5 }}`  

具体的には、こんな感じで行います。  

1. 関連記事を表示するhtmlを用意する
1. 記事ページの所定位置に埋め込む
1. 重み付けをする

### 関連記事を表示するhtmlを用意する

関連記事を表示させるためのhtml（ここではrelated.html）を用意します。  

ファイルの場所  
layouts/partials/related.html  
```html
{{ $related := .Site.RegularPages.Related . | first 5 }}
{{ with $related }}
<h3>See Also</h3>
<ul>
 {{ range . }}
 <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
 {{ end }}
</ul>
{{ end }}
```

1行目が関連記事の機能を使うための魔法。  
たぶん、最初の5つを取得していると思います。  

それ以下は、実際に表示させるためのhtmlです。  
箇条書き（li）の部分は繰り返し処理になっているので、1行で済んでいます。  

ちなみに上のサンプルは、公式サイトから転記です。  
[Related Content | Hugo](https://gohugo.io/content-management/related/#list-related-content)  

### 記事ページの所定位置に埋め込む

表示させたい場所にrelated.htmlを呼び出すコードを追記します。  

ファイル  
layouts/default/single.html  
```html
{{- if (.Param "ShowRelated") }}
{{ partial "related.html" . }}
{{- end }}
```

2行目だけで成立しますが、設定ファイル（config.yml）内でShowRelatedがTrueのときに表示するというようにしておきました。  
```yml
params:
  ShowRelated: true
```

### 重み付けをする

設定ファイル（config.yml）内で、関連度の重み付けをします。  
```yml
related:
  includeNewer: true
  threshold: 80
  toLower: true
  indices:
    - name: keywords
      weight: 100
    - name: tags
      weight: 70
    - name: categories
      weight: 40
    - name: date
      weight: 10
```

数字が重みですね。  
詳細は公式サイトをご覧ください。

[Configure Related Content | Hugo](https://gohugo.io/content-management/related/#default-configuration)  

### サンプル

こちらのサイトで関連記事の表示を行っています。  
[k-kaz が好きなことを書く](https://k-kaz.net/blog2/)  

時間が出来たらここでも行おうかな・・・。  
