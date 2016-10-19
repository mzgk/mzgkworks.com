+++
categories = ["Usage"]
date = "2016-09-12T19:00:43+09:00"
description = "Macでシンボリックリンクを操作する方法。$ ln -s"
draft = false
slug = "mac-symbolic-link"
tags = ["Mac"]
title = "Macでシンボリックリンクを活用する : ln -s"

+++
Macで使えるシンボリックリンクの操作メモ。  
ディレクトリやファイルを別の場所で管理したいが、ディレクトリ構成上の制約がある時とかに便利。  

## シンボリックリンク
- 元ファイル・ディレクトリと同様の操作ができる
- エイリアスを違って ` $ cd ` も可能

## 作成
**絶対パス** が重要。  
` $ ln -s 元ファイル・ディレクトリの絶対パス リンク作成先のパス `

```@bash
# ~/dotsfiles/.editorconfigファイルのシンボリックリンクを
# ホームディレクトリに作成する
$ ln -s ~/dotsfiles/.editorconfig ~
```


## 確認
こんな感じで作成される。  
ファイル種類の「l」はシンボリックリンクを表す。  
```@bash
$ ls -la ~
...
lrwxr-xr-x    1 usr  staff     35  9 12 18:29 .editorconfig -> /Users/usr/dotsfiles/.editorconfig
...
```

## 削除
作成されたシンボリックリンクを削除する。  
` $ unlink シンボリックリンクのパス `
```@bash
# 削除
$ unlink ~/.editorconfig
```
