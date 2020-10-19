---
title: "Deploy でエラーが出たんですけど。"
date: 2020-10-14T18:51:11+09:00
description: "1年半振りの Deploy で・・・。"
categories: ["homepage"]
tags: ["hugo" , "github" , "Netlify"]
featuredImage: "/images/hugo-deploy-error.webp"
featuredImageDescription: "厳重な扉"
dropCap: false
displayInMenu: false
displayInList: true
draft: false
---
## 1年半振りの Deploy で・・・
Netlify に記事を上げようと思って、`./deploy.sh` （※スクリプトを組んである）とやってみたけど、うまくいかず。  
うっかりやってしまう `draft: false` もちゃんとなっているし、なんだろうと思って、ひとつずつチェックしてみました。

ひっかかったのが、ssh 認証のところ。

`ssh -T git@github.com` で、ssh が通るか試してみたら、`git@github.com: Permission denied (publickey).` って怒られました。

```html
$ ssh -T git@github.com     /* ssh が通るか試すコマンド */
$ git@github.com: Permission denied (publickey).
```


ネットで調べてみたところ、 **「Github に公開鍵（publickey）を登録しましょう」** と書いてあったので、1年半振りの Github ログイン。

**お前、本当に k-kaz か？**  
**メール送ったから、そこに書かれた6桁のコードを入れてみろよ！**

って、疑われ、言われた通りに6桁のコードを入れてログイン。

[ssh の項目](https://github.com/settings/ssh) を覗くと公開鍵がありません。  
PC に保存していた公開鍵の中身を登録して、再度 `ssh -T git@github.com` を実行すると、うまく繋がりました。

1年半の間に仕様が変わったのか、削除されちゃったのかよく分かりませんが、再登録した形ですね。覚えていないけど、たぶん。

で、Netlify にもログインする必要がありました。
こちらは疑われることなくできましたが、ここまでやらないと記事の反映ができなかったです。


