---
title: "自作の Hugo用 Shortcodese"
date: 2019-03-14T12:42:21+09:00
description: "自作の Shortcodes をテストしたり、思い出すための場所"
categories: ["homepage"]
tags: ["hugo" , "shortcodes"]
featuredImage: "/images/shortcodes-vegetables-make_640.webp"
featuredImageDescription: "収穫したものをいただきましょう"
dropCap: false
displayInMenu: false
displayInList: true
draft: true
---
## Shortcodes を作りましょう
少し前に勉強しましたので、実際に作ってみましょう。  

[Hugo の Shortcodes を使ってみる。](../hugo-shortcodes/)

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
#### hoge.scss
```css
.memo {
  position: relative;
  margin: 2em 2em;
  padding: 0.5em 1em;
  border: solid 1px #a9a9a9;
  background: #f5f5f5;
  max-width: 70%;

  .memo-title {
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

  p {
    margin: 0;
    padding: 0;
    font-size: 0.8em;
  }
}
```
### VSCode にスニペット登録

{{% gist k-kaz-git fdf4888a89f20ee6238ea56de99e8524 %}}