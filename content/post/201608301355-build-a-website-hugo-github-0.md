+++
categories = ["Setting"]
date = "2016-08-30T13:58:05+09:00"
description = "Hugo,GitHub Pages,独自ドメインで静的サイトを構築する。方針など。"
draft = false
slug = "build-a-blog-in-hugo-0"
tags = ["Blog","Hugo"]
title = "Hugo・GitHub Pages・独自ドメインでブログを構築 : 0"

+++
## 望むこと
1. 静的なWebサイト
  - 動的にするほどのコンテンツはないし、WordPressだといろいろ面倒
1. 記事はエディタを使ってMarkdown形式で作成し、テキストファイル（.md）として残したい
  - DBにデータとして保存されているとかだと取り回しが面倒
1. ブログ環境と作成した記事のファイルは、GitHub（or Bitbucket）で管理したい
  - マシンが変わっても `$ git clone` 一発で環境構築ができる
1. 独自ドメイン
  - バックのシステムが変わってもURLを変えたくない
1. サーバーの運用を軽くしたい
  - VPSとかS3とか契約や運用が面倒（勉強にはなるけど、アプトプットに集中したい）


## そうなると
- GitHubにGitHub Pages（プロジェクト版）の機能があるじゃないか！
- Gitの一連の流れ（add → commit → push）だけで、FTPとか使わずにいける！
- 2016.8の新機能でmasterブランチ/docsディレクトリでもいける
  - 前はgh-pagesブランチが必要
  - １リポジトリで管理運用とかsubtree運用が少し面倒だった
- 独自ドメインも適用できる


## ブログ遍歴
- d.hatena → WordPress → Octopress → hatenaBlog
- hatenaは手軽でいいが、ブラウザでの作業とファイルの管理は自前
- WordPressは書いたものがDB格納・機能過剰・WordPress自体の管理運用が面倒
- Octopressは、Macクリーンインストール等での環境維持が面倒だった


### 次はインストールから
