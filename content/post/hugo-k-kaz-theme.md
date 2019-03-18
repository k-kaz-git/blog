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
