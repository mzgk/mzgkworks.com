+++
title = "Golangの環境をMacに構築する"
tags = ["Golang"]
draft = false
description = "MacへGo言語をインストールしてGOPATHを設定する。"
date = "2017-01-26T18:00:25+09:00"
categories = ["Setting"]
slug = "golang-install-mac"

+++

## 環境
- MacBook Pro (15-inch, Mid 2012)
- macOS Sierra 10.12.3
- Homebrew 1.1.8


## Goインストール
Homebrewを利用してインストール

```
# Homebrewのメンテ
$ brew update
$ brew upgrade
$ brew doctor
Your system is ready to brew.


# Golangのインストール
$ brew install go
==> Downloading https://homebrew.bintray.com/bottles/go-1.7.4_2.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring go-1.7.4_2.sierra.bottle.tar.gz
==> Caveats
As of go 1.2, a valid GOPATH is required to use the `go get` command:
  https://golang.org/doc/code.html#GOPATH

You may wish to add the GOROOT-based install location to your PATH:
  export PATH=$PATH:/usr/local/opt/go/libexec/bin
==> Summary
🍺  /usr/local/Cellar/go/1.7.4_2: 6,438 files, 250.7M
```


## インストールの確認
```
$ go version
go version go1.7.4 darwin/amd64

$ go env GOROOT
/usr/local/Cellar/go/1.7.4_2/libexec
```


## GOPATHの設定
- Go言語で開発するためのワークスペース
- Goが外部のライブラリが格納されている場所を知るために必要
- 今回は「$HOME/Development/Go」
- 設定は必須
- ~/.bash_profileに設定する

```
# ~/.bash_profileを開く
$ vim ~/.bash_profile


# 以下を追記する
export GOPATH=$HOME/Development/Go
export PATH=$PATH:$GOPATH/bin


# 保存後に反映
$ source ~/.bash_profile
```
