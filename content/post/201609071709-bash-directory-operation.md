+++
categories = ["Usage"]
date = "2016-09-07T17:09:45+09:00"
description = "bashでディレクトリ・ファイルの操作を行う。コピー・移動・削除など"
draft = false
slug = "bash-directory-operation"
tags = ["bash"]
title = "bashでディレクトリ・ファイルのコピー・移動・削除を行う"

+++

## cd : ディレクトリ移動
引数で指定したディレクトリへ移動する。  
他に便利な使い方としては、  
```@bash
# 引数なし : ホームディレクトリに移動
$ cd

# - : １つ前の作業ディレクトリへ移動
# ２つのディレクトリを交互に操作する際に便利
$ cd -
```


## コピー
ファイルまたはディレクトリをコピーする。  
```@bash
# A.txtをB.txtの名前でコピーする
$ cp A.txt B.txt

# DirディレクトリにA.txtの名前でコピーする。
# Dir部分を「.」にすると、カレントディレクトリを表す。
$ cp A.txt Dir

# A.txtをDirディレクトリにB.txtとしてコピーする。
$ cp A.txt Dir/B.txt

# ディレクトリをコピーする際には「-r」が必要
# A_DirディレクトリをB_Dirディレクトリ配下にコピー
$ cp -r A_Dir B_Dir

# A_DirディレクトリをB_Dirディレクトリ配下にC_Dirディレクトリとしてコピー
# B_Dir配下にC_Dirが存在していない場合
$ cp -r A_Dir B_Dir/C_Dir

# A_Dirディレクトリの中身だけを、B_Dir配下にコピーする
$ cp -r A_Dir/ B_Dir
```


## 移動とリネーム
移動とリネームを行う。ファイルとディレクトリは同様に実行可能。
```@bash
# A.txtをB.txtにリネームする（同じディレクトリ内）
$ mv A.txt B.txt

# A.txtをA_Dirディレクトリに移動する
$ mv A.txt A_Dir

# A.txtをA_DirディレクトリにB.txtとして移動する
$ mv A.txt A_Dir/B.txt
```


## ディレクトリの作成
ディレクトリを作成する。
```@bash
# 単独
$ mkdir A_Dir

# 複数階層分
$ mkdir -p A_Dir/B_Dir
```


## 削除
ファイルとディレクトリの削除。  
**ゴミ箱はない。**
```@bash
# 確認つきでファイルを削除する（yで削除）
$ rm -i A.txt

# ディレクトリの中身ごと削除する
$ rm -r A_Dir
```
