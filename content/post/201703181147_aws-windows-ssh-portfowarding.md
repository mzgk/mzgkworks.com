+++
tags = ["AWS","Windows"]
draft = false
description = "Windowsクライアント → EC2 Linux踏み台 → EC2 WindowsServerにRDP接続を行う方法。"
date = "2017-03-18T11:47:00+09:00"
categories = ["Usage"]
slug = "aws-windows-ssh-portfowarding"
title = "SSHポートフォワーディングで、Windows → Linux → WindowsServerにRDP接続する"

+++

## やりたいこと
作業PC（Windows7）のTeraTermを使って、AWS上のEC2（WindowsServer2016）へ踏み台サーバー（Linux:EC2）経由でRDP接続をする。<br>
SSHポートフォワーディングの機能を利用する。


## 環境
- AWS EC2 : AmazonLinux -> 踏み台サーバー
- AWS EC2 : Windows Server 2016 -> RDP接続先
- Windows7 : ローカル作業PC TeraTerm4.9.4


## 準備
- RDP接続先のWindowsServer（EC2）は、**パブリックIPでなくても可**
- 事前に、AWSコンソールから接続ボタンでWindowsServerのパスワードを取得しておく
- [AWS EC2でWindowsインスタンスの起動とRDP接続](http://mzgkworks.com/post/aws-ec2-windows-rdp/)


## やり方
1. TeraTermで、踏み台EC2（Linux）にSSH接続
2. 接続したら、上部メニューの［設定］-> ［SSH転送］を選択
3. SSHポート転送のダイアログが表示される -> ［追加］ボタン
4. ［ローカルポート］: 適当なポート（13389）を入力
5. ［リモート側ホスト］: RDP接続先のIP（WindowsServer EC2のプライベートIP）
6. ［ポート］: RDP接続先のポート（3389）
7. ［OK］
8. リモートデスクトップクライアント（mstsc）を起動
9. ［コンピュータ］127.0.0.1:13389 -> ［接続］

**※RDP中はTeraTermで踏み台に接続されている必要がある。**
<br><br>

久々に開発クライアントでWindows7 32bitを使わざるをえないことになったのでメモ。作業効率が悪い。<br><br>
以上
