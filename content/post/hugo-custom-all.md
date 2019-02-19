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
**念の為、テーマを丸ごとコピーして、そちらを弄ることにします。**  

その際、以下の修正が必要です。

`/サイト名/themes/新しいテーマ名/theme.toml`  
`name = "新しいテーマ名"` 

`/サイト名/config.toml`  
`theme = "新しいテーマ名"`

---
### 標準機能内
---
#### ホームボタンの設定
{{<smallimg src="/images/aether-custom-home-btn_400.webp" alt="ホームボタンの設置" width="400px">}}
個別記事を開いた時に、一番下に真っ白なスペースがあります。  
ここは、ホーム（トップページ）に戻るためのボタンになっており、Home という白文字が入っています。**実際は白同士で見えないんですけど。**
  
`homeimg` に少し暗めの画像を指定すると、文字がハッキリして見やすいと思います。

`/サイト名/config.toml`
<script src="https://gist.github.com/k-kaz-git/2df71ae7a4d133f118cf424e6d48476b.js"></script>

---
### 標準機能外
---
#### コメント用の外部サービスを外す
テーマには、個別記事に disqus のサービスが組み込まれていますが、これも使いませんので外します。

`/サイト名/themes/aether/layouts/_default/single.html`
<script src="https://gist.github.com/k-kaz-git/6fbc59bd45e57c5086e3d4e7a87ac63b.js"></script>

#### テンプレートに外部リンクを仕込む
{{<smallimg src="/images/aether-custom-link-btn_175.webp" alt="外部リンクボタンの設置" width="175px">}}
表示させる場所によって、編集するファイルは変わりますが、今回は最下部（フッター）に入れることにしました。普通に HTML を修正します。  
footer.html は、`<footer></footer>` と、空っぽなので、その間にタグを入れます。

`/サイト名/themes/aether/layouts/footer.html`
<script src="https://gist.github.com/k-kaz-git/2dd802712a513a5017593a9d77536aa3.js"></script>


#### スタイルシートを適用する
テーマ内のスタイルシートではなく、別途カスタマイズ用のファイルを用意します。  
また、そのファイルを読み込むよう、HTML の修正をします。

##### カスタマイズファイルを用意 #####
`/サイト名/static/css/hoge.css`  
css というフォルダも自分で作成し、その中にスタイルシート用のファイルを作ります。

##### スタイルシートの読み込み #####
`/サイト名/themes/新しいテーマ名/layouts/partials/head.html`  
スタイルシートの読み込み行が続いているので、一番下に `/css/hoge.css` を追加します。  
<script src="https://gist.github.com/k-kaz-git/be3470f0ad5ab3538adcc7df1bb6182e.js"></script>


---
また何かあれば、随時追加します。
