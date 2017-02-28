+++
categories = ["Usage"]
slug = "aws-automatically-create-snapshot-of-ec2"
title = "EC2のスナップショットを自動作成する"
tags = ["AWS"]
draft = false
description = "AWS EC2のスナップショットを定期的に自動で作成する方法。"
date = "2017-02-28T16:28:32+09:00"

+++

EC2のスナップショットを自動（AWS CLI + Cron）で採取するための設定方法。  

## 手順
1. アクセス権（EC2FullAccess）があるIAMユーザーで、AWS CLIをセットアップ
2. EC2上でawsコマンドを実行して、スナップショットが作成されていることを確認
3. awsコマンドをシェルスクリプトファイルにして、EC2のcrontabに設定


## 1. AWS CLIのセットアップ
- IAMダッシュボードで、使用するIAMユーザーを選択
- 認証情報タブ -> アクセスキーの作成 -> 新規アクセスキーが作成され、.csvのダウンロードができる
- EC2にsshでログイン

```
# EC2（Amazon Linux）上で、AWS CLIの初期設定
$ aws configure
AWS Access Key ID [None]: .csvのAccess key ID
AWS Secret Access Key [None]: .csvのSecret access key
Default region name [None]: ap-northeast-1
Default output format [None]: json
```


## 2. EC2のスナップショットを採取確認
- EC2ダッシュボードで対象インスタンスの情報を確認する
  - EC2.Name : 対象のEC2インスタンス
  - EBS.ID : vol-xxxxxxxxxc0c
- 対象のEC2にssh接続をする
- 以下のawsコマンドを実行する
- コマンド実行後に、EC2ダッシュボードでスナップショットを確認すると作成を実行している

```
# スナップショットを作成する
$ aws ec2 create-snapshot --volume-id vol-xxxxxxxxxc0c --description "説明"

# 結果が返ってくる
{
    "Description": "入力した説明",
    "Encrypted": false,
    "VolumeId": "vol-xxxxxxxxxc0c",
    "State": "pending",
    "VolumeSize": 8,
    "Progress": "",
    "StartTime": "2017-02-28T01:39:43.000Z",
    "SnapshotId": "snap-021be12d0fc9abf3a",
    "OwnerId": "xxxxxx"
}
```


## 3. crontabを設定
### 採取コマンドをシェルスクリプト化
- backup.shというファイル名で保存する

```
#!/bin/sh
aws ec2 create-snapshot --volume-id vol-xxxxxxxxxc0c --description "説明"
```

### crontab.txtを作成
- 毎朝４時にスナップショットを作成する設定
- **ファイルの最後は改行をいれること**

```
0 4 * * * /bin/sh /home/ec2-user/backup.sh

```

### EC2へのアップロードと設定
- 作成した２ファイルをEC2のホーム直下にアップロード
- EC2上でbackup.shにec2-userのアクセス権を付与
- crontabに設定

```
# MacからSCPコマンドでファイルをアップロード
$ scp -i ~/.ssh/キーペアファイル.pem backup.sh crontab.txt ec2-user@EC2.IP:~/
backup.sh  100%  115    19.9KB/s   00:00
crontab.txt  100%   37     7.1KB/s   00:00
```

```
# EC2上で
# 実行権限を付与
$ chmod 700 backup.sh

# crontabに設定
$ crontab crontab.txt

# 確認
$ crontab -l
```

### 実行確認
- 実行されるとEC2上でCronからec2-user宛てにメールが届く
- EC2ダッシュボードでもスナップショット作成が確認できる

```
# EC2にmailコマンドをインストール
$ sudo yum install -y mailx

# mailを確認
$ mail
```


## 停止（crontabの削除）
- 定期実行を停止するにはcrontabから削除を行う

```
# 削除
$ crontab -r
```
