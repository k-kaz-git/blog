---
title: "自作の Hugo用 Shortcodes"
date: 2019-03-14T12:42:21+09:00
description: "自作の Shortcodes をテストしたり、思い出すための場所"
categories: ["homepage"]
tags: ["hugo" , "shortcodes"]
featuredImage: "/images/shortcodes-vegetables-make_640.webp"
featuredImageDescription: "収穫したものをいただきましょう"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Shortcodes を作りましょう
少し前に勉強しましたので、実際に作ってみましょう。  

[Hugo の Shortcodes を使ってみる。](../hugo-shortcodes/)

---

{{% memo 注意 %}}
こちらのサイトでは、スタイルシートを `scss` で書いています。  
そこからコピペしてきたものを `css` に手直ししながら掲載しているので、もしかしたら記述ミスがあるかもしれません。その際はご容赦ください。  
また、おかしな部分は、こっそり教えていただけると助かります。
{{% /memo %}}

## メモを挿入する

### 完成形
{{% memo メモ %}}
メモ書きを記事内に挿入するためのショートコードです。  
うまく出来ているでしょうか。
{{% /memo %}}

基本形はいつもの [サルワカさん](https://saruwakakun.com/html-css/reference/css-sample) から頂戴しています。

### 中身

#### 記述例
{{% gist k-kaz-git 954ff77860dcbc29782ebd7ac44f4d16 %}}

{{% memo これを直したい %}}
通常のコード入力だと Shortcodes が展開されちゃうので、毎回 gist に頼っています。それをなんとかしたい今日このごろ。
{{% /memo %}}
#### memo.html

```html
<div class="memo">
    <span class="memo-title">{{.Get 0}}</span>
    <p>{{.Inner}}</p>
</div>
```
#### hoge.css

```css
.memo {
  position: relative;
  margin: 2em 2em;
  padding: 0.5em 1em;
  border: solid 1px #a9a9a9;
  background: #f5f5f5;
  max-width: 70%;
}
.memo .memo-title {
    position: absolute;
    display: inline-block;
    top: -26px;
    left: -1px;
    padding: 0 9px;
    height: 25px;
    line-height: 25px;
    vertical-align: middle;
    font-size: 0.8em;
    background: #a9a9a9;
    color: #ffffff;
    font-weight: bold;
    border-radius: 5px 5px 0 0;
}
.memo p {
    margin: 0;
    padding: 0;
    font-size: 0.8em;
}

```
### VSCode にスニペット登録

{{% gist k-kaz-git fdf4888a89f20ee6238ea56de99e8524 %}}

## 引用を挿入する
### 完成形
{{% blockquote author="Wikipedia" link="https://ja.wikipedia.org/wiki/引用" %}}
引用（いんよう、英語：citation, quotation）とは、広義には、自己のオリジナル作品のなかで他人の著作を副次的に紹介する行為、先人の芸術作品やその要素を副次的に自己の作品に取り入れること。
{{% /blockquote %}}

### 中身
#### 記述例
{{% gist k-kaz-git 9345ab4fab29cd73b3a202cdce34792b %}}

#### blockquote.html

```html
<blockquote>
    <p>{{.Inner}}</p><br>
    <cite><a class="author" href="{{.Get "link"}}">{{.Get "author"}}</a></cite>
</blockquote>
```

#### hoge.css

```css
blockquote {
  font-size: 0.8em;
  padding-bottom: 0;
}
blockquote a.author {
    font-size: 1em;
    line-height: 3em;
    font-style: normal;
    text-decoration: none;
    border-bottom: 1px dashed;
}
blockquote a.author:hover {
      font-weight: bold;
      border-bottom: 2px dashed;
}
```
