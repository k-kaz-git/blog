---
title: "テーマ aether のカスタマイズ"
date: 2019-02-18T21:30:02+09:00
description: "テーマ aether のカスタマイズをこちらにまとめます。"
categories: ["homepage"]
tags: ["hugo","theme","aether"]
featuredImage: "/images/pair-love_640.webp"
featuredImageDescription: "カップル"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## aether のカスタマイズをこちらにまとめます。
---
~~念の為、テーマを丸ごとコピーして、そちらを弄ることにします。~~
~~その際、以下の修正が必要です。~~

~~`/サイト名/themes/新しいテーマ名/theme.toml`~~  
~~`name = "新しいテーマ名"`~~ 

~~`/サイト名/config.toml`~~  
~~`theme = "新しいテーマ名"`~~

### 追記：注意事項 ［2019/02/20 12:50］
素人が遊びながら行っているカスタマイズです。  
違っている部分など **多々** あるかもしれません。  
そのあたりは、十分にお気を付けください。

### 追記：今更ですが・・・［2019/02/20 12:45］
わざわざテーマ内を弄らずに、root に用意されているフォルダ（static とか layout とか）にファイルを置いて弄れば良いような気がしてきた。  
それが可能なら、テーマの丸ごとコピーも要らないしね。  
なんか、すごく無駄なことをしていたかもしれない。  

ということで、このページも修正しました。

## 標準機能内

### ホームボタンの設定
<img src="/images/aether-custom-home-btn_400.webp" alt="ホームボタンの設置" style="max-width:400px;">

個別記事を開いた時に、一番下に真っ白なスペースがあります。  
ここは、ホーム（トップページ）に戻るためのボタンになっており、Home という白文字が入っています。**実際は白同士で見えないんですけど。**

`homeimg` に少し暗めの画像を指定すると、文字がハッキリして見やすいと思います。

`/サイト名/config.toml`
<script src="https://gist.github.com/k-kaz-git/2df71ae7a4d133f118cf424e6d48476b.js"></script>

### イメージデータの入れ方
テーマの `readme` にも書いてありますが、普通のコマンドで挿入すると、エリア内いっぱいに広がります。

普通の入れ方
```md
![イメージタイトル](/images/hoge.webp)
```

<script src="https://gist.github.com/k-kaz-git/d27e935f5404952ae5419250516ebe1e.js"></script>

### Google Analytics
テーマが元々対応してくれているので、設定ファイルにトラッキングコードを記入するだけで、各ページに展開してくれます。  

`/サイト名/config.toml`  
```toml
googleAnalytics = "トラッキングコード"
```

`<head></head>` の間に長いタグを入れる必要もありません。楽ちんだ。

## 標準機能外

### コメント用の外部サービスを外す
テーマには、個別記事に disqus のサービスが組み込まれていますが、これも使いませんので外します。  
下記フォルダが無い場合は作成し、該当ファイルをテーマから持ってきましょう。

`/サイト名/layouts/_default/single.html`
<script src="https://gist.github.com/k-kaz-git/6fbc59bd45e57c5086e3d4e7a87ac63b.js"></script>

### テンプレートに外部リンクを仕込む
<img src="/images/aether-custom-link-btn_175.webp" alt="外部リンクボタンの設置" style="width:175px;">

表示させる場所によって、編集するファイルは変わりますが、今回は最下部（フッター）に入れることにしました。普通に HTML を修正します。  
footer.html は、`<footer></footer>` と、空っぽなので、その間にタグを入れます。

`/サイト名/layouts/partials/footer.html`
<script src="https://gist.github.com/k-kaz-git/2dd802712a513a5017593a9d77536aa3.js"></script>


### スタイルシートを適用する
テーマ内のスタイルシートではなく、別途カスタマイズ用のファイルを用意します。  
また、そのファイルを読み込むよう、HTML の修正をします。

#### カスタマイズファイルを用意
`/サイト名/static/css/hoge.css`  
css というフォルダも自分で作成し、その中にスタイルシート用のファイルを作ります。

#### スタイルシートの読み込み
`/サイト名/layouts/partials/head.html`  
スタイルシートの読み込み行が続いているので、一番下に `/css/hoge.css` を追加します。  
<script src="https://gist.github.com/k-kaz-git/be3470f0ad5ab3538adcc7df1bb6182e.js"></script>


### 目次を追加する
手動で目次を作ることもできますが、後から構成が変更になると、追加修正が大変なことになりますので、自動的に目次を生成するようにします。  
これは、Hugo に元々組み込まれている関数 `{{.TableOfContents}}` を使用することで実現可能です。

公式ドキュメントでは、**本文が 400文字以上の記事のみ目次を付ける** というものが紹介されていました。

<script src="https://gist.github.com/k-kaz-git/078e749b2dcf0bf59192c871049ccc98.js"></script>


しかし、**私の英語力と、Hugo知識ではまだ乗り越えられない壁** のようです。

結局、一番簡単な、下記方法で妥協しております。

`/サイト名/layouts/_default/single.html`

<script src="https://gist.github.com/k-kaz-git/cd2030eb11278acb7172621f18a3e7f8.js"></script>

こうすることで、＜目次＞という文字列の下に、自動で目次を生成してくれます。  
条件としては、`H2` 以下の `Hタグ` が目次としての構成要素になるようです。

但し、このやり方だと、400文字以上とか無視して、**どのページにも出現します** ので注意。

### 更新日を追加する
こちらのサイトは、日記的な [姉妹サイト（本家）](https://k-kaz.net "姉妹サイト（本家）") とは違って、同じ記事を後から何度も加筆修正します。  
そうなると、投稿日だけではなく、最終更新日もあると嬉しい。

さぁ、やってみましょう。

#### 更新日を追加する（個別記事）
<img src="/images/aether-custom-update_284.webp" style="width:284px;" alt="更新日を追加する">  

Hugo に用意されている関数 `.Lastmod` を利用します。  
下準備として、`config.toml` に関数を有効にするための記述を追加します。

`/サイト名/config.toml`  
`enableGitInfo = true` ← これで有効化されます。

続いて、更新日を出力したい場所に `.Lastmod` を入れます。

`/サイト名/layouts/_default/single.html`

`<header>` だけ抜き出しました。  
上が元のコードで、下が修正したコードです。

```html
[single.html 修正前]

<header class="post-header">
   	<h1 class="post-title">{{ .Title }}</h1>
    <p class="post-date">Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "Jan 2, 2006" }}</time></p>
</header>
```

```html
[single.html 修正後]

<header class="post-header">
   	<h1 class="post-title">{{ .Title }}</h1>
    <p class="post-date">
        Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "2006-01-02" }}</time>
         / Update {{ .Lastmod.Format "2006-01-02" }}</p>
</header>
```
`.Lastmod` だけだと、時間まで表示されて大げさなので、`.Format "2006-01-02"` で書式を設定しました。
ついでに初回投稿のほうも、同じ書式に変更しています。
#### 更新日を追加する（トップページ）［2019-02-25］
<img src="/images/aether-custom-card-update_640.webp" alt="トップにも更新日を入れる" style="max-width:640px;">

やり方は個別記事と同じで、編集するファイルが変わるだけです。

`/サイト名/layouts/_default/li.html`  
※theme の中にあるのでコピーして持ってきます。

ついでに、ポストされた日のフォーマットも合わせておきます。  
`<div>` のすぐ下にある `<p></p>` の中を修正しています。


```html
[li.html 修正前]

<div class="card-subtext muted-text">
  <p>Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "Jan 2, 2006" }}</time></p>
  {{ if (isset .Params "categories") }}<p>{{ range .Params.categories }}#{{ . }} {{ end }}</p>{{ end }}
</div>
```


```html
[li.html 修正後]

<div class="card-subtext muted-text">
  <p>Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "2006-01-02" }}</time>
     / Update {{ .Lastmod.Format "2006-01-02" }}</p>
  {{ if (isset .Params "categories") }}<p>{{ range .Params.categories }}#{{ . }} {{ end }}</p>{{ end }}
</div>
```
### 更新日がうまく取得できずに悩む → 解決したっぽい［2019-02-25］
手動でコミットしてプッシュするといけるけど、スクリプトでやるとダメっぽいぞ・・・。  
うーん、何が悪いんだ。

**［2019-02-28］**  
**サーバー側で反映されるのに、ちょびっとだけ時間がかかるだけだった疑惑発生！！**

#### 更新日がうまく入らない
プッシュするのにコマンドをいくつも入れるのが面倒なので、ネットで調べて下記のスクリプトを組んであります。

```sh
#!/bin/bash
echo -e "\033[0;33mDeploying updates to GitHub...\033[0m"
# Build the project.
hugo
# Go to public folder
cd public
# Add changes to git.
git add .
# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"
# Push source and build repos.
git push origin master
# Come back
cd ..
# Add changes to git.
git add .
# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"
# Push source and build repos.
git push origin master
```
手動で同じことをやっていくと反映するんですけど、これを実行したときってうまく反映されないような。  
気のせいかな。  

手動とスクリプトで何か違うのかな。

#### 更新日がうまく入らない（解決したっぽい）
`config.toml` に追記することで解決したっぽい。  

```toml
[frontmatter]
date = ["date", "publishDate", "lastmod"]
lastmod = [":git", "lastmod", "date", "publishDate"]
publishDate = ["publishDate", "date"]
expiryDate = ["expiryDate"]
```
`[frontmatter]` というセクションを追加し、その下に日付的な設定を追加しました。  
参考にしたのは [Configure Hugo](https://gohugo.io/getting-started/configuration/#configure-front-matter "Configure Hugo") です。

もしかしたら、`lastmod` の行だけでもいいかもしれない。  
どの段階を最終更新と見るかという設定で、左から優先？みたい。  
デフォルトがこの設定なので、本来は書かなくても良さそうなんだけど・・・。  
まぁ、動いたから良しとする。

#### 更新日の件［2019-02-28］
サーバー側で反映されるのに時間がちょびっとかかるだけで、元々出来ていたような気がしてきた。  
そして、カテゴリーとタグを付け直したら、**全ページの最終更新日が 2019-02-27 になってもうた。**  
本文中の修正でなくても反映するのね。そりゃそうか。

どうも気持ち悪いので、ファイルのタイムスタンプから更新日を取得するように変更しました。  


参考にしたのは公式のドキュメント  
[Configure Dates](https://gohugo.io/getting-started/configuration/#configure-dates)

`:fileModTime` というのが、ファイルのタイムスタンプを見てくれるようなので、`config.toml` を下記のように変更です。  

`lastmod = [":fileModTime", ":git", "lastmod", "date", "publishDate"]`

`lastmod` のところで、一番左側に追加しました。  

尚、ファイルはすべて 2019-02-27 のタイムスタンプになっているので、別途アプリで弄っています。

#### 更新日の件［2019-03-04］
ファイルのタイムスタンプだと、git-pull で落としてきたときの日付が入っちゃって、やっぱり意図したものと違う。  
結局、元の設定に戻しました（苦笑）

`lastmod = [":git", "lastmod", "date", "publishDate"]`

### フォントを指定する［2019-02-25］
やっぱり見た目は大事ですよね。  
ということで、フォントを変えてみようと思います。
#### Webフォントを使う
フォントが汚いので、Webフォントで有名な [Google Fonts](https://fonts.google.com "Google Fonts") を使ってみました。（[姉妹サイト（本家）](https://k-kaz.net "姉妹サイト（本家）") でも使っている）  
PageSpeed Insights のモバイルの点数が **15点落ちました** ので、一瞬で戻しました（笑）  
パソコンのほうは相変わらず100点でしたけどね。モバイル、厳しいなぁ。  

Webフォントじゃなくてもきれいに表示することはできるのかしら。調べてみよう。

尚、使うときは、`<head>` での読み込みと、`css` での指定が必要となります。  
以下のファイルを修正します。  

`/サイト名/layouts/partials/head.html`  
`/サイト名/static/css/hoge.css`  

尚、この後に書いている内容は、[姉妹サイト（本家）](https://k-kaz.net "姉妹サイト（本家）") で使っているものを若干修正したサンプルです。  
使うフォントによってタグの内容も変わりますので、[Google Fonts](https://fonts.google.com "Google Fonts") で好きなフォントを選んで、それ用のタグを入手してください。

```html
［head.html に下記を貼り付ける］

<!-- webfontloader -->
<script>
window.WebFontConfig = {
  google: { families: ['Roboto:400,700', 'Noto+Sans+JP:400,700'] },
  active: function() {
    sessionStorage.fonts = true;
  }
};
(function() {
  var wf = document.createElement('script');
  wf.src = 'https://ajax.googleapis.com/ajax/libs/webfont/1.6.26/webfont.js';
  wf.type = 'text/javascript';
  wf.async = 'true';
  var s = document.getElementsByTagName('script')[0];
  s.parentNode.insertBefore(wf, s);
})();
</script>
<!-- webfontloader ここまで-->
```

```css
［hoge.css に下記を貼り付ける］

body * {font-family: 'Roboto', 'Noto Sans CJK JP', 'Noto Sans JP', sans-serif !important;}
```
スタイルシートのほうは、かなり乱暴な書き方をしていますけど、テストということで許してください（笑）

#### ローカルフォントを指定する
とりあえず、最近は **游ゴシック体** が良いみたいなので、下記のように指定しました。  
游ゴシックに Medium が入っているのは、**標準の太さだとかすれちゃう問題** が発生するからだそうです。

```css
font-family: "游ゴシック Medium", "Yu Gothic Medium", "游ゴシック体", "YuGothic", "ヒラギノ角ゴ ProN W3", "Hiragino Kaku Gothic ProN", "メイリオ", "Meiryo", "verdana", sans-serif;
```

### コード表示にサイズ制限を設ける ［2019-03-14］
こちらのサイトでは、よくコードの紹介をしています。（このページもそうですね）  
コードが長くなった場合に記事がえらく間延びしてしまうので、その対策をしたいと思います。  
尚、gist などを使えば自動的にサイズ調整はしてくれますので、ここでは素でコード記述をする場合の内容です。
#### スタイルシートを修正する

```css
code {
  max-width: 100vw;
  max-height: 20vw;
}
```

最大の幅、高さを設定しました。
スクロールバーを出すには `overflow` を入れてあげるのが正しいようですが、それをするとスクロールする必要のないコードまでスクロールバーが表示されて、格好悪かったです。OS やブラウザによるみたいですけど。  

で、それを抜いてあげたら、Hugo か、このテーマ内でうまいこと処理をしてくれましたので、めでたしめでたし。

## タグ問題を解決する［2019-02-28］
aether は、カテゴリーのみの運用で、タグの使用は意図されていません。  
タグを使いたい場合は、自力で頑張ることになります。

### タグを使えるようにする
2つのファイルを修正してあげます。
#### 設定ファイルの修正
設定ファイル `config.toml` に `tag = "tags"` を追加します。  
カテゴリーとまとめて書いておくと見やすいかと思います。

```toml
[taxonomies]
category = "categories"
tag = "tags"
```
#### 記事テンプレートの修正
もう1つ。  
記事データに `tags: [""]` を追加しますが、毎回手動で入れるのは大変なので、テンプレートの方を直しておきます。  
投稿の仕方によって変わりますが、私は `post` というフォルダの中に記事を入れているので、下記のファイルを修正します。

`サイト名\archetypes\post.md`

私のファイルはこんな感じになっています。
```post.md
---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
date: {{ .Date }}
description: ""
categories: [""]
tags: [""]
featuredImage: ""
featuredImageDescription: ""
dropCap: false
displayInMenu: false
displayInList: true
draft: true
---
```
こうしておけば、`hugo new post/hoge.md` としたときに `post` に属する記事ということで、この `post.md` をテンプレートとして読み込んでくれ、ちょっとだけ楽になるのと、タグの付け忘れがなくなります。

尚、記事のほうで `tag` が複数ある時は `tags: ["aaa","bbb","ccc"]` と書いてください。

### タグを表示する
ここまでの作業で、記事にタグ付けが出来ています。  
ただ、残念なことにそのタグを表示する術がありません。  
ここから HTML のテンプレートを修正し、好きな場所にタグを表示させるようにします。

#### トップページ

`サイト名\layouts\_default\li.html`

一部分だけ抜き出しています。
```html
<article class="card-body">
  <h2 class="card-title">{{ .Title }}</h2>
  <p class="card-text">{{ if (isset .Params "description") }}{{ index .Params "description" }}{{ else }}{{ .Summary }}{{ end }}</p>
  <div class="card-subtext muted-text">
    <p>Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "2006-01-02" }}</time>
      / Updated {{ .Lastmod.Format "2006-01-02" }}</p>
  </div>
  <div class="card-subtext muted-text li-line">
      {{ if (isset .Params "categories") }}{{ range .Params.categories }}<span class="box-gray">{{ . }}</span> {{ end }}{{ end }}
      {{ if (isset .Params "tags") }} {{ range .Params.tags }}<span class="box-gray"># {{ . }}</span> {{ end }}{{ end }}
  </div>
</article>
```
先日、更新日を追加した修正のとき、カテゴリーも同じ `<div></div>` の中に入っていましたが、外側に出しています。  
もう1つ `<div></div>` を用意して、その中にカテゴリーと、今回追加のタグを入れています。

**よく分かっていませんが、`categories` を `tags` にしたら動くだろう**と思ってやったら、その通り動きました。  
私が凄いのではなくて、感覚的にできちゃうという分かりやすい設計の Hugo が凄いのでしょう。

ついでに、カテゴリーに `#` が付いていましたが、タグのほうに持ってきました。  
あと、`class` を入れていますが、これは見た目の装飾だけで、動きには影響していません。

#### 個別記事
`サイト名\layouts\_default\single.html`

一部分だけ抜き出しています。
```html
<header class="post-header">
 	<h1 class="post-title">{{ .Title }}</h1>
    <p class="post-date">
      Posted <time datetime="{{ .Date.Format "2006-01-02" }}">{{ .Date.Format "2006-01-02" }}</time>
      / Update {{ .Lastmod.Format "2006-01-02" }}</p>
    <div class="card-subtext muted-text li-line">
      {{ if (isset .Params "categories") }}{{ range .Params.categories }} <a class="cat" href="/categories/{{ . }}">{{ . }}</a>{{ end }}{{ end }}
      {{ if (isset .Params "tags") }}{{ range .Params.tags }} <a class="tag" href="/tags/{{ . }}"># {{ . }}</a> {{ end }}{{ end }}
    </div>
</header>
```
こちらもトップとほぼ同じですが、リンクの設定をしてあります。  
表示されたカテゴリーやタグをクリックすると、同一グループが一覧表示されるようになります。  
アドレスだけ気を付けてあげれば、一覧表示はテーマ側が自動でやってくれます。

if文の中で、該当する文字列が見付かれば、リンクに展開するって感じかな。（知らんけど）  
複数見付かれば、その回数分、リンクを展開してくれます。

カテゴリーのアドレス指定  
`<a class="cat" href="/categories/{{ . }}">{{ . }}</a>`

タグのアドレス指定  
`<a class="tag" href="/tags/{{ . }}"># {{ . }}</a>`

それぞれ `{{ . }}` の中に、読み込まれたカテゴリーやタグが自動で入力されます。  
カテゴリーが `homepage` なら `/categories/homepage` と展開されます。

私のサイトでは、このタグを上部と下部の2ヶ所に入れてみました。

#### 次の記事
個別記事の最下部に次の記事が表示されます。  
そこにも入れておきます。

`サイト名\layouts\_default\li-next.html`

一部分だけ抜き出しています。
```html
<article class="card-body">
  <h2 class="card-title">{{ .Title }}</h2>
  <p class="card-text">{{ if (isset .Params "description") }}{{ index .Params "description" }}{{ else }}{{ .Summary }}{{ end }}</p>
  <div class="card-subtext muted-text">
    <p>Posted <time datetime="{{ .PublishDate.Format "2006-01-02" }}">{{ .PublishDate.Format "2006-01-02" }}</time>
    / Updated {{ .Lastmod.Format "2006-01-02" }}</p>
  </div>
  <div class="card-subtext muted-text li-line">
    {{ if (isset .Params "categories") }}{{ range .Params.categories }}<span class="box-gray">{{ . }}</span> {{ end }}{{ end }}
    {{ if (isset .Params "tags") }}{{ range .Params.tags }}<span class="box-gray"># {{ . }}</span> {{ end }}{{ end }}
  </div>
</article>
```
トップページと同じかな。

#### メニューボタン
右上にある 三 みたいなボタンですけど、これを押すとカテゴリーを表示することができます。  
ここはもう趣味なんですけど、この中にタグまで表示したい場合は下記を修正します。

`サイト名\layouts\partials\nav-bar.html`

一部分だけ抜き出しています。
```html
<div class="hamburger-menu">
  <button onclick="hamburgerMenuPressed.call(this)" aria-haspopup="true" aria-expanded="false" aria-controls="menu" aria-label="Menu">
    <span></span>
    <span></span>
  </button>
  <ul id="menu" class="hamburger-menu-overlay">
    <li><a href="{{ .Site.BaseURL }}" class="hamburger-menu-overlay-link">Home</a></li>
    {{ range where .Site.Pages "Params.displayinmenu" true -}}
      <li><a href="{{ .Permalink }}" class="hamburger-menu-overlay-link">{{ .Title }}</a></li>
    {{ end -}}
    <li class="navi-menu">- Categories -</li>
    {{ range $key, $value := .Site.Taxonomies.categories -}}
    <li style="font-size: 0.7em;"><a href="{{ (printf "%s%s" "categories/" ($key | urlize)) | absURL }}" class="hamburger-menu-overlay-link">{{ $key | humanize }}</a></li>
    {{- end }}
    <li class="navi-menu">- Tags -</li>
    {{ range $key, $value := .Site.Taxonomies.tags -}}
    <li style="font-size: 0.5em;"><a href="{{ (printf "%s%s" "tags/" ($key | urlize)) | absURL }}" class="hamburger-menu-overlay-link">{{ $key | humanize }}</a></li>
    {{- end }}
  </ul>
</div>
```
すでにカテゴリーは記載済みですので、そこをコピペしてタグ用の行を追加しています。  
`categories` = `tags` で置換してあげれば良いです。  

ついでにカテゴリーとタグの区切りを入れたかったので、`<li class="navi-menu">- Categories -</li>` と `<li class="navi-menu">- Tags -</li>` を追加しました。

`class` は例のごとく見栄えだけです。

メニューボタンの隣に並べることも考えたんですが、モバイルでレイアウトが崩れたので、メニューに入れてしまうという安易なやり方にしております。数が増えたらえらいことになるんだろうなと思いつつ（笑）

最近は、ブラウザの検証機能を使って、パソコンとモバイルの両方で表示して確認するというクセがついてきました。（良いことです）

### カテゴリーとタグの表示調整
カテゴリーやタグの数が増えると表示が崩れます。  
（縦に伸びて文字全てを表示するようになるので、格好悪い）

ここもとりあえず `@media screen` で、幅の小さい端末の場合は、フォントサイズを小さくして誤魔化していますが、
数が増えてレイアウトが崩れるまえに改行表示するように、根本的な修正をしたいんですけどね。それはまた・・・。

#### その後
インライン要素のために改行が出来ていなかったので、モバイルのときはブロック要素に変更するようスタイルシートを調整しました。これでウダウダと書いた上の問題は解消したと思います。

---
また何かあれば、随時追加します。  
Markdown 記法も覚えなきゃね。
