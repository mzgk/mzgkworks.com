+++
title = "Elastic Load Balancingを利用した、EC2-RDS環境の構築"
tags = ["AWS"]
draft = false
description = "AWS上で、ELBを使用してEC2のバランシングを行う環境の構築方法メモ。"
date = "2017-02-27T13:33:52+09:00"
categories = ["Usage"]
slug = "aws-elb-ec2-rds-wordpress"

+++

## 0. 環境
- 異なるAZにロードバランシングされるWordPress環境を構築する
- VPC・Subnet（4）・Internet Gateway・Route Table・Security Group
- ロードバランサー
  - 異なるAZに配置されたEC2に接続
- Webサーバー
  - 異なるAZにEC2を1台づつ配置
  - Public Subnet
- DBサーバー
  - テスト用なのでSingle-AZ
  - EC2からアクセス
  - Private Subnet
- アクセス制御
  - 各インスタンス用にSecurity Groupを作成して制御


----------
## 1. ネットワーク領域の作成
### VPC
- Sample_VPC
  - 20.0.0.0/16

### Subnet
- WebAP用（AZ-A）
  - Sample_Public_1A
  - ap-northeast-1a
  - 20.0.1.0/24
  - パブリックIP : はい
- WebAP用（AZ-C）
  - Sample_Public_1C
  - ap-northeast-1c
  - 20.0.2.0/24
  - パブリックIP : はい
- DB用（AZ-A）
  - Sample_Private_1A
  - ap-northeast-1a
  - 20.0.10.0/24
  - パブリックIP : いいえ
- DB用（AZ-C）
  - Sample_Private_1C
  - ap-northeast-1c
  - 20.0.20.0/24
  - パブリックIP : いいえ

### Internet Gateway
- Sample_IGW
  - アタッチ : Sample_VPC

### Route Table
- Sample_IGW_RTB
- VPC : Sample_VPC
- ルート
  - 送信先 : 0.0.0.0/0
  - ターゲット : Sample_IGW
- サブネットの関連付け
  - Sample_Public_1A
  - Sample_Public_1C

### Security Group
**EC2ダッシュボードから作成すると、送信元の設定が楽。**

- Sample_ELB_Sec
  - InBound : HTTP, 80, MyIP（本来は0.0.0.0/0）
- Sample_WebAP_Sec
  - InBound : HTTP, 80, Sample_ELB_Sec.ID
- Sample_DB_Sec
  - InBound : MySQL, 3306, Sample_WebAP_Sec.ID
- Sample_Maintenance_Sec
  - InBound : SSH, 22, MyIP
  - InBound : HTTP, 80, MyIP
  - InBound : MySQL, 3306, MyIP -> いらないかも


----------
## 2. インスタンスの作成
### RDSサブネットグループ
- Sample_DB_SubnetG
  - VPC : Sample_VPC
  - Subnet : Sample_Private_1A, Sample_Private_1C

### RDS
- Sample_DB_Master
  - Multi-AZ用のマスターDB
- エンジンの選択
  - MySQL
- 本番稼働用？
  - 開発／テスト
- 詳細の指定
  - DBエンジン :  mysql
  - ライセンスモデル : General Public License
  - DBエンジンのバージョン : 5.6.27
  - DBインスタンスのクラス : db.t2.micro
  - マルチAZ配置 : いいえ **（開発のため）**
  - ストレージタイプ : 汎用（SSD）
  - ストレージ割り当て : 5GB
  - DBインスタンス識別子 : Sample-DB
  - マスターユーザーの名前 : Sample_DB_User
  - マスターパスワード : Sample_DB_Password
- 詳細設定の設定
  - VPC : Sample_VPC
  - サブネットグループ : Sample_DB_SubnetG
  - パブリックアクセス可能 : いいえ
  - アベイラビリティーゾーン : ap-northeast-1a
  - VPCセキュリティーグループ : Sample_DB_Sec, Sample_Maintenance_Sec
  - データベースの名前 : Sample_DB
  - データベースのポート : 3306
  - DBパラメータグループ : default.mysql5.6
  - オプショングループ : default:mysql-5-6
  - その他はデフォルトのまま

### EC2-1
- Sample_Web_01
  - AMI : Amazon Linux
  - t2.micro
  - EBS : 8GB
  - ネットワーク : Sample_VPC
  - サブネット : Sample_Public_1A
  - 自動割り当てパブリックIP : 有効化
  - その他はデフォルト
  - セキュリティグループ : Sample_WebAP_Sec, Sample_Maintenance_Sec
  - キーペア : 作成


----------
## 3. ミドルウェアのセットアップ
### EC2 : Sample_Web_01
- 基本設定
  - MacからSSHログイン
  - `$ sudo yum update -y`
  - `$ sudo yum install -y httpd`
  - `$ sudo chkconfig httpd on` **#EC2起動時に合わせてApacheも起動させる**
  - `$ sudo service httpd start`
  - Macからブラウザ経由でアクセスし、Apacheぺージが表示されること
  - `$ sudo yum install -y php`
  - `$ sudo yum install -y php-mysql`
- WordPress
  - `$ mkdir ~/ttmp;cd ttmp`
  - `$ wget https://ja.wordpress.org/latest-ja.tar.gz`
  - `$ tar xzvf latest-ja.tar.gz`
  - `$ cd wordpress`
  - `$ cp wp-config-sample.php wp-config.PHP`
  - `$ vi wp-config.php`
      - DB_NAME : Sample_DB
      - DB_USER : Sample_DB_User
      - DB_PASSWORD : Sample_DB_Password
      - DB_HOST : Sample_DB_Masterのエンドポイント:3306
      - 認証キー : https://api.wordpress.org/secret-key/1.1/salt/
  - `$ sudo mv * /var/www/html/`
  - `$ sudo service httpd restart`
  - ブラウザからSample_Web_01のパブリックIPにアクセスして、wordpressの設定を行う


----------
## 4. ELBの適用
### ELBの作成
- Sample-ELB
  - ロードバランサーの種類 : Classic Load Balancer
  - ロードバランサーを作成する場所 : Sample_VPC
  - リスナー : HTTP 80
  - サブネットの選択 : Sample_Public_1A, Sample_Public_1C
  - セキュリティグループ : Sample_ELB_Sec
  - ヘルスチェック
      - pingプロトコル : HTTP
      - pingポート : 80
      - pingパス : /
      - 正常の閾値 : 5 -> 2
  - EC2インスタンス
      - Sample_Web_01（Sample_Web_02はAMI作成後）
      - クロスゾーン負荷分散の有効化 : チェック
      - 接続のストリーミングの有効化 : 300 -> 30 **（WordPressの場合は必須）**
  - タグ
      - Name : Sample_ELB

### 作成後の確認
- インスタンス タブ : インスタンスがInServiceになっていること

### WordPressの設定変更
- Sample_Web_01のパブリックIPでWordPressにアクセス
- 設定画面にて以下の内容をSample_ELBの「http:// + DNS名」に変更
  - WordPressアドレス
  - サイトアドレス

### ELB経由での接続確認
- ブラウザからSample_ELBのDNSでアクセスし、WordPressが表示されることを確認


----------
## 5. EC2の追加
### AMI作成
- Sample_Web_01からAMIを作成する
  - 停止 -> イメージの作成
  - イメージ名 : Sample-WebAP-AMI
  - AMI、スナップショットが作成される

### EC2の作成
- Sample_Web_02
  - **Sample_Web_01のAMIから作成**
  - ネットワーク : Sample_VPC
  - サブネット : Sample_Public_1C
  - 自動割り当てパブリックIP : 有効化
  - その他はデフォルト
  - セキュリティグループ : Sample_WebAP_Sec, Sample_Maintenance_Sec
  - キーペア : 先に作成したもの

### ロードバランサー配下に追加
- インスタンス タブ -> インスタンスの編集 -> Sample_Web_02を追加

### バランシングの確認
- 各EC2上の/var/log/httpd/access_logを確認
- `$ sudo tail -f /var/log/httpd/access_log`
