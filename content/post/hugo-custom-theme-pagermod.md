---
title: "テーマ Pagermod のカスタマイズ"
date: 2020-10-28T12:32:55+09:00
description: "Pagermod を自分好みにしていくよ。"
categories: ["homepage"]
tags: ["Hugo" , "Theme" , "pagermod"]
image: "/images/hugo-custom-theme-pagermod.webp"
imageDescription: "雑誌とコーヒー"
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
フォルダ構成はまったく一緒です。単純に `\themes\hugo-Pagermod\` が外れるだけと考えて良いかと思います。

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
    color: var(--entry);    /* リンクの色を定義 */
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

## Pagermod のカスタマイズをしていきます。
### アイコンを消したい。
記事内に共有アイコン、トップページにソーシャル系アイコンが搭載されています。  
私は基本それらを使わないので、消し去ります。
#### 記事内の共有アイコンを消す。
こちらは簡単。  
サイト全体の構成を司る `config.yml` or `config.toml` の記述を1箇所変えるだけ。

ちなみにこのテーマの初期値は `config.yml` なので、以下こちらで統一します。

```yml
  ShowShareButtons: false     # True にすれば表示するよ。
```
#### トップページのソーシャル系アイコンを消す。
これも簡単ですが、ファイルを持ってくる必要があります。

これを → `サイト名\themes\hugo-PaperMod\layouts\partials\home_info.html`  
ここへ → `サイト名\layouts\partials\home_info.html`

##### 修正します。
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
### 右上のメニューをカスタマイズしたい。
2段階の修正が必要です。  
まずはメニューの文字列を表示する。  
次にその内容を表示する。

#### メニューの文字列を表示する。
`config.yml` を開いて、表示したい文字列、優先順位等を記述します。  

```yml
menu:
  main:
    - name: Archives    # 表示される文字列
      url: /archives/   # url
      weight: 5         # 表示順（小さいほど先に表示）
    - name: Tags
      url: /tags/
      weight: 10
    - name: Link        # 今回追加
      url: /links/      # 今回追加
      weight: 12        # 今回追加
```

今回は `Link` というメニューを追加してみました。

#### Link の中身を作ります。
下記の場所に `links.md` を設置します。  
`サイト名\content\links.md`
```bash
$ hugo new links.md
```
ファイルを作ったら、その中身を編集します。
```md
---
title: "Link"
url: "/links"
summary: "links"
---
[k-kaz ブログ](https://k-kaz.net)  
[k-kaz が Hugo で遊ぶサイト](https://k-kaz-git.github.io)
```

これで画面右上に `Archives` `Tags` `Link` が並び、クリックするとその中身が表示されます。

### トップページに記事のサマリーを表示する。
何も設定しない場合は、記事本文の先頭から何文字かがサマリーとして表示されます。
意図的に表示したい文言があるときは記事上部の設定部分に `summary` を入れます。
```md
summary: "意図的に表示したい文言を書く。"
```

## Hugo のカスタマイズをします。（他テーマでも共通）
この記事に載せなくても良いのですが、せっかくなので一緒に書いておきましょう。

### 外部サイトを新しいタブで開く
[Hugo で外部サイトを新しいタブで開きたい。](https://k-kaz-git.github.io/post/hugo-alink/) を参照あれ。

### 記事内の html を表示する。
Hugo v0.60.0 からの仕様で、記事に直接書いた html が表示されなくなった問題への対応です。  
ちなみに表示されない部分は `<!-- raw HTML omitted -->` と置き換わっています。
#### config.toml
```toml
[markup.goldmark.renderer]
unsafe= true
```
#### config.yml
```yml
markup:
  goldmark:
    renderer:
      unsafe: true
```
参考 → [Link WordsConfigure Markup | Hugo](https://gohugo.io/getting-started/configuration-markup/)  
参考 → [あれ、引用がごっそり消えている](https://k-kaz-git.github.io/post/hugo-new-version/#%E3%81%82%E3%82%8C%E5%BC%95%E7%94%A8%E3%81%8C%E3%81%94%E3%81%A3%E3%81%9D%E3%82%8A%E6%B6%88%E3%81%88%E3%81%A6%E3%81%84%E3%82%8B)

### 日本語化
i18n 対応ですので、言語ファイルが用意されています。  
いつものごとく、テーマ内のファイルは直接触らず、コピペします。

`/home/k-kaz/data/git/k/themes/hugo-PaperMod/i18n` → `/home/k-kaz/data/git/k/i18n`

今回、日本語用のファイルを直したかったので、`ja.yaml` というものをコピペしました。

元のファイル（こんだけしか無い)
```yml
- id: prev_page
  translation: "前のページ"

- id: next_page
  translation: "次のページ"
```

こう直しました。
```yml
- id: prev_page
  translation: "前のページ"

- id: next_page
  translation: "次のページ"

- id: read_time               # ここから下を追記しました。
  translation:
    one : "1 分"
    other: "約 {{ .Count }} 分"

- id: toc
  translation: "目次"

- id: translations
  translation: "Translations"
```
参考にしたのは、`en.yaml` のファイルです。

実際に言語切り替えするときは、[exampleSite](https://adityatelange.github.io/hugo-PaperMod/) のようにサイト上にスイッチを付けるのが良いのでしょうが、**うちのサイトに日本語を読めない人は来ない** という決め付けで、横着して `config.yml` で日本語を直指定しました。

```yml
defaultContentLanguage : ja   # この行を追加です。
```

### 投稿日と更新日を表示したい。
初期状態だと投稿日のみが表示されています。  
また、並びが「月、日、年」と、日本人の私には馴染みのないものなので一緒に直しましょう。

#### テンプレートファイルをコピペします。
いつものようにテンプレート内から該当ファイルをコピペします。

`サイト名\themes\hugo-PaperMod\layouts\partials\post_meta.html` → `"サイト名\layouts\partials\post_meta.html`

元のファイル（該当部分だけ抜き出し）
```html
{{- if not .Date.IsZero -}}
{{- $scratch.Add "meta" (slice (.Date.Format (default "January 2, 2006" .Site.Params.DateFormat))) }}
{{- end -}}
```

修正後のファイル（2行目を修正しています）
```html
{{- if not .Date.IsZero -}}
{{- $scratch.Add "meta" (slice "Posted" (.Date.Format (default "2006-01-02" .Site.Params.DateFormat)) "Updated" (.Lastmod.Format (default "2006-01-02"))) }}
{{- end -}}
```

2行目のところで、日付の表示方法 `default "2006-01-02"` を決めてあげ、さらに最終更新日を取得する `.Lastmod.Format` を追加しました。

日時の前に書く言葉はお好みでどうぞ。  
ここでは `Posted` と `Updated` にしてあります。  
もちろん日本語でも良いですよ。

ただ、これだけだと、更新日時は正しく反映されません。  
もう1つ修正が必要です。

#### config.yml も直します。
最終更新日時を Git の履歴から自動取得するには、`config.yml` に、1行追記が必要です。

```yml
enableGitInfo: true
```

これで更新日時が正しく反映されるようになります。

### 現在の PageSpeed Insights はこんな感じ。
![PageSpeed Insights の結果](/images/pagermod-pagespeed-insights.webp)

**100点は気持ち良いね！！**

現状、99点～100点のところで、ほぼ安定しています。  
100点で安定させるには、記事のカバー画像をやめることかな。（データ量を減らす）  

でも、トップページが寂しくなっちゃうので、カバー画像ありで運用します。  
90点台後半が出ていれば十分でしょうしね。
