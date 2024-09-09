---
title: "そうだ Hugo の最新版を使おう！"
date: 2020-10-18T20:00:29+09:00
description: "そんなときは大体ハマる。"
categories: ["coding"]
tags: ["Hugo"]
image: "/images/man-1394395_1920.webp"
imageDescription: "孤独な考える人"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo ってバージョンアップしてるの？
まずは、私が使っている現在のバージョンを確認。

```html
$ hugo version
$ Hugo Static Site Generator v0.54.0/extended windows/amd64 BuildDate: unknown
```

ふむ、0.54 を使っているらしい。

公式サイトに行ってみると、0.76.5 が最新の様子。  
**すっかり時代に取り残されていました。**

## 最新版にするには（Windows）
Windows の場合は、GitHub から Zip を落としてきて、実行ファイルを入れ替えるだけで完了です。  

## チェックしましょう。
とりあえず今までの記事が正常に見られるかチェック。

```html
$ hugo server -D
Start building sites …
WARN 2020/10/16 15:08:48 Page.RSSLink is deprecated and will be removed in a future release. Use the Output Format's link, e.g. something like:
    {{ with .OutputFormats.Get "RSS" }}{{ .RelPermalink }}{{ end }}
```

なんか怒られた。

## 修正します。
調べてみたら、`.RSSLink` っていうのが最新版では使えないっていうことらしい。

`head.html` で使っていたので修正しました。

```html
{{ if .RSSLink }}
<link href="{{ .RSSLink }}" rel="alternate" type="application/rss+xml" title="{{ .Title }}" />{{ end }}
```
↓ こんな風に。
```html
{{ with .OutputFormats.Get "RSS" }}
<link href="{{ .RelPermalink }}" rel="alternate" type="application/rss+xml" title="{{ $.Site.Title }}" />{{ end }}
```

これでエラーは出なくなった。

## あれ？ レイアウトが・・・
トップページのレイアウトがおかしい。  
今までは `content/post/` 以下の記事がずらっと並んでくれたんですが、`content` が表示されてるっぽい。

![トップページのレイアウトがおかしいのイメージ画像](/images/hugo-newversion-01.webp)

画面に出ている `post` をクリックして開くといつもの感じになるけど、仕様が変わったのかな・・・。

## 調査開始
### まずは定番
Google でいろいろ調べました。

**まったく分からんかったけどね。**

Permalink の設定でなんとかならんもんかと四苦八苦するものの駄目でした。

### 地道に仕様変更を探る
どのバージョンからレイアウトが崩れているのか、GitHub から過去バージョンを落としまくって調査しました。

v0.60.0 NG  
v0.57.0 NG  ← **お前か！**  
v0.56.0 OK  
v0.56.3 OK  

リリース情報の中身を念入りチェックしていたら、気になる一文を発見。

{{% blockquote author="GitHub" link="https://github.com/gohugoio/hugo/releases/tag/v0.57.0" %}}
home.Pages now only returns pages in the top level section. Before this release, it included all regular pages in the site. This made it easy to list all the pages on home page, but it also meant that you needed to take special care if you wanted to navigate the page tree from top to bottom. If you need all regular pages, use .Site.RegularPages. 
{{% /blockquote %}}


英語読めないので、翻訳しました。

{{% blockquote author="GitHub" link="https://github.com/gohugoio/hugo/releases/tag/v0.57.0" %}}
home.Pagesはトップレベルセクションのページのみを返すようになりました。
このリリースの前は、サイト内のすべての通常のページが含まれていました。
これにより、ホームページ上のすべてのページを簡単にリストアップすることができましたが、
ページツリーを上から下に移動したい場合には、特別な注意が必要なことも意味していました。
すべてのレギュラーページが必要な場合は、.Site.RegularPagesを使用してください。
{{% /blockquote %}}

これだ。

`\themes\xxxx\layouts\_default\list.html` の中に `.Pages` を使っていたので、そこを `.Site.RegularPages` に直すことで解決しました。

半日近くかかったよ。

### あれ、引用がごっそり消えている
たまたま気付いたんですが、引用した部分がごっそり消えていたんですよ。  

html のソースを見ると、その部分が
```html
<!-- raw HTML omitted -->
```
となっていました。

これは Google ですぐに見付けました。

v0.60.0 から、記事ファイル（hoge.md）に書かれた html が表示されなくなったとのこと。（なんで？）

解決策は `config.toml` にオプションを追加すればオッケー。

```toml
[markup.goldmark.renderer]
unsafe= true
```

すぐ直って良かった。

ということで、今回から最新版の Hugo を使っていきます。
