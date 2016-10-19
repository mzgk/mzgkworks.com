+++
categories = ["Setting"]
date = "2016-09-12T16:28:46+09:00"
description = "ApacheにBasic認証を設定する方法。"
draft = false
slug = "apache-basic-auth"
tags = ["Apache"]
title = "ApacheにBasic認証を設定する"

+++
CentOS7・Apacheで基本認証（Basic認証）を設定する方法。  
基本認証は、あらかじめApacheにユーザー名とパスワードを登録しておき、特定のディレクトリ以下にアクセスがあれば認証を求めるという仕組み。  

## 環境
- Apache : Apache/2.4.6 (CentOS)

## 基本認証とダイジェスト認証
ユーザー名・パスワードの入力を戻る方法としては、以下の２種類が存在する。  

- 基本認証 : ユーザー・パスワードのデータを平文で送信
- ダイジェスト認証 : MD5で暗号化して送信
- 安全性 : ダイジェスト認証　＞　基本認証


## 基本認証の追加
### 登録ファイルの作成
` htpasswd -c ファイル名 ユーザー`コマンドで、ユーザーとパスワードを登録するファイルを作成する。  
```@bash
# 登録するファイルを作成する
$ sudo htpasswd -c /etc/httpd/conf.d/htpasswd webuser
New Password:
...
Adding password for user webuser
```

### 権限変更
作成された登録ファイルの権限をapacheユーザー・グループのみアクセス可能なように変更する。  
```@bash
# 所有権
$ sudo chown apache:apache /etc/httpd/conf.d/htpasswd

# アクセス権
$ sudo chmod 600 /etc/httpd/conf.d/htpasswd
```

### 認証ディレクトリの設定
基本認証を適用するディレクトリの設定を行う。  
設定を記述するファイルのファイル名は任意。  
```@bash
$ sudo vi /etc/httpd/conf.d/auth.conf

# 以下を追加（適用するディレクトリがドキュメントルートの場合）
<Directory "/var/www/http">
  AuthType Basic
  AuthName "Private Aera"
  AuthUserFile /etc/httpd/conf.d/htpasswd
  Require valid-user
</Directory>
```

設定内容  

| ディレクティブ | 説明                                                      |
|:---------------|:----------------------------------------------------------|
| AuthType       | BASICを指定すると基本認証                                 |
| AuthName       | 認証名（認証ウィンドウの表示）                            |
| AuthUserFile   | パスワードファイル名                                      |
| Require        | 認証ユーザー（valid-user : ファイルに書かれた全ユーザー） |

### Apache設定ファイルの再読み込み
Apacheをリロードして、設定ファイルを再読み込みさせる。  
```@bash
$ sudo systemctl reload hhtpd.service
```


### URLアクセス時
これで、アクセス時に「ユーザー」「パスワード」を求めるダイアログが表示される。  
認証が成功した場合に、アクセスしたページが表示される。
