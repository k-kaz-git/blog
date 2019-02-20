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
{{<smallimg src="/images/aether-custom-home-btn_400.webp" alt="ホームボタンの設置" width="400px">}}
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
{{<smallimg src="/images/aether-custom-link-btn_175.webp" alt="外部リンクボタンの設置" width="175px">}}
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



---
また何かあれば、随時追加します。  
Markdown 記法も覚えなきゃね。