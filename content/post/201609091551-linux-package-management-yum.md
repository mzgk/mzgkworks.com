+++
categories = ["Usage"]
date = "2016-09-09T15:51:31+09:00"
description = "Linuxのパッケージ管理システムについて。RPM系のYUM。"
draft = false
slug = "linux-package-management-yum"
tags = ["Linux"]
title = "Linuxのパッケージ管理システム（YUM）"

+++
## パッケージ
- Linuxでは「パッケージ」という単位でソフトウェアを管理している
- 実行プログラム・設定ファイル・ドキュメントなどが格納されている
- 形式はディストリビューションによって複数存在するが、互換性はない
  - RPM : Red Hat系（RHEL、CentOS、Fedora、openSUSEなど）
  - deb : Debian系（Debian GNU/Linux、Ubuntuなど）
- パッケージ間で依存関係がある
- 手動で依存関係を管理するのは大変なので、通常は「パッケージ管理システム」を使う
  - RPM系 : YUM
  - deb系 : APT


## YUM
CentOSのRPMパッケージを管理するパッケージ管理システム。  
主な機能としては  

- 面倒な依存関係等を制御
- パッケージをインターネット上で検索
- 必要なパッケージをインターネット上から自動でダウンロード

### yumコマンド
` $ sudo yum サブコマンド パッケージ名`  

| サブコマンド         | 説明                                             |
|:---------------------|:-------------------------------------------------|
| update               | システム全体をアップデートする                   |
| install パッケージ名 | 指定したパッケージをインストールする             |
| remove パッケージ名  | 指定したパッケージをアンインストールする         |
| update パッケージ名  | 指定パッケージをアップデートする                 |
| list                 | パッケージを一覧表示する（未インストールも含む） |
| info パッケージ名    | 指定したパッケージの情報を表示する               |
| search "キーワード"  | 指定したキーワードでパッケージを検索する         |


## システムの自動アップデート
yumコマンドを使ったアップデートを自動で行う設定。  
「yum-cron」というパッケージを使用する。  

### インストール
```@bash
$ sudo yum install yum-cron
[sudo] password for centuser:
...
...
```

### 設定
` /etc/yum/yun-cron.conf ` ファイルの編集を行う。  

- 自動更新する内容
  - 全部・セキュリティーのみなど
- 自動更新するか
  - する・しない

```@bash
[commands]
#  What kind of update to use:
# default                            = yum upgrade
# security                           = yum --security upgrade
# security-severity:Critical         = yum --sec-severity=Critical upgrade
# minimal                            = yum --bugfix update-minimal
# minimal-security                   = yum --security update-minimal
# minimal-security-severity:Critical =  --sec-severity=Critical update-minimal
update_cmd = default  # ← 範囲を指定（default 〜 minimal-security-severity:Critical）

...
...

# Whether updates should be applied when they are available.  Note
# that download_updates must also be yes for the update to be applied.
apply_updates = no    # ← yes / no : するしない
```

### 実行と次回自動起動
yum-cronサービスの起動と、次回からのシステム起動時に自動起動するように設定する。  
```@bash
# yum-cron.serviceの起動
$ sudo systemctl start yum-cron.service

# システム起動時の自動起動設定
$ sudo systemctl enable yum-cron.service
```
