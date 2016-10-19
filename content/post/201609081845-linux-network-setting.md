+++
categories = ["Usage"]
date = "2016-09-08T18:45:54+09:00"
description = "CentOS 7でのネットワークの確認と設定方法。IPアドレスやホスト名変更。"
draft = false
slug = "linux-centos7-network-setting"
tags = ["Network","Linux"]
title = "CentOS 7でのネットワーク設定・確認方法"

+++

## ネットワークインターフェース
CentOS 7では、ネットワークの処理を「NetworkManager」というサービスが担っている。  
NetworkManagerは ` nmcli `コマンドで管理をする。  

### ネットワークインターフェースの一覧表示
` $ nmcli device `で確認できる。  

```@bash
$ nmcli device
デバイス  タイプ    状態      接続
eth0      ethernet  接続済み  System eth0
eth1      ethernet  切断済み  --
eth2      ethernet  切断済み  --
lo        loopback  管理無し  --
```

### 詳細表示
` $ nmcli device show デバイス `で確認できる。  

```@bash
# eth0を詳細表示
$ nmcli device show eth0
GENERAL.デバイス:                        eth0
GENERAL.タイプ:                         ethernet
GENERAL.ハードウェアアドレス:               xx:xx:xx:xx:xx:xx
GENERAL.MTU:                          1500
GENERAL.状態:                          100 (接続済み)
GENERAL.接続:                          System eth0
GENERAL.CON パス:                      /xxx/NetworkManager/xxx
WIRED-PROPERTIES.キャリア:              オン
IP4.アドレス[1]:                        xxx.xx.xx.xxx/23 #IPアドレス
IP4.ゲートウェイ:                        xxx.xx.xx.x #デフォルトゲートウェイ
IP4.DNS[1]:                           xxx.xx.xx.xxx #DNSサーバー１
IP4.DNS[2]:                           xxx.xx.xx.xxx #DNSサーバー２
IP6.アドレス[1]:                        xx:xx:xx:xx:xx:xx/64
IP6.アドレス[2]:                        xx:xx:xx:xx:xx:xx/64
IP6.ゲートウェイ:                        xx::xx
IP6.DNS[1]:                           xx:xx:::xx
```

### 他には
以下でもネットワークインターフェースの確認が可能。  

- ` $ ifconfig `
- ` $ ip addr `（CentOS 7ではこちらが推奨）


## ホスト名の確認・変更
ホスト名の確認と変更をする方法。  

- ファイル : /etc/sysconfig/network
- ファイル（CnetOS 7） : /etc/hostname

```@bash
# 確認
$ hostname
centos7.sample.com

# 変更（root権限が必要）
$ nmcli general hostname 変更後のホスト名
#または
$ hostnamectl set-hostname 変更後のホスト名
```


## IPアドレスの設定
**VPSではしないこと。**  
` # nmcli connection modify "ネットワークインターフェース名" ipv4.address IPアドレス/サブネットマスクビット数 `

```@bash
# 固定IP（root権限）
$ nmcli connection modify "System eth0" ipv4.address 192.168.0.10/24

# DHCP（root権限）
$ nmcli c modify "System eth0" ipv4.address auto
```


## デフォルトDNSサーバーの設定
` # nmcli connection modify "ネットワークインターフェース名" ipv4.dns IPアドレス `

- ファイル : /etc/resolv.conf

```@bash
# Google提供のDNSサーバー8.8.8.8と8.8.4.4に変更（root権限）
$ nmcli connection modify "System eth0" ipv4.dns "8.8.8.8 8.8.4.4"

# 設定を反映
$ systemctl restart NetworkManager
```
