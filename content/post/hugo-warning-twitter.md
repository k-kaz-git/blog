---
title: "Hugo Warning Twitter"
description: "環境による"
date: 2025-03-10T21:49:08+09:00
image: "/images/hugo-warning-sitesocial.webp"
imageDescription: "WARNING"
categories: ["homepage"]
tags: ["Hugo","Theme"]
dropCap: false
displayInMenu: false
displayInList: true
comments: true
draft: false
---
## 環境によります

設定ファイルにTwitterの記述がある場合に出てきます。  
当サイト（theme: Stack）は影響なく、ブログ（theme: PaperMod）のほうで発生しました。  

以下、ブログでの話。  

## アップデートしてみたら（v0.143.1 → v0.145.0）

さぼっていて、いくつか飛ばしましたけど本日アップデート実行。  

Hugo Serverを起動すると以下のWarningが。  

``` bash
WARN  deprecated: site config key privacy.twitter.enableDNT was deprecated in Hugo v0.141.0 and will be removed in a future release. Use privacy.x.enableDNT instead.
WARN  deprecated: site config key privacy.twitter.simple was deprecated in Hugo v0.141.0 and will be removed in a future release. Use privacy.x.simple instead.
WARN  deprecated: site config key services.twitter.disableInlineCSS was deprecated in Hugo v0.141.0 and will be removed in a future release. Use services.x.disableInlineCSS instead.
```

3つのWarningが出ています。  
いずれも、Hugo v0.141.0 で非推奨になり、将来使えなくなりますよって話。  

| 非推奨                            | → | 推奨                        |
| --------------------------------- | --- | --------------------------- |
| privacy.twitter.enableDNT         | → | privacy.x.enableDNT         |
| privacy.twitter.simple            | → | privacy.x.simple            |
| services.twitter.disableInlineCSS | → | services.x.disableInlineCSS |

## 直す

### 問題が出ている場所を探す

非推奨の文字列をサイト全体で検索するものの、見当たらず。  
どういうことだとしばらく調べていて、ようやく気付きました。  

設定ファイル `config.yml` の中でした。  

```yml {name=config.yml linenos=true}
privacy:
  twitter:
    disabled: false
    enableDNT: true
    simple: true

services:
  twitter:
    disableInlineCSS: true
```

階段になっているものを続けて読むと、非推奨の文字列が出てきました。  

- 1行目、2行目、4行目 → privacy.twitter.enableDNT
- 1行目、2行目、5行目 → privacy.twitter.simple
- 7行目、8行目、9行目 → services.twitter.disableInlineCSS

3行目はfalseだから引っかからなかったのかな。  

### 修正する config.yml

2行目と8行目を直す（twitter → x）だけでWarningは出なくなりました。  

```yml {name=config.yml linenos=true}
privacy:
  x:
    disabled: false
    enableDNT: true
    simple: true

services:
  x:
    disableInlineCSS: true
```

めでたしめでたし。  