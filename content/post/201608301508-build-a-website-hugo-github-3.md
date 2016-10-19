+++
categories = ["Setting"]
date = "2016-08-30T15:08:35+09:00"
description = "Hugo,GitHub Pages,独自ドメインで静的サイトを構築する。記事の作成から公開まで。"
draft = false
slug = "build-a-blog-in-hugo-3"
tags = ["Blog","Hugo"]
title = "Hugo・GitHub Pages・独自ドメインでブログを構築 : 3"

+++

- 記事の作成
- 公開ファイルの生成
- 公開


## 記事の作成準備
記事作成時の雛形となるファイル（default.md）を「archetypes」ディレクトリに作成する。  
テーマをインストールしている場合は、「themes/テーマ名/archetypes」にdefault.mdが存在する場合があるので「archetypes」ディレクトリにコピーする。  
項目はテーマに対応するので、テーマのサイトを参照し設定すること。以下は主な項目例。
```@toml
+++
draft = true         # 作成中（作成後にfalseにする）
date = "now()"    # 作成日時が入る
categories = [""] # カテゴリーを入れる
tags = ["", ""]   # タグを入れる
title = ""       # 記事のタイトル（ファイル名が入るので変更する）
description = ""  # 説明
slug = "" # URLになる（指定しない場合は、URLがファイル名になる）
+++
```


## 記事の作成
環境のトップでhugoコマンドを実行し、ファイルを作成する。
```@bash
# 環境のトップに移動（ここではmyblogディレクトリ）
$ cd ~/myblog

# 記事ファイルを作成
# --editor="atom"をつけるとATOMで開いてくれる
$ hugo new post/ファイル名.md

# content/post/にファイルが作成される
```


## 記事を書く
- ヘッダー部分（+++ 〜 +++）を設定する
- ２つ目の+++の下から記事をMarkdownで書いていく


## 確認（ライブリロード）
Hugoのローカルサーバを起動し、ブラウザで確認しながら書くことができる。  
アドレスはhttp://localhost:1313/
```@bash
# ローカルサーバを起動
# -t : テーマ
# -D : draft=trueの記事も表示する
# -w : ライブリロードを有効にする
$ hugo server -t masamune -D -w

# 停止はCtrl+c
```


## 公開ファイルの生成
記事を書き終わり確認ができたら、ヘッダ部分の **draft=true → false** に変更する。  
公開ファイルを生成し、docsディレクトリにコピーを行う。
```@bash
# draft=true → falseに変更して保存

# 生成（publicディレクトリに出力される）
$ hugo -t masamune

# docsディレクトリにコピーする
$ cp -p -f -R public/* docs
```


## 公開
GitHubにプッシュし、公開する。
```@bash
# コミット
$ git add .
$ git commit -m "コミットメッセージ"

# プッシュ
$ git push origin master
```


### 次回は一連の作業をまとめてスクリプト化する方法
