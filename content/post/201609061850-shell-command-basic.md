+++
categories = ["Usage"]
date = "2016-09-06T18:50:36+09:00"
description = "シェルでコマンドを打つ際の基本的なこと。リダイレクトや連結など。"
draft = false
slug = "shell-command-basic"
tags = ["bash"]
title = "シェルでコマンドを打つ際の基本的な機能"

+++

bashでコマンドを打つ際の基本的な操作のメモ。

## 入力
### 補完（コマンド・ファイル名）
- tabキー : 補完
- tabキー２回 : 候補の一覧を表示

### 履歴
- Ctrl + P or ↑ : 新しいものから、古いものへ
- Ctrl + N or ↓ : 古いものから、新しいものへ

### インクリメンタル検索
- Ctrl + P : 開始
- Ctrl + C : 終了


## パイプ
コマンドの結果を別のコマンドへ渡す。  
結果が膨大で画面がスクロールしてしまう時に、lessコマンドに渡して１ページづつ確認するなど。  
```@bash
# ファイル一覧と行数・単語数・バイト数を表示するコマンドを連携し
# ファイル数を数える
$ ls | wc -l

# /etcディレクトリのファイル一覧をlessコマンドで１ページづつ表示
$ ls -l /etc | less
```


## リダイレクト
コマンドの出力結果をファイルに出力する。  
もしくはファイルの内容をコマンドに送る。  

| リダイレクト | 内容             |
|:-------------|:-----------------|
| >            | 出力（上書き）   |
| >>           | 出力（追加）     |
| <            | 入力             |
| <<           | 終端文字列の指定 |

```@bash
# > : 上書き
$ date > today.txt

# >> : 末尾に追加
$ date >> today.txt

# < : sample.txtの中身から"Linux"を検索
$ grep "Linux" < sample.txt

# << : 入力終端文字列を指定し、複数行のファイルを作成する
$ cat << _END_ > result.txt
> test1
> test2
> _END_

$ cat result.txt
test1
test2
```

### 標準エラー出力のみをファイルへ
処理の記録からエラーメッセージだけを除外する時など。  
```@bash
# 通常
$ ls -l today.txt nofile.txt
ls: nofile.txt: No such file or directory
-rw-r--r--  1 mzgk  staff  96  9  6 18:23 today.txt

# エラーのみerror.txtへ
$ ls -l today.txt nofile.txt 2> error.txt
-rw-r--r--  1 mzgk  staff  96  9  6 18:23 today.txt

# error.txtの中身
$ cat error.txt
ls: nofile.txt: No such file or directory
```


## 連結
複数のコマンドを連結して使う場合。

| 連結         | 内容                                               |
|:-------------|:---------------------------------------------------|
| ;            | コマンド１の実行後、コマンド２を実行する           |
| &            | コマンド１の完了を待たず、コマンド２を実行する     |
| &&           | コマンド１が正常終了したなら、コマンド２を実行する |
| &#124;&#124; | コマンド１が失敗した場合、コマンド２を実行する     |



## メタキャラクタ
ファイル名の指定などに利用する特殊な文字。  

| メタキャラクタ | 説明                     |
|:---------------|:-------------------------|
| *              | ０文字以上の任意の文字列 |
| ?              | 任意の１文字             |
| []             | []内の任意の１文字       |

```@bash
# *.txt : .txt全て
$ ls *.txt

# 201?.txt : 201+１文字の.txt
$ ls 201?.txt
2016.txt  201a.txt

# 201??.txt : 201+２文字の.txt
20160.txt  201ab.txt

# [ab]*.txt : aまたはbで始まる.txt
$ ls [ab]*.txt
a1.txt  a200.txt  aA.txt  b.txt  bcd.txt

# [1-3].txt : 1〜3の範囲の.txt
$ ls [1-3].txt
1.txt  2.txt  3.txt

# *[24]*.txt : ファイル名に2か4が含まれる.txt
$ ls *[24]*.txt
2.txt  a2.txt a4bc.txt
```
