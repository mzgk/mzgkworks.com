+++
categories = ["Setting"]
date = "2016-09-12T15:42:13+09:00"
description = "CentOS 7へのWebサーバー（Apache）のインストールと基本設定。"
draft = false
slug = "centos7-apache-install"
tags = ["Linux","Apache"]
title = "CentOS 7へのWebサーバー（Apache）のインストールと基本設定"

+++

CentOS7への、Webサーバー（Apache）のインストールから設定まで。

## Apacheのインストール
` $ yum `コマンドを使用して関連あるパッケージを含めてインストールを行う。  
```@bash
$ sudo yum install httpd
[sudo] password for centuser:
...
インストール:
  httpd.x86_64 0:2.4.6-40.el7.centos.4
...
完了しました!
```


## ドキュメントルート
Webで公開するトップディレクトリ。  
デフォルトでは、「/var/www/html/」。  
「/etc/httpd/conf/httpd.conf」ファイルからでも確認ができる。  
```@bash
# ドキュメントルートを確認（L119付近）
$ sudo vi /etc/httpd/conf/httpd.conf
...
#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "/var/www/html"
...
```


## 設定ファイル（/etc/httpd/conf/httpd.conf）
Apacheのメイン設定ファイル。  

主なディレクティブ  

| ディレクティブ | 説明                                       | デフォルト                     |
|:---------------|:-------------------------------------------|:-------------------------------|
| ServerRoot     | 設定ファイル等を配置するトップディレクトリ | /etc/httpd                     |
| Listen         | Apacheが待ち受けるポート番号               | 80                             |
| User           | Apacheの実行ユーザー                       | apache                         |
| Group          | Apacheの実行グループ                       | apache                         |
| ServerAdmin    | Apacheの管理者                             | root@localhost                 |
| ServerName     | Webサーバー名                              | #ServerName www.example.com:80 |
| DocumentRoot   | ドキュメントルート                         | /var/www/html                  |
| DirectoryIndex | インデックスファイル名                     | index.html                     |

### ServerRoot
設定ファイル等を配置するトップディレクトリ。  
httpd.confファイル内で相対パスを指定すると、このディレクトリが起点となる。

### Listen
Apacheが待ち受けるポート番号。

### User/Group
Apacheの実行ユーザーとグルー。  
Apacheが扱うコンテンツは、ここで指定されているユーザー・グループが利用できるアクセス権が必要。

### ServerAdmin
Apacheが稼働しているサーバーの管理者のメールアドレス。

### ServerName
Webサーバーの名前。  
デフォルトではコメントアウトされているので、解除して設定すること。  
:80のようにポート番号を指定できるが、省略してもよい。

### DocumentRoot
ドキュメントルートを **絶対パス** で設定する。  
このディレクトリ以下には、User/Groupディレクティブで指定したユーザー・グループがアクセスできる必要がある。

### DirectoryIndex
URLでファイル名まで指定されなかったときに、表示するファイル名を指定する。


## httpd.confファイルの構文チェック
` $ httpd -t ` コマンドで構文チェックをすることができる。  
```@bash
# /etc/httpd/conf/httpd.confファイルのチェック
$ httpd -t
Syntax OK
```


## Apacheの起動・自動起動設定
Apacheの起動と、次回からのシステム起動時に自動起動させる設定。  
Apacheを起動すると、複数のhttpdプロセスが生成される。  

```@bash
# 起動
$ sudo systemctl start httpd.service

# 自動起動設定
$ sudo systemctl enable httpd.service
```


## ファイヤウォールの設定
ファイヤウォールの設定を変更し、80番ポート（http）へのアクセスを許可させる。  
```@bash
# 設定
$ sudo firewall-cmd --permanent --add-service=http

# 更新
$ sudo firewall-cmd --reload
```


## ログ
- アクセスログ : /var/log/httpd/access_log
- エラーログ : /var/log/httpd/error_log

```@bash
# アクセスログ
$ sudo less /var/log/httpd/access_log

# エラーログ
$ sudo less /var/log/httpd/error_log
```
