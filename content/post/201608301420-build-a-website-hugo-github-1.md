+++
categories = ["Setting"]
date = "2016-08-30T14:20:11+09:00"
description = "Hugo,GitHub Pages,独自ドメインで静的サイトを構築する。インストール〜テーマの適用まで。"
draft = false
slug = "build-a-blog-in-hugo-1"
tags = ["Blog","Hugo"]
title = "Hugo・GitHub Pages・独自ドメインでブログを構築 : 1"

+++
- Hugoのインストール
- ブログ環境の構築
- Git管理
- テーマの適用
- 公開ファイルの生成


## 構築環境
- OS X 10.11.6
- Homebrew 0.9.9
- Hugo 0.16
- [GitHub](https://github.com/)はアカウントを作成済み
- ドメインは[ムームードメイン](https://muumuu-domain.com/)で取得済み


## Homebrew
アップデート（インストールは[公式サイト](http://brew.sh/index_ja.html)を参照）。
```@bash
$ brew update
$ brew upgrade
$ brew cleanup
$ brew doctor
```


## Hugo
Homebrew経由でインストールを行う。
```@bash
$ brew install hugo
# 確認
$ hugo version
Hugo Static Site Generator v0.16 BuildDate: 2016-06-06T21:37:59+09:00
```


## ブログ環境の作成
Hugoコマンドを使って、ローカルにブログ作業ディレクトリを作成する。  
今回の例では、Home直下にmyblogディレクトリ（~/myblog）として作成。
```@bash
# 環境作成
$ hugo new site ~/myblog # 任意のディレクトリ名
Congratulations! Your new Hugo site is created in "<作成された場所>".
...
```

サイト名のディレクトリが作成される。
```@bash
$ cd ~/myblog
$ tree -a
.
|-- archetypes  # 記事のテンプレートを格納（空）
|-- config.toml # サイト全体の設定ファイル
|-- content     # 記事を格納（空）
|-- data        # データファイルを格納（空）
|-- layouts     # レイアウトファイルを格納（空）
|-- static      # 画像等の静的ファイルを格納（空）
`-- themes      # テーマを格納（空）

6 directories, 1 file
```


## GitHub Pages用の公開ディレクトリを追加
GitHub Pagesのmasterブランチでの公開用に **docs** ディレクトリを追加する。  
Hugoコマンドでの公開ファイル生成後に、publicディレクトリの中身をこのdocsディレクトリにコピーする。
```@bash
# GitHub pages用の公開ディレクトリを作成
$ mkdir docs

$ tree -a
.
|-- archetypes
|-- config.toml
|-- content
|-- data
|-- docs       # 公開用ディレクトリ（GitHub Pages用）
|-- layouts
|-- static
`-- themes

7 directories, 1 file
```


## Git管理を開始する
作成された初期状態の環境をGit管理下におく。  
空ディレクトリも管理対象となるように「.gitkeep」の名前で空ファイルを格納しておく。
```@bash
# 空ディレクトリに対して.gitkeepファイルを作成しておく
$ touch archetypes/.gitkeep
$ touch content/.gitkeep
...
.
|-- archetypes
|   `-- .gitkeep
|-- config.toml
|-- content
|   `-- .gitkeep
|-- data
|   `-- .gitkeep
|-- docs
|   `-- .gitkeep
|-- layouts
|   `-- .gitkeep
|-- static
|   `-- .gitkeep
`-- themes
    `-- .gitkeep

7 directories, 8 files

# Git初期化
$ git init

# コミット
$ git add .
$ git commit -m "Initial : 環境作成直後"
```


## テーマのインストール
### 既存のテーマを使う場合
[公式テーマサイト](http://themes.gohugo.io/)でテーマを選び、インストールをする。  
インストールしたいテーマを選んだら、テーマのリポジトリを `git clone` してくる。  
今回はシンプルできれいな『[Angel's Ladder](http://themes.gohugo.io/angels-ladder/)』を例に。
```@bash
# themesディレクトリに移動
$ cd themes

# テーマのリポジトリからcloneしてくる
$ git clone https://github.com/tanksuzuki/angels-ladder
```
もしくは、テーマのリポジトリから.zip形式でダウンロードし、themesディレクトリに解凍する。  
**解凍したディレクトリ名はテーマの名称に変更する（angels-ladder-master → angels-ladder）。**


### 自作テーマの場合
自作のテーマがGitHub上の別リポジトリに存在する場合。例としてこのサイトのテーマ（masamune）。  
同じように `git clone` でもいいが、この環境でテーマの改修作業もする場合は `git subtree` の仕組みを利用する。  
前提として、myblogディレクトリがGit管理下に置かれていること。
```@bash
# 環境のトップに移動（ここではmyblogディレクトリ）
$ cd ~/myblog

# テーマのリポジトリをリモートに追加（リモート名 : masamuneとする）
$ git remote add masamune <テーマのリポジトリURL>

# テーマmasamuneをsubtreeとして取り込む
# --prefix=themesディレクトリの下にmasamuneというディレクトリで
# --squash : 履歴なし
# リモートリポジトリ名（masamune） master
$ git subtree add --prefix=themes/masamune --squash masamune master
git fetch masamune master
warning: no common commits
...
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> masamune/master
Added dir 'themes/masamune'
```
これで、themesディレクトリの配下にmasamuneディレクトリが作成されて、テーマが配置（インストール）される。


## テーマの設定
インストールしたテーマに即した設定を『config.toml』に行う。  
内容はテーマのサイトを参照すると記載されている。  
[Angel's Ladder](https://github.com/tanksuzuki/angels-ladder)は丁寧に記載されている。  


## サイトの設定（config.toml）
サイト自体の設定は『config.toml』に記述する。  
[本家サイト](http://gohugo.io/overview/configuration/)に記載されている。  
また、テーマごとに設定する項目が存在するので、テーマの公式サイトを参照すること。
```@yaml
baseurl = "GitHub PagesのURL（https://<アカウント>.github.io/<リポジトリ名>）"
title = "サイトのタイトル"
theme = "テーマ名" ← 確認・生成時に『-t テーマ名』が不要になる
languageCode = "ja-JP"
canonifyurls = true
hasCJKLanguage = true
```


## テーマの確認
ローカルサーバーを起動し、テーマが適用されているかブラウザで確認を行う。  
起動したら、ブラウザを立ち上げlocalhost:1313/myblogにアクセスする（起動時に表示される宛先を参照）。  
localhost:1313部分は、**「Web Server is available at」** の部分を参照すること。
```@bash
# 環境のトップに移動
$ cd ~/myblog
# config.tomlにthemeを設定している場合は、-t masamuneは不要
$ hugo server -t masamune -D -w
...
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)

# 停止する場合は『Ctrl + c』
```


## テーマの確認ができたらコミット
テーマの設定まで確認ができたら、一旦コミットしておく。
```@bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

       	modified:   config.toml

no changes added to commit (use "git add" and/or "git commit -a")

# コミット
$ git commit -am "テーマの設定"
```


## 公開ファイルの生成
公開用のファイルを生成する（記事なし。テーマ適用のみ）。  
publicディレクトリが作成され、公開用のファイル類が生成される。
```@bash
# 環境のトップで行うこと
$ cd ~/myblog

# 生成（-t テーマ名）
$ hugo -t masamune
Started building site
0 draft content
0 future content
0 pages created
0 non-page files copied
0 paginator pages created
0 tags created
0 categories created
in 12 ms
```


## publicディレクトリの中身をdocsディレクトリにコピー
GitHub Pagesの機能で公開するため、publicディレクトリの中身をdocsディレクトリにコピーする。  
これは記事作成後の生成時に必須の作業となる。
```@bash
# publicディレクトリの中身をdocsディレクトリにコピーする
# -p : コピー元の情報を引き継ぐ
# -f : 上書き等の問い合わせをしない
# -R : ディレクトリも
$ cp -p -f -R public/* docs

# コミット
$ git add .
$ git commit -m "公開用ファイル生成"
```


### 次回は、GitHubにリモートリポジトリを作成するところから。
