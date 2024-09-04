---
title: "テーマ Cupper のカスタマイズ"
date: 2019-03-12T16:02:29+09:00
description: "テーマ Cupper のカスタマイズをこちらにまとめます。"
categories: ["homepage"]
tags: ["hugo" , "theme" , "cupper"]
image: "/images/cupper-org-logo.png"
imageDescription: "cupper のオリジナルロゴ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Cupper のカスタマイズをこちらにまとめます。
### 全然スタイルが適用されないんですけど（泣）
aether は、簡単にスタイルの変更が出来たんですけど、Cupper は全然適用されない。  
何でだろうと調べていたら、テーマによっては `config.toml` に手を入れなければいけないものがあることを知ったおじさんです。

```toml
[params]
  customCSS = ["/css/hoge.css"]
```

こうすることで、`static/css/hoge.css` が元のスタイルシートの上に被さるようになるんだって。  
もちろん `<head></head>` に読み込み用の追記が必要です。

```html
<head>
    <link rel="stylesheet" type="text/css" href="/css/hoge.css">
</head>
```

スタイルを弄る前段階で躓きましたよ・・・。

## 標準機能内
### ロゴ画像を差し替える
SVG形式のロゴ画像が必要になります。  
私は [Inkscape](https://inkscape.org/ja/) で作りました。

<img src="/images/cupper-custom-logo.svg" alt="cupper のロゴ画像" style="width:auto;">

logo.svg というファイル名で保存して、下記に保存すると適用されます。

`static/images/logo.svg`


## 標準機能外
### h2 に付くリンクアイコンを消す
before: <img src="/images/cupper-custom-h2-icon-before.webp" alt="h2アイコンにリンクが付いちゃう" style="width:auto;" >
after: <img src="/images/cupper-custom-h2-icon-after.webp" alt="h2アイコンにリンクが付いちゃう" style="width:auto;" >

WordPress では、[Font Awesome](https://fontawesome.com)  を使って、スタイルシートで設定していたので、これも同じかと思いきや、完全に外していました（笑）  

画像ファイルで指定していましたので、そのファイルを修正します。

`themes\cupper-hugo-theme\layouts\partials\svg.html`

↓ にコピーします。

`layouts\partials\svg.html`

この中にある `<symbol id="link"> ～ </symbol>` というのが、`h2` のところで使われているリンクアイコンなので、該当行を丸ごと削除すれば、きれいに消えます。

```html
<symbol id="link" viewBox="0 0 50 50">
  <g transform="translate(0 -1002.4)">
   <g transform="matrix(.095670 0 0 .095670 2.3233 1004.9)">
    <g>
     <path style="stroke-width:0;fill:currentColor" d="m452.84 192.9-128.65 128.65c-35.535 35.54-93.108 35.54-128.65 0l-42.881-42.886 42.881-42.876 42.884 42.876c11.845 11.822 31.064 11.846 42.886 0l128.64-128.64c11.816-11.831 11.816-31.066 0-42.9l-42.881-42.881c-11.822-11.814-31.064-11.814-42.887 0l-45.928 45.936c-21.292-12.531-45.491-17.905-69.449-16.291l72.501-72.526c35.535-35.521 93.136-35.521 128.64 0l42.886 42.881c35.535 35.523 35.535 93.141-.001 128.66zm-254.28 168.51-45.903 45.9c-11.845 11.846-31.064 11.817-42.881 0l-42.884-42.881c-11.845-11.821-11.845-31.041 0-42.886l128.65-128.65c11.819-11.814 31.069-11.814 42.884 0l42.886 42.886 42.876-42.886-42.876-42.881c-35.54-35.521-93.113-35.521-128.65 0l-128.65 128.64c-35.538 35.545-35.538 93.146 0 128.65l42.883 42.882c35.51 35.54 93.11 35.54 128.65 0l72.496-72.499c-23.956 1.597-48.092-3.784-69.474-16.283z"/>
    </g>
   </g>
  </g>
</symbol>
```
### カテゴリーを追加する
本テーマでは、タグは自動で出てきますが、カテゴリーは出てきません。  
必要があれば下記の設定をしていきます。

#### 設定ファイルに追加
`config.toml` に `tag` の行があるので、その前後に `category` を入れます。  
右側に入るのは複数形になるので注意！！

```toml
[taxonomies]
  tag = "tags"
  category = "categories"   # ここに追加しました
```

#### メニューに追加
before: <img src="/images/cupper-custom-menu-before.webp" alt="メニューにカテゴリーが無い" style="width:auto;" >

after: <img src="/images/cupper-custom-menu-after.webp" alt="メニューにカテゴリーを追加" style="width:auto;" >


これも設定ファイル `config.toml` の中です。  
`[menu]` というセクションがありますので、表示したい位置に入れてあげます。  

```toml
[menu]
  ＜中略＞
  [[menu.nav]]
    name = "Categories"   # 表示される名称
    url = "/categories/"    # 参照するアドレス
    weight = 3                  # 表示する順番？
  ＜中略＞
```

ついでに about も要らないので消しました。

#### 記事のタイトル下に追加
before: <img src="/images/cupper-custom-post-title-before.webp" alt="記事にカテゴリーが無い" style="width:auto;" >

after: <img src="/images/cupper-custom-post-title-after.webp" alt="記事にカテゴリーを追加" style="width:auto;" >


記事を表示するテンプレートを修正します。

`themes\cupper-hugo-theme\layouts\post\single.html`

↓ にコピーします。

`layouts\post\single.html`

ファイルを開くと、`{{ with .Params.tags }} ～ {{ end }}` という、タグ情報を表示する部分があります。  
丸ごとコピーして、`tags` を `categories` に変更して貼り付ければおしまい。  
スタイルは同じで良いかなと思ったので、手を抜いてそのままにしました。

```html
<div class="date">
  {{ $dateFormat := $.Site.Params.dateFormat | default "2006-01-02" }}
  <strong aria-hidden="true">Post: </strong>{{ .PublishDate.Format $dateFormat }}
</div>

<!-- ここから下を追加 -->
{{ with .Params.categories }}
<div class="tags">
  <strong aria-hidden="true">Categories: </strong>
  <ul aria-label="tags">
    {{ range . }}
      <li>
        {{ $href := print ("categories/" | absLangURL) (. | urlize) "/" }}
        <a href="{{ $href }}">{{ . }}</a>
      </li>
    {{ end }}
  </ul>
</div>
{{ end }}
<!-- ここまで -->
```
### 更新日を追加する
このテーマも更新日がないので追加します。

#### 設定の有効化
`config.toml` に以下を追加します。

```toml
enableGitInfo = true
```

#### 記事一覧に追加
before: <img src="/images/cupper-custom-list-before.webp" alt="記事一覧にカテゴリーが無い" style="width:auto;" >

after: <img src="/images/cupper-custom-list-after.webp" alt="記事一覧にカテゴリーを追加" style="width:auto;" >

記事一覧を表示するテンプレートを修正します。

`themes\cupper-hugo-theme\layouts\_default\list.html`

↓ にコピーします。

`layouts\_default\list.html`

公開日の下に `/ Up: {{ .Lastmod.Format "2006-01-02" }}` を追加します。  
日付のフォーマットも自分好みに変えておきました。

```html
{{ $dateFormat := $.Site.Params.dateFormat | default "2006-01-02" }}
Post: {{ .PublishDate.Format $dateFormat }}
/ Up: {{ .Lastmod.Format "2006-01-02" }}    <!-- ここに追加 -->
```

#### 記事のタイトル下に追加
`layouts\post\single.html` を修正しますが、上とまったく同じなので省略。

### 記事の概要を表示する
日付の下に書いてある文章が概要です。  

after: <img src="/images/cupper-custom-list-after.webp" alt="記事一覧にカテゴリーを追加" style="width:auto;" >

#### 記事に概要を書く（下準備）
記事 `hoge.md` に以下を追加します。

```md
description: "ここに記事の概要を書きます。"
```

テンプレート（例えば `archetypes\post.md` ）に予め `description: ""` を追加しておくと、毎回書かなくて良いので楽です。

#### 記事一覧に概要を追加
`layouts\_default\list.html`

表示したい場所に下記を追加します。

```html
{{ if (isset .Params "description") }}{{ index .Params "description" }}{{ else }}{{ .Summary }}{{ end }}
```

### トップページを記事一覧ページにする［2019-03-19］
テーマ内にある `_index.html` を削除すると、次に見に行くのが `list.html` になるので、それで完了・・・のはずが、うまくいかず。  
最終的には **姑息な手段** で、`layouts\_default\list.html` をコピーして `layouts\_index.html` としました。  
たぶん、どっか見落としているんだろうけど、とりあえず出来た（笑）