---
title: "Hugo Warning Get Page Images"
description: 
date: 2025-05-22T21:12:01+09:00
categories: ["homepage"]
tags: ["Hugo","Theme"]
image: "/images/hugo-warning-sitesocial.webp"
imageDescription: "WARNING"
dropCap: false
displayInMenu: false
displayInList: true
comments: true
draft: false
---
## アップデートしてみたら（v0.145.0 → v0.147.3）

{{% memo 注意 %}}
テーマ PagerModでのお話。  
{{% /memo %}}

しばらく忙しくてアップデートを放置していたんですよね。  
やっと時間が少しできたので、3カ月ぶりのアップデートを行ってみました。  

Warningが1つにErrorが5つ出ました。  
まあ、出るだろうとは思っていたけどね。  

```
WARN  Partial name "partials/templates/_funcs/get-page-images" starting with 'partials/' (as in {{ partial "partials/templates/_funcs/get-page-images"}}) is most likely not what you want. Before 0.146.0 we did a double lookup in this situation.
You can suppress this warning by adding the following to your site configuration:
ignoreLogs = ['warning-partial-superfluous-prefix']

ERROR render of "/archives/2020/11" failed: "hoge\layouts\_default\baseof.html:8:8": execute of template failed: template: list.html:8:8: executing "list.html" at <partial "head.html" .>: error calling partial: "hoge\themes\PaperMod\layouts\partials\head.html:9:15": execute of template failed: template: _partials/templates/twitter_cards.html:9:15: executing "partials/templates/twitter_cards.html" at <partial "partials/templates/_funcs/get-page-images" .>: error calling partial: partial "partials/templates/_funcs/get-page-images" not found

ERROR render of "/archives/2022/06" failed: "hoge\layouts\_default\baseof.html:8:8": execute of template failed: template: list.html:8:8: executing "list.html" at <partial "head.html" .>: error calling partial: "hoge\themes\PaperMod\layouts\partials\head.html:9:15": execute of template failed: template: _partials/templates/twitter_cards.html:9:15: executing "partials/templates/twitter_cards.html" at <partial "partials/templates/_funcs/get-page-images" .>: error calling partial: partial "partials/templates/_funcs/get-page-images" not found

ERROR render of "/archives/2020/10" failed: "hoge\layouts\_default\baseof.html:8:8": execute of template failed: template: list.html:8:8: executing "list.html" at <partial "head.html" .>: error calling partial: "hoge\themes\PaperMod\layouts\partials\head.html:9:15": execute of template failed: template: _partials/templates/twitter_cards.html:9:15: executing "partials/templates/twitter_cards.html" at <partial "partials/templates/_funcs/get-page-images" .>: error calling partial: partial "partials/templates/_funcs/get-page-images" not found

ERROR render of "/404" failed: "hoge\layouts\_default\baseof.html:8:8": execute of template failed: template: 404.html:8:8: executing "404.html" at <partial "head.html" .>: error calling partial: "hoge\themes\PaperMod\layouts\partials\head.html:9:15": execute of template failed: template: _partials/templates/twitter_cards.html:9:15: executing "partials/templates/twitter_cards.html" at <partial "partials/templates/_funcs/get-page-images" .>: error calling partial: partial "partials/templates/_funcs/get-page-images" not found
Built in 3668 ms

Error: error building site: render: failed to render pages: render of "/archives/2021/11" failed: "hoge\layouts\_default\baseof.html:8:8": execute of template failed: template: list.html:8:8: executing "list.html" at <partial "head.html" .>: error calling partial: "hoge\themes\PaperMod\layouts\partials\head.html:9:15": execute of template failed: template: _partials/templates/twitter_cards.html:9:15: executing "partials/templates/twitter_cards.html" at <partial "partials/templates/_funcs/get-page-images" .>: error calling partial: partial "partials/templates/_funcs/get-page-images" not found
```

v0.146.0でテンプレートの仕組みが変わったと更新履歴にあったので、Errorするだろうなと思っていました。

### Hugo リリースノート

:link: [Release v0.146.0 · gohugoio/hugo](https://github.com/gohugoio/hugo/releases/tag/v0.146.0)

> The big new thing in this release is a fully refreshed template system – simpler and much better. We're working on the updated documentation for this, but see this issue for some more information. We have gone to great lengths to make this as backwards compatible as possible, but make sure you test your site before going live with this new version. This version also comes with a full dependency refresh and some useful new template funcs:  
>
> - [templates.Current](https://gohugo.io/functions/templates/current/): Info about the current executing template and its call stack. Very useful for debugging.
> - [time.In](http://time.in/): Returns the given date/time as represented in the specified IANA time zone.
> 
> このリリースの目玉は、テンプレートシステムの全面的な刷新です - よりシンプルで、はるかに優れたものになりました。現在、更新されたドキュメントを作成中ですが、詳細についてはこの課題をご確認ください。可能な限り下位互換性を維持するよう努めましたが、この新バージョンを本番環境に適用する前に、必ずサイトのテストを行ってください。このバージョンでは、依存関係の完全な更新と、いくつかの便利な新しいテンプレート関数も提供されています：
>
> - templates.Current: 現在実行中のテンプレートとそのコールスタックに関する情報。デバッグに非常に便利です。
> - [time.In](http://time.in/): 指定されたIANAタイムゾーンにおける日付/時刻を返します。

## 情報収集

あれこれ直さないといけないのかしら・・・。  

とりあえず情報収集です。  

### Hugo Issues

:link: [no such template "partials/templates/schema_json.html" in PaperMod, but partial appears to exist · Issue #13592 · gohugoio/hugo](https://github.com/gohugoio/hugo/issues/13592)

読んでみると、私が使っているテーマPagerMod特有の現象っぽい。

ということで、PagerModのほうも見てみます。

### PagerMod Issues

:link: [Missing template definitions breaks production builds in Hugo v0.146.0+ · Issue #1719 · adityatelange/hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod/issues/1719)

皆さん四苦八苦しながら、PagerMod側に手を入れる回避方法を構築されています。

ただ、最後にはHugoのv0.146.6で修正されたよーとのこと。

もしかして私のErrorって、ここで書かれていたことと別なのか・・・。

## 解決

Errorの文章に出てくるいくつかのファイルを眺めます。

1個だけあるWarningに`get-page-images`とありまして、これがあるのは`twitter_cards.html`なんですよね。

Errorにもすべてこの`twitter_cards.html`が出てきますので、やっぱりこれが怪しいのかなと、ファイルを開いてみましたが、よくわかりません。

再びPagerModのソース（リリースはされていないけど、日々メンテされているところ）をチェックすると、その`twitter_cards.html`のコミットメッセージに **Implement fixes for refreshed template system for Hugo v0.146.0+** とあるじゃないですか。  

同じ階層にある`schema_json.html`にも同じメッセージが。  

早速、自分のテンプレートを差し替えてみましたら、一発で動きました。  
わかりやすいコミットメッセージありがとうございます。  

### 最新のテンプレート

:link: [hugo-PaperMod/layouts/partials/templates/](https://github.com/adityatelange/hugo-PaperMod/tree/master/layouts/partials/templates)

- :link: [twitter_cards.html](https://github.com/adityatelange/hugo-PaperMod/blob/master/layouts/partials/templates/twitter_cards.html)
- :link: [schema_json.html](https://github.com/adityatelange/hugo-PaperMod/blob/master/layouts/partials/templates/twitter_cards.html)

これをテーマフォルダーじゃなくて、自分のフォルダー `layouts/partials/templates/` に持ってきたらオッケーです。  
