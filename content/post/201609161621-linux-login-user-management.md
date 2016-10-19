+++
categories = ["Usage"]
date = "2016-09-16T16:21:06+09:00"
description = "LinuxでrootユーザーのSSHログイン禁止や確認の方法。"
draft = false
slug = "linux-login-user-management"
tags = ["Linux"]
title = "Linuxでのログインユーザの確認や制御の方法"

+++
Linuxにはrootユーザーが必ず存在するため、外部からのログインは一般ユーザーのみに限定し、rootユーザーを禁止する。

## SSHでのrootログインの禁止
/etc/ssh/sshd_configファイルを以下に変更。  
```@bash
PermitRootLogin no
```


## ログイン可能なユーザーを限定
/etc/ssh/sshd_configファイルに以下を追記。  
```@bash
# AllowUsers [ユーザー名]
AllowUsers centuser
```


## （おまけ）SSHポートの変更
/etc/ssh/sshd_configファイルを以下に変更。  
```@bash
# Port [ポート番号]
Port 10022
```


## 変更後はSSHサーバーの再起動
設定ファイルの変更後は、SSKサーバーを再起動させる。  
```@bash
$ sudo systemctl restart sshd.service
```


## 過去のログイン情報を確認する
` $ last `

```@bash
$ last
centuser pts/0        219.118.174.90   Fri Sep 16 15:59   still logged in
centuser pts/0        219.118.174.90   Fri Sep 16 15:54 - 15:54  (00:00)
centuser pts/1        219.118.174.90   Thu Sep 15 19:20 - 19:35  (00:15)
centuser pts/0        219.118.174.90   Thu Sep 15 16:06 - 21:01  (04:54)
centuser pts/1        219.118.174.90   Tue Sep 13 17:34 - 19:50  (02:16)
centuser pts/0        219.118.174.90   Tue Sep 13 16:57 - 19:08  (02:11)
reboot   system boot  3.10.0-327.28.3. Tue Sep 13 16:56 - 16:16 (2+23:19)
centuser pts/0        219.118.174.90   Tue Sep 13 16:48 - 16:56  (00:07)
...
```


## ユーザーごとの最終ログインを確認する
ユーザーごとの最終ログインを表示することによって、覚えのないユーザーのログインを発見できる。  
` $ lastlog `

```@bash
$ lastlog
ユーザ名         ポート   場所             最近のログイン
root             pts/0                     金  9月 16 15:54:17 +0900 2016
bin                                        **一度もログインしていません**
...
centuser         pts/0    219.118.174.90   金  9月 16 15:59:23 +0900 2016
apache                                     **一度もログインしていません**
mysql                                      **一度もログインしていません**
```
