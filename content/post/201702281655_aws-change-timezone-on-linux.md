+++
draft = false
description = "Amazon LinuxでタイムゾーンをUTCからJSTに変更する方法。"
date = "2017-02-28T16:55:07+09:00"
categories = ["Usage"]
slug = "aws-change-timezone-on-linux"
title = "EC2上のLinuxタイムゾーンを変更する"
tags = ["AWS"]

+++

EC2インスタンスを作成した際に、タイムゾーンをUTC -> JSTに変更する方法。  
（Amazon Linux限定ではない。）

## タイムゾーンを変更する
```
# 確認
$ date
2017年  2月 28日 火曜日 01:06:29 UTC

# UTC -> JSTに変更
$ sudo cp /usr/share/zoneinfo/Japan /etc/localtime

# 確認
$ date
2017年  2月 28日 火曜日 10:07:48 JST
```
