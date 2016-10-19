+++
categories = ["Setting"]
date = "2016-08-30T14:45:11+09:00"
description = "Hugo,GitHub Pages,独自ドメインで静的サイトを構築する。GitHub Pagesと独自ドメインの適用。"
draft = false
slug = "build-a-blog-in-hugo-2"
tags = ["Blog","Hugo"]
title = "Hugo・GitHub Pages・独自ドメインでブログを構築 : 2"

+++

GitHub側の設定と独自ドメインの適用を行う。

- GitHubの設定
- 独自ドメインの適用


## GitHub
このリモートリポジトリの持つ意味は２つ。

- ブログ作業環境 : ローカルの作業環境がそのまま。git cloneすればどこでも作業できる
- 公開環境 : GitHub Pageを利用した公開環境（masterブランチ/docsディレクトリ）


## GitHub Pages
GitHub Pagesの形態として、**ユーザーページ** と **プロジェクトページ** の２つがある。

- ユーザーページ : リポジトリ名を「アカウント名.github.io」で作成
- プロジェクトページ : リポジトリ名は任意で、masterブランチにdocsディレクトリを作成する
  - docsディレクトリ方式は、2016.8に追加された機能
  - 以前はgh-pagesブランチを作成する必要があった

今回は環境ごと管理したかったので、プロジェクトページ形式で作成する。


## リモートリポジトリ : GitHub
任意の名前（今回はサイトと同名のmyblog）でリポジトリを新規作成。  
- Initialize this repository with a README → 未チェック  
- リポジトリのURL : https://github.com/<アカウント>/<リポジトリ名>.git


## ローカルリポジトリ : Mac
リモートにプッシュを行う。
```@bash
# リモートリポジトリの追加
$ git remote add origin https://github.com/mzgk/myblog.git

# リモートの確認
$ git remote -v
masamune       	https://github.com/mzgk/masamune.git (fetch) # テーマ用（subtree）
masamune       	https://github.com/mzgk/masamune.git (push)
origin 	https://github.com/mzgk/myblog.git (fetch) # ブログ環境用
origin 	https://github.com/mzgk/myblog.git (push)

# プッシュ
$ git push -u origin master
```


## 独自ドメインの設定
ドメインは、ムームードメインで取得済み。  
作業としては、以下。  

- ムームードメイン側のAレコードにGitHubのアドレスを設定する
- GitHubリポジトリのdocsディレクトリにCNAMEファイルを格納する
- GitHubリポジトリでGitHub Pagesの設定をする

## ムームードメイン側の設定（DNS設定）
Aレコードの値は[GitHubのページ](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider)を参照。  

- ログイン -> ドメイン管理 : ムームーDNS -> ドメインを選択 -> 変更
- カスタム設定のセットアップ情報変更 -> 設定２ ->
- サブドメイン : 未入力
- 種別 : A
- 内容 : 192.30.252.153
- サブドメイン : 未入力
- 種別 : A
- 内容 : 192.30.252.154

セットアップ情報変更を押下。

## CNAMEファイル
- ローカルのdocsディレクトリに『CNAME』という名前でファイルを作成
- 中身には取得してあるドメイン名を記述（myblog.com）

### config.toml
- baseurlをGitHub PageのURLから、独自ドメインに変更する

### 生成〜コミット〜プッシュ
```@bash
# 公開ファイルを生成
$ hugo -t masamune

# docsディレクトリにコピー
$ cp -p -f -R public/* docs

# コミット
$ git add .
$ git commit -m "独自ドメインを設定"

# master
$ git push origin master
```


## GitHubの設定（GitHub Pages）
GitHub上で、GitHub Pagesの設定を行う。  
### Source
作成したリポジトリ → Settings → GitHub Pages → Source  
「master baranch /docs folder」を選択 → Save

### Custom domain
作成したリポジトリ → Settings → GitHub Pages → Custom domain  
ドメインを入力 → Save  

<br>

Saveが完了すると、GitHub Pagesの部分にURLが表示される。  
そのURLがGitHub Pages（今回のブログ）のURLとなる。  
アクセスすると、docsディレクトリ以下のWebページが閲覧できる。


## 確認
しばらくしてブラウザから設定したドメインにアクセスし、反映されていることを確認する。


### 次は、記事の作成から。
