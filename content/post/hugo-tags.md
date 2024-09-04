---
title: "リスト表示とシングル表示にタグを付ける"
date: 2024-06-10T21:05:42+09:00
description: ""
categories: ["homepage"    ]
tags: ["hugo","theme"]
image: "/images/tag_720.webp"
imageDescription: "タグのイメージ"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 前提

テーマはPagerModです。  
:link: [adityatelange/hugo-PaperMod: A fast, clean, responsive Hugo theme.](https://github.com/adityatelange/hugo-PaperMod/)


## Discussionsを見て

PagerModのDiscussionsを見たら、リスト表示（list.html）にタグを入れたいという投稿がありました。  
シングル記事（single.html）の本文下には元から入っているんですが、それをリストにも出したいと。  

:link: [How to display tags in the post list? · adityatelange/hugo-PaperMod · Discussion #606](https://github.com/adityatelange/hugo-PaperMod/discussions/606)

投稿自体は2021年と古かったのですが、最近になって解決したようでトップに上がっていました。  

## 完成形

実際に行ったサイトはこちら。  
:link: [k-kaz が好きなことを書く](https://k-kaz.net/blog2/)

![リスト表示](/images/hugo-tags-001.webp)
![シングル表示](/images/hugo-tags-002.webp)

シングル表示のほうはそれぞれのタグにリンクが付いていて、同タグ一覧を開くことが可能です。  

## やり方

スレッドに書いてあるとおりなんですけど、  

### 設定されたタグを抜き出す部品（tags.html）を用意する

``` html
{{- $tags := .Params.tags -}}
{{- if $tags -}}
  {{- $lastIndex := sub (len $tags) 1 -}}
  {{- range $index, $tag := $tags -}}
    <a href="/blog2/tags/{{ $tag | urlize }}"> {{ $tag }}</a>
    {{- if ne $index $lastIndex }} · {{ end -}}
  {{- end -}}
{{- end -}}
```

ファイル名はなんでもオッケー。  
urlize でURLを作成します。  
/blog2/ は[うちのサイト用](https://k-kaz.net/blog2/)に入れています。  

### 表示するための部品（post_meta_cattag.html）を用意する

``` html
{{- $scratch := newScratch }}

{{- with (partial "tags.html" .) }}
{{- $scratch.Add "meta" (slice " / ［" . "］")}}
{{- end}}

{{- with ($scratch.Get "meta") }}
{{- delimit . " " | safeHTML -}}
{{- end -}}
```

こちらのファイル名もなんでもオッケー。  
中ではさきほど作成したファイル名 `tags.html` を利用しています。  

### リスト表示（list.html）の画面に表示する

``` html
  {{- if not (.Param "hideMeta") }}
  <footer class="entry-footer">
    {{- partial "post_meta.html" . -}}
    {{- partial "post_meta_cattag.html" . -}}
  </footer>
  {{- end }}
```

一部分だけ抜き出しましたが、`{{- partial "post_meta_cattag.html" . -}}` を追加します。  

### おまけでシングル表示（single.html）の上部にも表示する

``` html
    <div class="post-meta">
      {{- partial "post_meta.html" . -}}
      {{- partial "post_meta_cattag.html" . -}}
      {{- partial "translation_list.html" . -}}
      {{- partial "edit_post.html" . -}}
      {{- partial "post_canonical.html" . -}}
    </div>
```

こちらも一部分だけ抜き出し。  
`{{- partial "post_meta_cattag.html" . -}}` を追加します。  
