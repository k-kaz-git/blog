---
title: "Hugo のテーマを作ったよ。"
date: 2019-03-18T12:31:49+09:00
description: "Hugo のテーマを自作してみました。途中で放り投げたけど（笑）"
categories: ["homepage"]
tags: ["hugo" , "theme" , "aether" , "cupper"]
featuredImage: "/images/horse-colorful_640.webp"
featuredImageDescription: "カラフルな馬"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo のテーマを自作する
[本家のほうでも書きました](https://k-kaz.net/2019/03/16/あずさ2号と-hugo-の-theme/) けど、Hugo のテーマを弄っていたら、次は自作かなと思って、手を付けてみました。  


**結果として、途中でやめました。**  

だって、今使っているこのテーマ aether に段々と似てきたんですもの。  
スタイルシートをコピペしたりしてたから、当然だけど。

まぁ、このテーマを気に入っているということですよ。


<div class="iframely-embed"><div class="iframely-responsive" style="padding-bottom: 56.25%; padding-top: 120px;"><a href="https://k-kaz.net/2019/03/16/azusa2-hugo-theme/" data-iframely-url="//cdn.iframe.ly/A1E0IvI"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>

{{% memo memo %}}
↑ これ、[iframely](URLhttps://iframely.com) というサービスを使っています。  
最初、情報取得の際にエラーを起こしていたんですが、URL に日本語が含まれていたからでした。
使用される方はご注意あれ。

あと、超でっかく表示されているので、スタイルシートで調整しました。
{{% /memo %}}

### 一から作って、こんな感じのところまで出来ました
素人のわりには、それっぽくなっているでしょ。

#### トップ
![k-kazのテーマ トップページ](/images/k-kaz-theme-top.webp)  
#### 投稿記事
![k-kazのテーマ シングルページ](/images/k-kaz-theme-single.webp)

## 覚書を残す
テーマ作りは途中でやめましたが、やり方をこちらに残しておきましょう。  
またいつか暇な時に続きをしたくなるかもしれないし。

### ローカルサイトと空テーマの準備

```css
$ hugo new site hoge            /* サイト hoge 作成 */
$ cd hoge
$ hugo new theme hoge-theme     /* 空テーマ hoge-theme 作成 */
```

### テーマ内で最低限必要なもの
最低限って書くと、ヘッダーもフッターも要らないけど・・・一応書いておく（笑）  
`line-hight` の関係で、線がブツブツ切れていますが、許してください。  

```html
hoge-theme  
│  theme.toml  
├──layouts  
│   │  
│   ├──partials  
│   │      footer.html  <!-- フッター（ページ最下層）のパーツ -->
│   │      head.html  <!--ヘッダー（ページ最上層）のパーツ -->
│   │  
│   └──_default  
│           baseof.html  <!-- ページの元で各種パーツを読み込む -->
│           list.html  <!-- 記事一覧出力 -->
│           single.html  <!-- 投稿記事出力 -->
```

### 簡単に構成を書く
`baseof.html` が、すべての `html` の親玉で、その中身はこんな感じになっています。  

頭 `head.html`  
体 `list.html` or `single.html`  
足 `footer.html`  

バラバラで作ってあるパーツを、必要なときに、必要な場所で読み込むことで、1枚のページを作り上げています。

`baseof.html` の例

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  {{ block "head" . }}
  {{ end }}
  <title>{{ block "title" . }}
    {{ .Site.Title }}
    {{ end }}</title>
</head>

<body>
  {{ block "main" . }}
  {{ end }}
  {{ block "footer" . }}
  {{ end }}
</body>

</html>
```

各パーツに機能を持たせて、それを呼び出しているため、非常にシンプルな構成となります。

各パーツに書いておくコマンド → `{{ define "hoge" }}...{{ end }}`

呼び出し用コマンド → `{{ block "hoge" . }}{{ end }}`

呼び出されると、各パーツに書かれている `...` の部分がすべて展開される形。  
大体、`{{ block "main" . }}{{ end }}` のところにコンテンツも含まれているので、ここで記事が出力されると思ってください。
普通、`single.html` の中に `{{ .Content }}` と書いてあります。

あとは好きなようにスタイルシートで装飾をしたり、メニューを作ったり。

---
とりあえず、今回はここまで。

## その後 ［2019-03-27］
### どうも気に入らなくて、再度作成中
細々したところがどうも気に入らなくて、もう一度作っています。  
#### トップページ（モバイル系）
縦並びです。  
<img src="/images/k-kaz-theme-top-mobile-01.webp" alt="k-kaz トップページ モバイル" style="width:auto;">
#### トップページ（PC系）
最大で4記事まで横に並び、それを超えたら2行、3行という形で表示します。
<img src="/images/k-kaz-theme-top-pc-01.webp" alt="k-kaz トップページ PC" style="width:auto;">
#### 投稿記事
こんな感じ。
<img src="/images/k-kaz-theme-post-pc-01.webp" alt="k-kaz 投稿記事 モバイル" style="width:auto;">

### 今回気付いたこと
新たにテーマを作り直しながら、気付いたことを書いておきます。

#### netlify で baseof.html を使う場合は注意
こうやって、`{{ define "header" }}` を入れた `header.html` を読み込んだんですけど、

```html
<head>
    {{ block "header" . }}{{ end }}
</head>
```

ローカルサーバーではうまく表示されるのに、**netlify だとうまく表示されない。**  

具体的には、ファイルを読み込めずに `<head>` の中身が空になります。
正確に書くと、netlify で生成した際の Hugo バージョンがメタ情報として載るだけ。

ということで、`header.html` の読み込みは諦め、中身をそのまま `baseof.html` に直書きして対処しました。美しくないけど。

#### フォント調整
いろんなサイトで調べたんですけど、調べれば調べるほど、どツボにハマる感じ。  

今の主流は、レスポンシブデザインを意識して、サイト全体でのフォントサイズを指定するみたい。
それ以下については、基準に対して `rem` で指定することで調整すると。

その中で、一番、理想的なのがこれかなと思いました。
横幅全体を 32 で割ったサイズ。

```css
font-size: calc(100vw / 32);
```

試したところ、モバイル系は良かったんですが、PC の大画面で見ると、フォントが大きくなり過ぎて困る。
rem で調整するのも当時（昨日だけど）は面倒でした。

その後、cupper でも使われていた下記にしました。

```css
font-size: calc(1em + 0.33vw);
```

基本となるサイズ（1em = 16px だっけ？）に対し、画面の大きさにあわせてプラスをしていく方法。
こちらのほうが、個人的には微調整がしやすかったです。（**昨日苦労したのが役に立っただけ疑惑あり** ）

px で直接指定するのが一番楽っていえば楽なんですけど、**レスポンシブデザイン** の呪いが。

#### スタイルシートの書き方
PC用のスタイルを定義して、その後にモバイルの定義かと思っていましたが、逆でありました。
先にモバイルです。今はモバイル優先で何事も考えたほうが良さそうです。

モバイルのところは普通にスタイルシートを書いて、一通り終わったら PC の番です。

```css
/* 画面サイズ 481px 以上はこの中で設定 */
@media only screen and (min-width: 481px) {
  body {}
  h1 {}
  h2 {}
}
```

面倒なので中身は空にしましたけど、モバイルの指定と変えたい部分（特にフォントサイズ）だけをここで定義していきます。
同じようなことをスタイルシートの上部と下部で書いていきます。だらだらと長くなります（笑）

あと、同じ定義をした場合は下のほうが優先されるようです。  
なんで反映されないのかと思ったことが何度もありました（恥）
