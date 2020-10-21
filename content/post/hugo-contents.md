---
title: "コンテンツ（画像とか）を同一フォルダにする場合"
date: 2020-10-21T12:45:27+09:00
description: "それ用のルールがあるのね。"
categories: ["coding"]
tags: ["hugo" , "theme"]
featuredImage: "images/hugo-contents.webp"
featuredImageDescription: "絵を描くよ！"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## コンテンツ（画像とか）を同一フォルダに置きたい。
ここのサイトは昔からの Hugo のシキタリ通り、記事と画像が別フォルダになっています。  

記事：/content/post/hoge.md  
画像：/static/images/hoge.webp

何も考えずに使える反面、記事数や画像数が多くなってくると、収集がつかなくなります。

そんな声が多かったのでしょう。  
Hugo で同一フォルダに記事と画像等のファイルを置ける **ページバンドル** という仕組みができました。

[k-kaz が好きなことを書くサイト](https://k-kaz.netlify.app) では、その仕組みを利用しています。

詳しくはこちらで。
[Page Bundles | Hugo](https://gohugo.io/content-management/page-bundles)


### 画像が表示されないぞ。
今まで全然気付いていなかったというか、そもそも記事の中に画像を置くことが無かったので気付かなかったんですが、今回 **画像を置いてみたら表示されないじゃん** という現象に遭遇しました。


## 若干設定が必要です。
### インデックスファイルを置く。
まず、画像を表示させるには、そのフォルダの中に `index.md` というファイルが必要になります。  
このファイルがあることで、そのフォルダ内にコンテンツが集約されているというような認識をしてくれます。

今までは、`hoge.md` という記事を置いていましたが、こちらを `index.md` という名称に変更します。

これで画像が表示されるようになりました・・・が、**URL がフォルダ名になっちゃう。**

### URL を指定する。
これまでファイル名が URL になってくれていましたので、同じようにしましょう。  
index.md の上部（各種設定内容を記述するところ）に1行追加します。

```toml
url: "hoge"
```

これで意図した通りの URL になりました。

### ついでにもうひと踏ん張り。
この URL を指定する記述を毎回入れるのは面倒ですし、そもそもこちらのサイトでは使わないので忘れそうです。  
ということで、新規に記事を作成するとき、自動で記述されるようにしておきましょう。

私の場合、下記ファイルが新規作成時に使われるものになりますので、開いて追記します。

`\archetypes\post.md`

こんな感じになっています。

```toml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
description: ""
featuredImage: "/images/k-kaz.webp"
featuredImageDescription: "k-kaz のサイトイメージ画像"
url: ""
categories: [""]
tags: [""]
toc: true
draft: true
---
```

これで良しと。