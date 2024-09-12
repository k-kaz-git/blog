---
title: "Hugoのテーマ Stack について"
description: 触り始めたのでまとめていく
date: 2024-09-05T12:48:18+09:00
categories: ["homepage"]
tags: ["Hugo","Theme"]
image: /images/hugo-theme-stack.avif
imageDescription: かわうそ
math: 
license: 
hidden: false
comments: true
draft: false
---
## はじめに

当サイトで新しいテーマを採用しました。  
時間が経つときれいに忘れ去るため、修正した内容を自分用にメモしておこうと思います。  

正しい内容かどうか定かではないため、ご覧になる方はご注意ください。  

### 採用したテーマ

:link: [Stack | Hugo Themes](https://themes.gohugo.io/themes/hugo-theme-stack/)  
:link: [CaiJimmy/hugo-theme-stack: Card-style Hugo theme designed for bloggers](https://github.com/CaiJimmy/hugo-theme-stack)

### 追記 2024-09-06

初期設定で行った内容を書き終わったタイミングで、同じ趣旨の内容を書かれているサイトを発見。  
めっちゃ真似したみたいになってしまった。ごめんなさい・・・。  

:link: [Hugo Stackテーマのカスタマイズ](https://notes.nakurei.com/post/customize-stack-theme/)

↑ まずはこちらのサイトをご覧いただくほうがよろしいかと。  

良いもの見付けた！  
と思って、コードブロックにファイル名を表示させる方法、参考にさせていただきました 🙇‍♂️  

### カスタマイズの基本

テーマフォルダー内でファイルの変更を行うと、テーマの更新があったときに上書きされてしまうため、変更したいファイルを専用の場所にコピーして編集します。  

Hugoが動作するとき、テーマフォルダーの中身を読み込んで表示をしてくれますが、専用の場所に置いたファイルが優先される仕様になっています。  

#### サイト全体の基本設定

ルートフォルダーにある `config.yaml` を触ります。  
テーマフォルダーのルートや、exampleSiteにあるものを持ってきて修正するのが楽です。  
今回はexampleSiteにある `hugo.yaml` を使いました。  
ファイル名は「config」じゃなくて「hugo」でも良いみたいですね。

尚、ファイル形式は、yaml、toml、jsonが使えるため、お好みのもので。  

#### レイアウトやスタイルの設定

`/assets` ← スタイルシートの保存先  
`/layouts` ← レイアウト用ファイルの保存先  

こちらもテーマフォルダーから変更したいものを持ってきて修正します。  

## 全体設定 config.yaml

全部書くと大変なので、引っかかりやすいものとか。  
ここに未掲載のもので重要なのは、サイトのURLとか言語設定とか。  

### Google Analytics

Analyticsのコードを貼り付けます。  

```yaml {name="config.yaml"}
services:
    # GA Tracking ID
    googleAnalytics:
        id: hoge-ID     # 自身のタグ
```

#### Google Analytics 後日変更

Googleタグマネージャーを使っているのでそちらのIDを貼り付けていたんですが、動作しているのかしていないのか微妙な感じ。  
Googleタグマネージャーのプレビュー画面でも黄色マーク（要注意）が出ているので、ここを使うのはやめました。  
そもそもAnalytics用のIDっぽいしね。  

`config.yaml`は空にして、`/layouts/partials/head/custom.html`と`/layouts/_default/baseof.html`にタグマネージャーで指示されたものを貼り付けました。  

```yaml {name="config.yaml"}
services:
    # GA Tracking ID
    googleAnalytics:
        id:          # 空にしました
```

```html {name="/layouts/partials/head/custom.html"}
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','hoge-ID');</script> <!-- hoge-IDは自分のもので -->
<!-- End Google Tag Manager -->
```

```html {name="/layouts/_default/baseof.html"}
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}" dir="{{ default `ltr` .Language.LanguageDirection }}">
    <head>
        {{- partial "head/head.html" . -}}
        {{- block "head" . -}}{{ end }}
    </head>
    <body class="{{ block `body-class` . }}{{ end }}">
    <!-- Google Tag Manager (noscript) -->
    <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=hoge-ID"
    height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript> <!-- hoge-IDは自分のもので -->
    <!-- End Google Tag Manager (noscript) -->
        {{- partial "head/colorScheme" . -}}
```

### パーマリンク

初期値のままですが、あとで変えるかも。  

```yaml {name="config.yaml"}
permalinks:
    post: /p/:slug/     # 投稿した記事のURL（/p/タイトル）
    page: /:slug/       # ページ（Aboutとか）のURL
```

### ページタイトル

`languages:` セクションの中には、英語、中国語、アラビア語？があり、訪問者が切り替えできるようになっていました。  
このサイトは日本人向けなので、元の言語はすべて削除して、日本語を追加しています。  

```yaml {name="config.yaml"}
languages:  # 切り替えないのでセクション不要と思いつつ残している。
    ja:
        languageName: 日本語
        title: k-kaz が Hugo で遊ぶサイト   # これだけあれば問題ないはず。
        weight: 1                           # 複数言語を用意したときに有効。
        params:
            description: 
```

### 1ページに表示する記事数を設定

元々1行で `paginate: 3` となっていました。  
動作はしますが最新のHugoでは非推奨のようで、Warningが出ていました。  
下記のように変更しています。  

```yaml {name="config.yaml"}
pagination:
  disableAliases: false
  pagerSize: 3              # 3つ表示させる
  path: page
```

### 日付のスタイル

`Params:` の中で設定。  

```yaml {name="config.yaml"}
Params:
    dateFormat:
        published: 2006/01/02               # 投稿日、年月日がわかりやすいので。
        lastUpdated: 2006/01/02/15:04 JST   # 更新日、同上。
```

### コメント機能

`Params:` の中で設定。  
機能が用意されているので楽ちんです。  
私はgiscusを利用しました。 > :link: [Hugoにコメント機能を付ける（giscus編）](/p/hugoにコメント機能を付けるgiscus編)

```yaml {name="config.yaml"}
params:
    comments:
        enabled: true     // コメント機能を有効に
        provider: giscus  // どのコメント機能を使うか選択
        giscus:           // providerで選択する機能名
            repo: hoge/hoge-repo                // ユーザー名と保管用リポジトリ名
            repoID: hoge-repo-ID                // 自動発行されるリポジトリのID
            category: General                   // Discussions内でどのカテゴリに保管するか
            categoryID: hoge-cat-ID             // カテゴリのID
            mapping: title                      // 保管時の表題となるもの（タイトル、URL等）
            lightTheme: preferred_color_scheme  // ライトモードのときのテーマカラー（ここではモードで自動調整？）
            darkTheme: preferred_color_scheme   // ダークモードのときのテーマカラー（ここではモードで自動調整？）
            lang: ja                            // 項目がなかったので自分で追加し、メッセージ等を日本語に
            reactionsEnabled: 1
            emitMetadata: 0
```

## ウィジェット（ページ）

検索とかアーカイブとかの機能です。  

|オリジナル|複写先|
|---|---|
|/themes/stack/exampleSite/content/page|/content/page|

`config.yaml` の中で設定がありますが、そこで有効にしてもページがないと画面に出てきません。  
/themes/stack/exampleSite/content/page 内にあるものを、/content/pageへコピーすることで画面に表示できます。  
最初、機能を有効にしても出ない理由が不明で途方に暮れました。  

## スタイルシート

スタイルシートは各ファイルを持ってくるのではなく、カスタム用のファイルにまとめて記述するようです。  
1ファイルにまとまっているとあとで直したところを確認したくなったときに楽ですね。  

|オリジナル|複写先|
|---|---|
|/themes/stack/assets/scss/custom.scss|/assets/scss/custom.scss|

`custom.scss`にはこのコメントのみ記述されていました。  

```css {name="custom.scss"}
/* Place your custom SCSS in HUGO_SITE_FOLDER/assets/scss/custom.scss */

```

**スタイルシートの修正後、Hugo Serverを再起動しないと反映されないことがある**ので注意です。  
ページの再読み込みでいけるものもありますが、違いがよくわかっておらず。  

これでちょっとハマったので、意図した通りに変更されない場合は再起動を試してみましょう。  

### フォントを変える

中国製のためか、フォントに違和感があるので変えました。  

/themes/stack/assets/scss/variables.scss の該当部分を`custom.scss`にコピペして編集します。  
4行目にある `--base-font-family` の行を変更しています。  


```scss {name="/assets/scss/custom.scss"}
:root {
    --sys-font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Droid Sans", "Helvetica Neue";
    --zh-font-family: "PingFang SC", "Hiragino Sans GB", "Droid Sans Fallback", "Microsoft YaHei";
    --base-font-family: -apple-system,BlinkMacSystemFont,segoe ui,Roboto,Oxygen,Ubuntu,Cantarell,open sans,helvetica neue,sans-serif, var(--sys-font-family), var(--zh-font-family), sans-serif;
    --code-font-family: Menlo, Monaco, Consolas, "Courier New", var(--zh-font-family), monospace;
}
```

## レイアウト

未着手。  

## 不具合修正

### resources.ToCSS はもう古い

Hugo Serverを起動すると下記のWarningが出ます。  

***WARN  deprecated: resources.ToCSS was deprecated in Hugo v0.128.0 and will be removed in a future release. Use css.Sass instead.***  

`resources.ToCSS` じゃなくて、 `css.Sass` を使えと言っています。  
SassをCSSにするための機能らしい。  

- [ToCSS | Hugo](https://gohugo.io/hugo-pipes/transpile-sass-to-css/) ← 古い
- [css.Sass | Hugo](https://gohugo.io/functions/css/sass/) ← 新しい

フォルダー内を検索して2つのファイル `style.html`、`disqusjs.html` で該当箇所を発見しましたので修正。  

#### style.html

|オリジナル|複写先|
|---|---|
|/themes/stack/layouts/partials/head/style.html|/layouts/partials/head|

```html {name="/themes/stack/layouts/partials/head/style.html" linenos=true}
{{ $sass := resources.Get "scss/style.scss" }}
{{ $style := $sass | resources.ToCSS | minify | resources.Fingerprint "sha256" }}
<link rel="stylesheet" href="{{ $style.RelPermalink }}">
```

2行目の `resources.ToCSS` を `css.Sass` に修正しました。  

```html {name="/layouts/partials/head/style.html" linenos=true}
{{ $sass := resources.Get "scss/style.scss" }}
{{ $style := $sass | css.Sass | minify | resources.Fingerprint "sha256" }}
<link rel="stylesheet" href="{{ $style.RelPermalink }}">
```

#### disqusjs.html

|オリジナル|複写先|
|---|---|
|/themes/stack/layouts/partials/comments/provider/disqusjs.html|/layouts/partials/comments/provider|

```html {name="/themes/stack/layouts/partials/comments/provider/disqusjs.html" linenos=true}
{{- $pc := .Site.Config.Privacy.Disqus -}}
{{- $disqusjs := .Site.Params.Comments.disqusjs -}}
{{- if and (not $pc.Disable) (and $disqusjs.Shortname $disqusjs.ApiKey) -}}

{{- $style := resources.Get "scss/partials/comments/disqusjs.scss" | resources.ToCSS | minify -}}
```

5行目の `resources.ToCSS` を `css.Sass` に修正しました。  

```html {name="/layouts/partials/comments/provider/disqusjs.html" linenos=true}
{{- $pc := .Site.Config.Privacy.Disqus -}}
{{- $disqusjs := .Site.Params.Comments.disqusjs -}}
{{- if and (not $pc.Disable) (and $disqusjs.Shortname $disqusjs.ApiKey) -}}

{{- $style := resources.Get "scss/partials/comments/disqusjs.scss" | css.Scss | minify -}}
```

### paginate はもう古い

Hugo Serverを起動すると下記のWarningが出ます。  

***WARN  deprecated: site config key paginate was deprecated in Hugo v0.128.0 and will be removed in a future release. Use pagination.pagerSize instead.***  

1画面に表示する記事の数を指定する機能です。  
`paginate` は古いので `pagination.pagerSize` に変えなさいというものです。  

`config.yamal` の中で設定します。  
元は `paginate: 3` の1行ですが、下記のようにします。  

```yaml {name=config.yaml}
pagination:
  disableAliases: false
  pagerSize: 3
  path: page
```
