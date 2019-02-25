---
title: "テーマ aether のカスタマイズ"
date: 2019-02-18T21:30:02+09:00
description: "テーマ aether のカスタマイズをこちらにまとめます。"
categories: [ホームページ]
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

---
## 標準機能内
---
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

---
## 標準機能外
---
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


---
また何かあれば、随時追加します。  
Markdown 記法も覚えなきゃね。
