---
title: "テーマ Pagermod のカスタマイズ"
date: 2020-10-28T12:32:55+09:00
description: "Pagermod を自分好みにしていくよ。"
categories: ["homepage"]
tags: ["hugo" , "theme" , "pagermod"]
featuredImage: "/images/hugo-custom-theme-pagermod.webp"
featuredImageDescription: "雑誌とコーヒー"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 次に目を付けたテーマは Pagermod です。
不定期でお送りしているテーマのカスタマイズコーナー。  
今回は Pagermod というテーマをカスタマイズしていきます。

[PaperMod | Hugo Themes](https://themes.gohugo.io/hugo-papermod/)

とはいえ、かなりシンプルなテーマであり、選んだ時点である程度、自分好みの状態ですから、あまり記事にはならないかも。  
こちらの記事はカスタマイズを加える度に、ちょこちょこと更新していきます。
## テーマを弄る前に。
テーマファイルを弄る方には当たり前の話かと思いますので、ご存知の方は読み飛ばしてください。  

Hugo のテーマは下記にあります。  

`サイト名\themes\`

Pagermod の場合は `サイト名\themes\Pagermod\` です。

修正するときは、**この中のファイルを直接直しません。**  
修正したいファイルをコピーして、しかるべき場所に置いてから、そちらを修正します。  
その場所が、テーマ読み込み時に優先される仕組みになっているため、**テーマの更新などがあっても影響を受けない** ようになっています。

例えば、`サイト名\themes\hugo-PaperMod\layouts\partials\home_info.html` というファイルを弄りたい場合、`サイト名\layouts\partials\` というフォルダを用意して、そこに `home_info.html` を持ってきて修正するわけです。  
フォルダ構成はまったく一緒です。単純に `\themes\hugo-Pagermod\` が外れるだけだと考えて良いかと思います。

## このテーマで気付いたこと。
### モード選択。
流行りのライトモードとダークモードの切り替え機能が搭載されています。  
ライトモードは背景が白っぽく、文字は黒っぽく。  
ダークモードは背景が黒っぽく、文字は白っぽく。  
スイッチ1つでどうやって切り替えているのかなと思ったら、スタイルシート内に秘密がありました。
#### var()関数。
初めて見た、この関数。  
予め **定義しておいた内容を呼び出す関数** だそうです。  
中身が呼び出すことで変わるので **CSS変数** と呼んだりもするそう。

Pagermod の場合、`theme-vars.css` の中で、ライトモードが `.root` ダークモードが `.dark` で定義されています。
```css
:root {
    --theme: #fff;
    --entry: #fff;
    --primary: rgba(0, 0, 0, 0.88);
    --secondary: rgba(0, 0, 0, 0.56);
    --tertiary: rgba(0, 0, 0, 0.16);
    --content: rgba(0, 0, 0, 0.88);
}
.dark {
    --theme: #1d1e20;
    --entry: #2e2e33;
    --primary: rgba(255, 255, 255, 0.84);
    --secondary: rgba(255, 255, 255, 0.56);
    --tertiary: rgba(255, 255, 255, 0.16);
    --content: rgba(255, 255, 255, 0.64);
}

```
長くなるので途中の行とか割愛しましたが、こんな感じです。

定義したものを使うには、
```css
a {
    color: var(--entry); //リンクの色を定義
}
```
とします。  
すると、ライトモードのときは `#fff` が、ダークモードのときは `#2e2e33` が入るというわけ。
なんだこれ、超便利じゃないか。  
もちろん、色だけではなく、フォントのサイズとかでも同様に使えます。
### フォントサイズについて。
これまで出会ったテーマは、レスポンシブデザインの対応として、フォントサイズを可変にしていました。横幅（width）のサイズから算出するとかね。  

Pagermod では画面サイズに関係無く、**フォントサイズは決め打ち** です。  
つまり、パソコンで見ようが、スマホで見ようが、文字サイズは常に同じ。

これってどうなんでしょうね。  
タイトルタグとか、かなり文字サイズが大きいので、スマホで見ると見やすいんだけど文字数が溢れて改行しちゃうのよね。  
やっぱりここは手を入れて可変にしようかなと悩んでいます。

## それではカスタマイズしていきます。
### アイコンを消したい。
記事内に共有アイコン、トップページにソーシャル系アイコンが搭載されています。  
私は基本それらを使わないので、消し去ります。
#### 記事内の共有アイコンを消す。
こちらは簡単。  
サイト全体の構成を司る `config.yml` or `config.toml` の記述を1箇所変えるだけ。

ちなみにこのテーマの初期値は `config.yml` なので、以下こちらで統一します。

```yml
  ShowShareButtons: false # True にすれば表示するよ。
```
### トップページのソーシャル系アイコンを消す。
これも簡単ですが、ファイルを持ってくる必要があります。

これを → `サイト名\themes\hugo-PaperMod\layouts\partials\home_info.html`  
ここへ → `サイト名\layouts\partials\home_info.html`

#### 修正します。
ソーシャル系アイコンは `social_icons.html` を使って表示していますので、その部分を消します。  
コメントアウトか、削除はお好みで。（私は削除）

元ファイル（`サイト名\themes\hugo-PaperMod\layouts\partials\home_info.html` ）
```html
{{- with $.Site.Params.homeInfoParams }}
<article class="first-entry home-info">
    <header class="entry-header">
        <h2>{{- .Title | markdownify }}</h2>
    </header>
    <section class="entry-content">
        <p>{{- .Content | markdownify }}</p>
    </section>
    <footer class="entry-footer">
        {{- partial "social_icons.html" $.Site.Params.socialIcons -}}
    </footer>
</article>
{{- end}}
```

コピペしたファイル（`サイト名\layouts\partials\home_info.html`）
```html
{{- with $.Site.Params.homeInfoParams }}
<article class="first-entry home-info">
    <header class="entry-header">
        <h2>{{- .Title | markdownify }}</h2>
    </header>
    <section class="entry-content">
        <p>{{- .Content | markdownify }}</p>
    </section>
<!-- ここをコメントアウト or バッサリ削除
    <footer class="entry-footer">
        {{- partial "social_icons.html" $.Site.Params.socialIcons -}}
    </footer>
-->
</article>
{{- end}}
```