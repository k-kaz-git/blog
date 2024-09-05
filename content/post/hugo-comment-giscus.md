---
title: "Hugoにコメント機能を付ける（giscus編）"
date: 2024-02-20T08:43:46+09:00
description: "giscusにしてみました。"
categories: ["homepage","coding",]
tags: ["hugo","theme"]
image: "images/speech-bubbles_640.webp"
imageDescription: ""
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## utterancesからgiscusへ

以前、コメント欄を付けるぞとこんな記事を書きました。  

[Hugoにコメント機能を付ける](../hugo-comment)  

その際、GitHubのIssuesにコメントを保管する [utterances](https://utteranc.es/) という機能を利用しました。  

今回、その [utterances](https://utteranc.es/) から着想を得たという [giscus](https://giscus.app/ja) を利用してみることにしました。  
1年間で1件もコメントが来ていないのに変更するという・・・。  

## giscusって？

[utterances](https://utteranc.es/) とよく似ています。  
giscusは、GitHubのDiscussions機能を使います。  

ということは、こちらもコメント投稿時にGitHubアカウントが必要となります。  

## giscus 設定手順

### GitHub

#### アカウント取得とリポジトリ作成

- GitHubアカウントの取得
- コメント格納用リポジトリを作成

これらは前回とまったく同じなので、下記参照ください。  

[GitHubアカウントの取得｜Hugoにコメント機能を付ける](https://k-kaz-git.github.io/post/hugo-comment/#github%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%81%AE%E5%8F%96%E5%BE%97)  
[コメント格納用リポジトリを作成｜Hugoにコメント機能を付ける](https://k-kaz-git.github.io/post/hugo-comment/#%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%88%E6%A0%BC%E7%B4%8D%E7%94%A8%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%82%92%E4%BD%9C%E6%88%90)

#### 補足

- リポジトリの方は、公開設定（Public）にしておくことをお忘れなく。  
- [utterances](https://utteranc.es/) 等ですでにリポジトリを作ってあれば流用可能です。  
- Issuesに入っているコメントをDiscussionsへ移行する方法もあります。  
[Moderating discussions - GitHub Docs](https://docs.github.com/en/discussions/managing-discussions-for-your-community/moderating-discussions#converting-an-issue-to-a-discussion)  
- 設置するサイトが複数ある場合は、リポジトリも同じ数だけ用意します。  

#### Discussionsを有効にする

コメント格納用リポジトリで、Discussionsの機能を有効にします。  

1. Settingsへ移動
1. General内のFeaturesにあるDiscussionsを有効

### giscusアプリ

リポジトリ毎にインストールする必要がありますので、複数設置する場合はご注意を。  

#### インストール

1. [GitHub Apps - giscus](https://github.com/apps/giscus) へ移動
1. インストール
    - インストール先は今回のリポジトリを選択

#### 設定（コード取得）

1. [giscus](https://giscus.app/ja) へ移動
1. 自分好みに設定
1. 出来上がったコードを控える

### Hugo

#### コード挿入

1. コメント機能を表示したい場所へ先程のコードを挿入

私の場合は、記事の下に入れるため `single.html` の最後に入れました。  

```js
// single.html
{{- if (.Param "comments") }}
  <script src="https://giscus.app/client.js"
    data-repo="hoge/hoge-repo" //ユーザー名と保管用リポジトリ名
    data-repo-id="hoge-ID" //自動発行されるID
    data-category="Announcements" //Discussions内でどのカテゴリに保管するか
    data-category-id="hoge-ID" //カテゴリのID
    data-mapping="title" //保管時、表題となるもの（タイトル、URL等）
    data-strict="0"
    data-reactions-enabled="1"
    data-emit-metadata="0"
    data-input-position="bottom"
    data-theme="preferred_color_scheme" //テーマカラー
    data-lang="ja" //日本語
    data-loading="lazy" //遅延読み込み機能有効
    crossorigin="anonymous"
    async>
  </script>
{{- end }}
```

コメントで説明を入れてみましたが、違っていたらすいません。  

先程取得したコードをそのまま使っています。  
修正など不要で利用できています。ありがたや。  

## 所感

難しいことは何もなく、簡単にコメント機能の設置ができました。  

今回設置したのは下記サイトになります。  
[k-kaz が好きなことを書く](https://k-kaz.net/blog2/)
