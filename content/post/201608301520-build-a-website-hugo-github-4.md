+++
categories = ["Setting"]
date = "2016-08-30T15:20:03+09:00"
description = "Hugo,GitHub Pages,独自ドメインで静的サイトを構築する。一連の作業を自動化する。"
draft = false
slug = "build-a-blog-in-hugo-4"
tags = ["Blog","Hugo"]
title = "Hugo・GitHub Pages・独自ドメインでブログを構築 : 4"

+++
手順通りにコマンドを叩いていくのが面倒なので、スクリプトにまとめる。  
~/.bash_profile または ~/.bash_rcに記述しておく。

- 記事作成〜確認
- コミット〜公開

## 記事ファイルの作成〜確認
記事ファイルを作成しATOMで起動〜ローカルサーバ起動し確認ができる環境までを用意する。  
-t テーマ名は、config.tomlに設定しているので省略している。  
` $ hugow ファイル名 ` で実行する（ファイル名はprefixとして日時が自動挿入される）。
```@bash
# $ hugow ファイル名
# フロー : ファイル作成 → Chrome起動 → ローカルサーバ起動
function hugow() {
  echo -e "\033[0;32m***ファイル作成・Chrome起動・サーバ起動***\033[0m"
  # 作業ディレクトリに移動（自分の環境に合わせる）
  command cd ~/Develop/Web/mzgkworks.com
  # content/post/YYYYMMDDHHMM_引数.md を作成し、ATOMを起動
  command hugo new post/$(date +%Y%m%d%H%M)_$*.md --editor="atom"
  # chromeを起動しローカルホストを表示（確認用）
  command open -a "/Applications/Google Chrome.app" http://localhost:1313
  # ローカルサーバーを起動（ライブリロード）
  command hugo server -D -w
}
```


## コミット〜公開（プッシュ）
コミット〜プッッシュして公開までを行う。  
` $ hugop コミットメッセージ ` で実行する。
```@bash
# $ hugop コミットメッセージ
# フロー : コミット → プッシュ
function hugop() {
  echo -e "\033[0;32m***add -> commit -> push***\033[0m"
  # 作業ディレクトリに移動
  command cd ~/Develop/Web/mzgkworks.com
  # ファイル生成
  command hugo
  # public → docsディレクトリにコピー
  command cp -p -f -R public/* docs
  # add
  command git add .
  # 引数（途中の空白文字可）をメッセージにしてcommit
  command git commit -m "$*"
  # push master
  command git push origin master
}
```


### 以上
GitHub Pagesがmasterブランチのdocsディレクトリで使えるようになったので、かなり楽になった。
