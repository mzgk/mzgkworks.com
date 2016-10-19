+++
categories = ["Usage"]
date = "2016-09-15T19:08:29+09:00"
description = "Linuxで開いているポートを確認する方法。netstat, ss"
draft = false
slug = "linux-port-confirm"
tags = ["Linux"]
title = "Linuxで開いているポートを確認する方法"

+++
開いているポートの確認方法。  
<br>
ポートはサービス（ソフトウェア）が使用し、そのポートを通る通信をFireWallが監視（制御）している。  
なので、サービスの登録（ソフトのインストール）をするとポートが使用され、FireWallでそのポートを開放してやると、外部との通信が可能になる。

## netstat（ss）
` $ netstat `　または ` $ ss `コマンドで、現在開いているポートを一覧表示することができる。  

| オプション | 説明                                     |
|:-----------|:-----------------------------------------|
| -l         | Listenしているポートのみ表示             |
| -t         | TCPを表示                                |
| -u         | UDPを表示                                |
| -n         | ポートやホストを数値で表示               |
| -p         | ポートを開いているプロセスを表示（sudo） |
| -4         | IPv4のみ                                 |
| -6         | IPv6のみ                                 |

```@bash
# 開いているポートと使用しているプロセス（IPv4）
$ sudo netstat -ltup4
sudo netstat -ltup4
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:mysql           0.0.0.0:*               LISTEN      2015/mysqld
tcp        0      0 localhost:smtp          0.0.0.0:*               LISTEN      2282/master
tcp        0      0 0.0.0.0:10022           0.0.0.0:*               LISTEN      1274/sshd
udp        0      0 localhost:323           0.0.0.0:*                           658/chronyd

# 上記を数値で
sudo netstat -ltunp4
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      2015/mysqld
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      2282/master
tcp        0      0 0.0.0.0:10022           0.0.0.0:*               LISTEN      1274/sshd
udp        0      0 127.0.0.1:323           0.0.0.0:*                           658/chronyd
```


## 例えば
例えばhttps通信を許可してやるには、  

- mod_sslをインストール
  - $ sudo yum install mod_ssl
- SSLの設定
  - /etc/httpd/conf.d/sl.conf
- Apaceを再起動する
  - $ sudo systemctl restart httpd
- ポート状態を確認する
  - $ sudo netstat -ltupn
  - 443がLISTEN状態になっているか？
- FireWallにサービスの許可を追加
  - $ sudo firewall-cmd --add-service=https


## 関連
[ネットワークの基礎知識（プロトコル・IPアドレス・サブネットマスク・ポート）](http://mzgkworks.com/post/network-protocol-ipadress/)
