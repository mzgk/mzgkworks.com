+++
categories = ["Usage"]
date = "2016-09-15T18:57:14+09:00"
description = "CentOS7のサービス管理を行うsystemctlコマンドの使い方。"
draft = false
slug = "centos7-systemctl-service"
tags = ["Linux"]
title = "CentOS7のsystemctlコマンドの使い方"

+++
CentOS 7のsystemdサービス管理のメモ。  
サービスの稼働状態や自動起動設定の確認・操作。　　


## UNITの状態
### 確認 : 一覧
` $ systemctl list-unit `

ACTIVE, SUBが実行状態を表している。  
```@bash
# UNITのタイプがServiceのみを確認
$ systemctl list-units -t service
UNIT                 LOAD   ACTIVE SUB     DESCRIPTION
auditd.service       loaded active running Security Auditing Service
chronyd.service      loaded active running NTP client/server
...
● kdump.service      loaded failed failed  Crash recovery kernel arming
```

### 確認 : 個別
` $ systemctl is-active [UNIT名]`

```@bash
$ systemctl is-active yum-cron
active
```

### 確認 : 個別詳細
` $ systemctl status [UNIT名] `

```@bash
$ systemctl status yum-cron
● yum-cron.service - Run automatic yum updates as a cron job
   Loaded: loaded (/usr/lib/systemd/system/yum-cron.service; enabled; vendor preset: disabled)
   Active: active (exited) since 火 2016-09-13 16:56:49 JST; 2 days ago
  Process: 644 ExecStart=/bin/touch /var/lock/subsys/yum-cron (code=exited, status=0/SUCCESS)
 Main PID: 644 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/yum-cron.service
```

### 操作（起動・停止・再起動・強制終了）
UNITの操作。  
` $ sudo systemctl [UNITコマンド] [UNIT名]`

| UNITコマンド | 説明           |
|:-------------|:---------------|
| start        | 起動させる     |
| stop         | 停止させる     |
| restart      | 再起動させる   |
| kill         | 強制終了させる |



## 自動起動設定
### 確認 : 一覧
` $ systemctl list-unit-files `

| STATE    | 説明                |
|:---------|:--------------------|
| enabled  | 自動起動 : 有効     |
| disabled | 自動起動 : 無効     |
| static   | 自動起動 : 設定不可 |

```@bash
# UNITのタイプがServiceのみを確認
$ systemctl list-unit-files -t service
UNIT FILE                                   STATE
arp-ethers.service                          disabled
auditd.service                              enabled
chrony-dnssrv@.service                      static
dbus-org.freedesktop.hostname1.service      static
...

# grepへパイプで繋いで「enabled」のみを表示させる
$ systemctl list-unit-files -t service | grep enabled
UNIT FILE                                   STATE
auditd.service                              enabled
chronyd.service                             enabled
...
tuned.service                               enabled
yum-cron.service                            enabled
```

### 確認 : 個別
` $ sudo systemctl is-enabled [Unit名] `

| 状態     | 説明           |
|:---------|:---------------|
| enabled  | 自動起動が有効 |
| disabled | 自動起動が無効 |

```@bash
# yum-cronの状態確認
$ sudo systemctl is-active yum-cron
enabled
```

### 操作（有効化・無効化）
` $ sudo systemctl [UNITコマンド] [UNIT名]`

| UNITコマンド | 説明              |
|:-------------|:------------------|
| enable       | 自動起動 : 有効化 |
| disable      | 自動起動 : 無効化 |
