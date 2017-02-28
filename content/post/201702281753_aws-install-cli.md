+++
slug = "aws-install-cli"
title = "AWS CLIのインストール方法（Mac / Windows）"
tags = ["AWS"]
draft = false
description = "MacやWindowsにAWS CLI（コマンドラインインターフェイス）をインストールする方法。"
date = "2017-02-28T17:53:18+09:00"
categories = ["Setting"]

+++

AWS CLI（コマンドラインインターフェイス）をインストールする方法。

## 環境
- macOS Sierra 10.12.3
- Windows10


## Mac
- Homebrewを使ってインストールする

```
# Homebrewアップデート
$ brew update

# Homebrewに存在するか確認
$ brew search awscli
awscli

# インストール
$ brew install awscli
...
==> Summary
🍺  /usr/local/Cellar/awscli/1.11.55: 3,780 files, 32.7M

# 確認
$ aws --version
aws-cli/1.11.55 Python/2.7.10 Darwin/16.4.0 botocore/1.5.18

# 設定
$ aws configure
AWS Access Key ID [None]: accessKeys.csvの値
AWS Secret Access Key [None]: accessKeys.csvの値
Default region name [None]: ap-northeast-1
Default output format [None]: json
```


## Windows
- アマゾンWebサービスのツール : https://aws.amazon.com/jp/tools/
- コマンドラインツール -> インストール
- 左ペイン : Installing the AWS CLIをクリック
- 左ペイン : Windowsをクリック
- [Download the AWS CLI MSI installer for Windows (64-bit)](http://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)
- AWSCLI64.msiがダウンロードされる
- ダブルクリックしてインストール
- 完了後に再起動
- コマンドプロンプトから `> aws configure` で初期設定を行う


## AWS CLIリファレンス
リファレンスの場所は以下。  

http://docs.aws.amazon.com/cli/latest/reference/
