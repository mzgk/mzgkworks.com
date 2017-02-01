+++
date = "2017-02-01T16:27:09+09:00"
categories = ["Setting"]
slug = "ssh-connction-to-github"
title = "GitHubにSSH接続する"
tags = ["Git"]
draft = false
description = "Mac上でSSH鍵を生成し、GitHubとのSSH接続を可能にする。"

+++

SSH鍵を作成・登録し、GitHubとのSSH接続を可能にする。  
たまにやる作業で、いつも忘れて検索するのでメモ。


## 環境
- MacBook Pro (15-inch, Mid 2012)
- macOS Sierra 10.12.3


## SSH鍵の作成（Mac）
- ターミナルを起動し、Mac上でSSH鍵を作成する
- 秘密鍵・公開鍵の２つが作成される
  - id_rsa_xxx : 秘密鍵（Mac上へ）
  - id_rsa_xxx.pub : 公開鍵（GitHubに登録）
- SSH鍵は `~/.ssh` 配下に格納する

```
# 鍵の作成
# -t 鍵の種類
# -b 鍵の長さ（GitHubは4096を推奨）
$ ssh-keygen -t rsa -b 4096 -C "鍵のコメント"
Generating public/private rsa key pair.

# 鍵の出力場所と鍵名の入力を求められる
# 鍵名 -> id_rsa_github
Enter file in which to save the key (/Users/USER名/.ssh/id_rsa): ここに入力

# 鍵のパスフレーズの入力を求められる
Enter passphrase (empty for no passphrase): ここに新規パスワードを入力
Enter same passphrase again:

# 鍵が作成される
...
+---[RSA 4096]----+
...
+----[SHA256]-----+
```


## SSH鍵の確認
- 鍵ファイルが作成されたことを確認する
- 秘密鍵のパーミッションを変更（600になっていない場合）

```
# 作成されたかを確認する
$ ls -la ~/.ssh
-rw-------   1 xxxx  staff  3326  2  1 15:37 id_rsa_github     # 秘密鍵
-rw-r--r--   1 xxxx  staff   742  2  1 15:37 id_rsa_github.pub # 公開鍵

# 秘密鍵のパーミッションを変更する（600以外の場合）
$ chomod 600 ~/.ssh/id_rsa_github
```


## SSHの設定ファイルへの追記
- `~/.ssh/config` ファイルに、SSHの接続情報を記述する
- 今後、ターミナル上で `$ ssh github` を入力することで接続できるようになる

```
# エディタで開く
$ vim ~/.ssh/config

# 以下を追加する
HOST github
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_github
```


## GitHubに公開鍵を登録する
- ブラウザでGitHubにログインし、公開鍵を登録する
- アカウントメニュー -> Settings -> SSH and GPG keys
- SSH Keys -> New SSH Key
- Title : 任意の名前（どのPCからの鍵なのかなど）
- Key : 以下で公開鍵をコピーした内容を貼り付ける

```
# 公開鍵の内容をコピーする
$ pbcopy < ~/.ssh/id_rsa_github.pub
```


## GitHubとのSSH接続を確認する
- GitHubへSSH接続の確認を行う
- 途中で秘密鍵を作成した際に入力したパスフレーズの入力を求められる

```
# 接続（github -> configに設定したHOSTの値）
$ ssh github
The authenticity of host 'github.com (192.30.253.112)' can't be established.
...
Are you sure you want to continue connecting (yes/no)? yes
...
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
Enter passphrase for key '/Users/xxxx/.ssh/id_rsa_github': 鍵作成時のパスフレーズを入力
PTY allocation request failed on channel 0
Hi ユーザー名! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

<br>
以上
