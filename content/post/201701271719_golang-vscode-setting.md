+++
categories = ["Setting"]
slug = "golang-vscode-setting"
title = "Visual Studio CodeにGolang向けの開発デバッグ環境を設定する"
tags = ["Golang"]
draft = false
description = "VSCodeにGolangの開発・デバッグ環境を設定する"
date = "2017-01-27T17:19:47+09:00"

+++

Visual Studio CodeにGolang向けの開発環境の設定をする。  
補完・デバッグ実行までできるのは便利。  

1. VSCodeのインストール
2. Golangの拡張機能をインストール
3. Golangのデバッグ実行ができるようにする
4. おまけ


## 環境
- MacBook Pro (15-inch, Mid 2012)
- macOS Sierra 10.12.3
- Golang 1.7.4 darwin/amd64
- Visual Studio Code 1.8.1


## 1. Visual Studio Codeのインストール
### インストール
- 公式 : https://code.visualstudio.com/  
- Mac版をダウンロードしてインストール

### VSCodeをターミナルから起動できるように設定する
- 公式 : https://code.visualstudio.com/docs/setup/mac  
- VSCode起動 -> Shift+Cmd+P -> Shell Command: Install 'code' command in PATH command.  
- ターミナルを起動  
- `$ code` でVSCodeが起動するか確認


## 2. Golangの拡張機能をインストール
- https://marketplace.visualstudio.com/items?itemName=lukehoban.Go
- 必要パッケージが不足している場合は、VSCodeの上部からInstallを促すダイアログが都度表示される
- もしくは、右下にメッセージ「Analysis Tools Missing」が表示されるので、クリックして不足分をインストール


## Golangのデバッグ実行を可能にする
### Delve公式サイト
- https://github.com/derekparker/delve/blob/master/Documentation/installation/osx/install.md
- 公式サイトにHomebrewでいけるとあったけど、ダメだった（証明書作成とHomebrew領域へのインストールはされた）
- たぶん$GOPATH配下に生成されたdlvを移さないとダメだったのかも（未検証）

### 基本的にはここを参照
- http://dev.classmethod.jp/go/visual-studio-code-golang-debug/

### 証明書の作成
- MacでDELVEを使う場合は証明書の作成が必要とのことなので、KeyChainで証明書を作成
- 証明書名はDELVEインストール時に使用する（今回はdlv-certで作成）。
- http://dev.classmethod.jp/go/visual-studio-code-golang-debug/

### DELVEのインストール
- ここも上記に同じ。もしくは公式サイトの「Manual install -> 2)Install the binary」を参照
- MacにXcodeがインストールされていない場合は、Command Line Toolのインストールが必要かも

```
# 必要に応じて
$ xcode-select --install

# ディレクトリ作成と移動
$ mkdir $GOPATH/src/github.com/derekparker && cd $GOPATH/src/github.com/derekparker

# GitHubからcloneして、移動
$ git clone https://github.com/derekparker/delve.git && cd delve

# 証明書と紐付けてmake。dlv-certは証明書の名前
$ CERT=dlv-cert make install

# KeyChainの変更を有効化
$ sudo kill `pgrep taskgated`
```

### VSCodeでのデバッグ
- デバッグをしたい場合は、プロジェクトのフォルダーを指定する必要あり
- フォルダーを作成 -> VSCodeで開く -> main.goファイルを作成の流れ
- ブレークポイント張って、F5で実行


## おまけ
### VSCode内でターミナルを開く
- メニューバー -> 表示 -> 統合ターミナルでターミナルを表示できる
- 使用するターミナルは、基本設定 -> ユーザー設定 -> 外部ターミナルで変更ができる
- "terminal.external.osxExec": "iTerm.app"

### VSCodeの使い方
- [@IT 特集：Visual Studio Code早分かりガイド](http://www.atmarkit.co.jp/ait/subtop/features/dotnet/all.html#xe789b9e99b86efbc9aVisualStudioCodee697a9e58886e3818be3828ae382ace382a4e38389)
