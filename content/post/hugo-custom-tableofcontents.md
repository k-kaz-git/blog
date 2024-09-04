---
title: "Hugo の目次について考える"
date: 2019-04-02T19:26:27+09:00
description: "Hugo に目次を入れようと思ったのです。"
categories: ["homepage"]
tags: ["hugo" , "tableofcontents"]
image: "/images/hugo-tableofcontents-menu.webp"
imageDescription: "メニュー、目次のイメージ画像"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## Hugo の目次について
Hugo には、コンテンツ内に登場する見出しタグ（H）を自動的にリスト化してくれる機能 `TableOfContents` が用意されています。  

**それすなわち、目次ですね。**

### 使い方
`single.html` なんかの表示したい部分にこれを追記します。

```html
{{ .TableOfContents }}
```

たったこれだけです。

### 出力サンプル

```html
<nav id="TableOfContents">
  <ul>
    <li>
      <ul>
        <li><a href="#h2-見出しタイトル-その１">H2 見出しタイトル その１</a></li>
        <li><a href="#h2-見出しタイトル-その２">H2 見出しタイトル その２</a>
          <ul>
            <li><a href="#h3-見出しタイトル">H3 見出しタイトル</a>
              <ul>
                <li><a href="#h4-見出しタイトル">H4 見出しタイトル</a>
                  <ul>
                    <li><a href="#h5-見出しタイトル">H5 見出しタイトル</a></li>
                  </ul>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</nav>
```

`<nav id="TableOfContents">` というタグで囲まれたリストが出来上がります。  
あとはお好みのスタイルを適用してあげるだけ。

### 考慮すべき点
見出しタグが無い場合に、空っぽのリストが出てきちゃうので、対策が必要です。

#### 自動で見出しタグの有無を判断し、無い場合は目次を出さない
やり方は、なんとなく分かっているんですが、まだ内容を理解出来ていないので、また今度。

#### 400字未満の場合は目次を出さない
[公式サイト](https://gohugo.io/content-management/toc/) に記述があります。  
短い記事であれば目次は要らんでしょうという考え方かな。

if文で文字数の判断をして、出す出さないを決めています。

```html
{{ if and (gt .WordCount 400 ) (.Params.toc) }}
<aside>
    <header>
    <h2>{{.Title}}</h2>
    </header>
    {{.TableOfContents}}
</aside>
{{ end }}
```

#### 記事毎にフラグを立てる
自作テーマで立ち上げている Nerlify のほう [k-kaz が好きなことを書くサイト](https://k-kaz.netlify.com) はコレを採用しています。

`single.html` にはこのように記述。

```html
{{ if eq .Params.toc true }}
  <aside>
  {{ .TableOfContents }}
  </aside>
{{ end }}
```

記事 `hoge.md` のパラメータ部分に追加。

```md
toc: true
```

この2つをセットにして、記事の `toc` というパラメータが `true` なら、`{{ .TableOfContents }}` を実行するようにしています。実行したくなければ `false` としておきます。  

また、毎回書くのも面倒なので、`archetypes/post.md` という記事のテンプレートファイルに予め入れました。

#### ちなみにこのサイトでは・・・
**必ず目次は表示する、そして、必ず見出しを書く** という、己に厳しいルールが制定されているので大丈夫です（笑）  
イヤイヤ、いずれちゃんと設定します・・・。

#### ”目次” という言葉
これまで html に直接書いていたんですが、`toc: false` にしても、”目次” だけ出てきちゃうんですよね。  
`{{ .TableOfContents }}` のところを `{{ partial "toc.html" . }}` として、別途用意した `toc.html` の中で ”目次” と書いておけば、不要なときに呼びされずに済んでスマートです。

私は別ファイルを用意するのが面倒だったので、スタイルシートの中で ”目次” を呼び出すようにして、良しとしました。（横着）

```css
aside::before {
    content: "目次";
}
```
