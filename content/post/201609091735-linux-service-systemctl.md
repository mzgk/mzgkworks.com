+++
categories = ["Usage"]
date = "2016-09-09T17:35:24+09:00"
description = "CentOS7でのサービスの操作方法。systemctlコマンド"
draft = false
slug = "linux-service-systemctl"
tags = ["Linux"]
title = "Linux（CentOS7）のサービス操作（systemctlコマンド）"

+++
## サービスの管理
サービス : OSから切り離し可能な、何らかの役割を持ったサブシステム。  
` $ systemctl サブコマンド サービス名 ` で操作を行う。  
CentOS 6までだと ` $ service ` コマンド。  

### 主なサブコマンド  
| サブコマンド | 説明                                               |
|:-------------|:---------------------------------------------------|
| start        | サービスを開始する                                 |
| stop         | サービスを停止する                                 |
| restart      | サービスを再起動する                               |
| enable       | システム起動時にサービスを自動開始する             |
| disable      | システム起動時にサービスが自動開始しないようにする |
| status       | サービスの状態を表示する                           |

### 主なサービス
| サービス名        | 説明                       |
|:------------------|:---------------------------|
| firewalld.service | ファイヤーウォールサービス |
| crond.service     | スケジュール処理サービス   |
| cups.service      | 印刷サービス               |
| postfix.service   | Postfix（メール）サービス  |
| rsyslog.service   | システムログサービス       |
| sshd.service      | SSHサーバー                |
| httpd.service     | Webサーバー                |


```@bash
# postfixサービスの状態を確認
$ systemctl status postfix.service
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: active (running) since 金 2016-09-09 16:17:13 JST; 1h 12min ago
...
...

# postfixサービスを停止
sudo systemctl stop postfix.service
[sudo] password for centuser:

# 確認
systemctl status postfix.service
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: inactive (dead) since 金 2016-09-09 17:31:09 JST; 17s ago
...

# 開始
# postfixサービスを停止
sudo systemctl start postfix.service
[sudo] password for centuser:

# 確認
systemctl status postfix.service
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: active (running) since 金 2016-09-09 17:32:26 JST; 33s ago
...
```
