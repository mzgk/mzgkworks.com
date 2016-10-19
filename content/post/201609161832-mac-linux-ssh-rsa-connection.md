+++
categories = ["Setting"]
date = "2016-09-16T18:32:16+09:00"
description = ""
draft = false
slug = "mac-linux-ssh-rsa-connection"
tags = ["Mac","Linux"]
title = "MacからLinux（CentOS7）にSSH公開鍵認証で接続する"

+++

MacからSSHの公開鍵認証でLinux（CentOS7）に接続する方法。


## Linux : 公開鍵の設置場所作成
Linux側で公開鍵ファイルを設置する場所を作成しておく。  
```@bash
# ホームディレクトリ直下に.sshディレクトリを作成
$ mkdir ~/.ssh

# パーミッションを700に変更
$ chmod 700 ~/.ssh
```


## Mac : 公開鍵と秘密鍵の作成
Mac側で ` $ ssh-keygen `コマンドを使って、公開鍵と秘密鍵ファイルを生成する。  
```@bash
# SSH2-rsa形式で作成
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/[ユーザー名]/.ssh/id_rsa):

# 作成したい場所とファイル名をフルパスで入力する
# 未入力の場合は、/Users/[ユーザー名]/.ssh/に以下が生成される
# id_rsa : 秘密鍵
# id_rsa.pub : 公開鍵

# 任意のパスフレーズを入力する
Enter passphrase (empty for no passphrase):
Enter same passphrase again:

# 作成が完了したか確認
$ ls -la ~/.ssh
-rw-------   1 xxxx  staff  1766  9 16 17:06 id_rsa
-rw-r--r--   1 xxxx  staff   402  9 16 17:06 id_rsa.pub

# 公開鍵ファイルのパーミッションを変更（所有者のみrw）
$ chmod 600 ~/.ssh/id_rsa.pub

```


## Mac : SSH接続の設定
SSH設定ファイル（~/.ssh/Config）に接続情報を追加する。  
ssh接続時にHOSTに設定した引数で接続ができるようになる。  
```@bash
HOST 任意の名前（sshコマンドの引数として入力する）
  HostName 接続先LinuxのIP
  User Linuxログインユーザー名
  Port Linux側SSHポート番号
  IdentityFile Mac内の秘密鍵ファイルのフルパス
  ServerAliveInterval 60 # 60秒ごとに接続先にmsg送信
  # これは接続が切れないようにするため
```


## 公開鍵ファイルの転送
Macから接続先のLinuxに` $ scp `コマンドを使って、公開鍵ファイルを転送する。  
その際に、公開鍵ファイルの名前を「authorized_keys」に変更する。  
```@bash
$ scp -P [SSHポート番号] [公開鍵フルパス] [ユーザー名]@[LinuxのIP]:~/.ssh/authorized_keys
xxx@xxx's password: # Linuxのユーザーパスワード
id_rsa.pub                                                                    100%  402     0.4KB/s   00:00
```


## 接続
Mac側から接続。  
```@bash
$ ssh [HOSTに定義した名前]

# 秘密鍵を作成した際のパスフレーズ入力のダイアログが表示される
```


## Linux : SSHパスワード認証を禁止する
SSHサーバーの設定を変更し、パスワード認証を無効にする。  
```@bash
$ sudo vi /etc/ssh/sshd_config

PassswordAuthentication yes → noに変更

# SSHサービスの設定を再読み込みする
$ sudo systemctl reload sshd.service
```
