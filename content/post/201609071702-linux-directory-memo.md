+++
categories = ["Knowledge"]
date = "2016-09-07T17:02:18+09:00"
description = "Linux（CentOS 7）のディレクトリと絶対・相対パスについてのメモ。"
draft = false
slug = "linux-directory-memo"
tags = ["Linux"]
title = "Linuxのディレクトリとパスについてのメモ"

+++
## 環境
- CentOS 7

## ディレクトリ
Linuxの主なディレクトリ。

| ディレクトリ名 | 使われ方                                                     |
|:---------------|:-------------------------------------------------------------|
| /              | ルートディレクトリ                                           |
| /bin           | 一般ユーザーが利用できるLinuxの基本コマンド                  |
| /boot          | Linuxカーネルなどの起動に必要なファイル類                    |
| /dev           | デバイスファイル                                             |
| /etc           | システム全体の設定ファイルなど                               |
| /home          | 一般ユーザーのホームディレクトリを格納                       |
| /lib           | 共有ライブラリーなど                                         |
| /lib64         |                                                              |
| /media         | 外付けHDDやUSBメモリなどをマウント                           |
| /mnt           | 一時的にファイルシステム等をマウント                         |
| /opt           | 追加パッケージや追加プログラムがインストールされる           |
| /proc          | カーネルやプロセスなどのシステム情報                         |
| /root          | rootユーザーのホームディレクトリ                             |
| /run           |                                                              |
| /sbin          | システム管理者が利用する基本コマンド                         |
| /srv           |                                                              |
| /sys           | プロセスに関係のないシステム情報                             |
| /tmp           | 一時的に使用するファイル                                     |
| /usr           | 一般ユーザーが使用するプログラムの実行ファイルや設定ファイル |
| /var           | ログファイルなどの頻繁に更新されるファイル                   |


## 絶対パス
ルートディレクトリを起点として、ファイルやディレクトリのパスを表す方法。  
ファイルやディレクトリの場所は一意になる。

## 相対パス
カレントディレクトリを起点として、ファイルやディレクトリのパスを表す方法。  
ファイルやディレクトリの場所は一意にならない。

## 相対パスの記述で使用可能な記号
| 記号 | 説明                           |
|:-----|:-------------------------------|
| .    | カレントディレクトリ（省略可） |
| ..   | １つ上の親ディレクトリ         |
| ~    | ホームディレクトリ             |

例 :  
カレントディレクトリ内のファイル → ./ファイル名 or ファイル名のみ  
カレントディレクトリと同レベルのディレクトリ → ../ディレクトリ名  
カレントディレクトリの親ディレクトリ → ..  
