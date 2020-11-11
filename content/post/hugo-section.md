---
title: "Hugo Section"
date: 2020-11-11T09:12:59+09:00
description: "いまいち分からんセクションの話"
categories: ["homepage"]
tags: ["hugo" , "theme" , "pagermod"]
featuredImage: "images/divider-800.webp"
featuredImageDescription: "セクション"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## コンテンツが表示されないぞ。
Hugo では、記事を置く場所が `content` の中となっています。  
さらにその下の階層でサブディレクトリを作って、カテゴリ毎に記事を管理することも可能です。

### k-kaz が好きなことを書くサイト の場合。
元々は、`content` 直下の2ファイルと、`post` というサブディレクトリで運用していました。

```bash
content
    │  archives.md
    │  links.md
    │          
    └─post
        │  _index.md
        │  
        ├─2020-10-25
        │      index.md
        │      
        └─2020-10-25-02
                coffee-5498486_300.webp
                coffee-5498486_720.webp
                index.md
```

この場合、トップページにリスト表示されるのは `post` 内の記事のみ。  
`archives.md` `links.md` は上部メニューに割り当ててあります。

今回、新たに `aaaaa` を加え、トップページに `post` と `aaaaa` の下にある記事を表示させようという考えです。


```bash
content
    │  archives.md
    │  links.md
    │          
    ├─aaaaa
    │  │  _index.md
    │  │  
    │  └─2020-11-11
    │          index.md
    │          
    └─post
        │  _index.md
        │  
        ├─2020-10-25
        │      index.md
        │      
        └─2020-10-25-02
                coffee-5498486_300.webp
                coffee-5498486_720.webp
                index.md
```

### post しか表示されんのだけど。
`aaaaa` を配置したのになぜか無視されて、今まで通りリスト表示されるのは `post` 内の記事だけ。  

リスト表示のテンプレートファイル（`list.html`）を開いてみましたら、気になるところが。

```html
{{- if .IsHome }}
{{- $pages = where site.RegularPages "Type" "in" site.Params.mainSections }}
{{- end}}
```

`site.Params.mainSections` これが呼ばれているっぽい。  
ということは、`post` は該当するけど `aaaaa` はこれに該当しないということか。

### 解決した。
メインセクションについて調べていたら、`config.yml` で設定できるみたい。
ということで、下記を追加しました。

```yml
params:
    mainSections: ["post", "aaaaa"]
```

toml の場合は `:` が `=` ですね。

これで狙い通りの表示がされるようになりました。

## _index.md について。
各サブディレクトリ ← これをセクションというんですが、ここに `_index.md` を置くことで、そのセクションに直接アクセスされたときに、セクション一覧が表示されるようにできます。

`https://hoge.com/post/` って指定したときにそれ以下の記事がリスト化する感じ。

まぁ、無くても良いけど、一応置いてあります。



