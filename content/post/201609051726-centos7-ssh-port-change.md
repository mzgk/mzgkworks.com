+++
categories = ["Setting"]
date = "2016-09-05T17:26:03+09:00"
description = "CentOS 7でSSHのポート番号を変更する方法。SELinux・firewalld対応。"
draft = false
slug = "cetos7-ssh-port-change"
tags = ["Linux"]
title = "CentOS 7でSSHのポート番号を変更する（SELinux・firewalld）"

+++

CentOS 7でSSHのポート番号を変更する方法のメモ。  
CentOS 7から、ファイヤウォールの設定が **iptables → firewalld** に変更となった。  
SELinuxが有効で、iptables → firewalld（FireWall）での内容。  
ポート番号は例。


## 環境
- さくらVPS
- CentOS 7


## SSH設定ファイル
SSHサーバーの設定ファイル `/etc/ssh/sshd_config` を変更する。  
rootユーザーに切替えて操作（切替え後の$は#に読替え）。

```@bash
# rootユーザーに切り替え
$ su -

$ vi /etc/ssh/sshd_config
```

| 変更場所 | 変更前               | 変更後             |
|:---------|:---------------------|:-------------------|
| L17      | #Port 22             | Port 10055         |
| L49      | #PermitRootLogin yes | PermitRootLogin no |


## SELinux
SELinuxの設定変更コマンドをインストールする。
```@bash
# 存在確認
$ rpm -q policycoreutils-python

# インストール
$ yum -y install policycoreutils-python

# SSHポート（10055）の許可
$ semanage port -a -t ssh_port_t -p tcp 10055

# 確認
$ semanage port -l | grep ssh
ssh_port_t                     tcp      10055, 22
```


## FireWall
外部から10055ポートへのアクセスを許可する。  
ひな形からコピーして編集を行う。

```@bash
# コピー
$ cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/

# viで編集
$ vi /etc/firewalld/services/ssh.xml

<port protocol="tcp" port="22"/>
↓
<port protocol="tcp" port="10055"/>
```


## 反映
FireWallとSELinuxに変更を反映する。

```@bash
# FireWall
$ firewall-cmd --reload

# SSHサーバー再起動
$ systemctl restart sshd.service
```


## クライアントからの接続
MacのTerminal.appから接続する。

```@bash
$ ssh -p 10055 ユーザー名@IPアドレス
```

以上
