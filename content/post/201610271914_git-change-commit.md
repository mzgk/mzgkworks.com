+++
draft = false
tags = [
  "Git"
]
title = "Git コミットをミスった時の操作"
slug = "git-change-commit"
categories = [
  "Usage",
]
date = "2016-10-27T19:14:50+09:00"
description = "Gitでコミットを直したい、取り消したい時の操作方法"

+++
コミットをミスった時の対処方法。  
**コミットのハッシュ値が変わるので、push前のコミットに対して行うこと。**  
独り開発の場合は、$ git push -f <リモート> <ブランチ> で強制pushもできるが...  

## 環境
- Git 2.10.1


## ハッシュ値
ハッシュ値の指定部分は以下でも置き換えが可能。  

- 現在のコミット : HEAD
- １つ前のコミット : HEAD~, HEAD^
- ２つ前のコミット : HEAD~2, HEAD^^


## メッセージを直したい
### 直前のコミットの場合
1. `$ git commit --amend`
2. コミットメッセージ入力画面で編集・保存・終了

### いくつか前のコミットの場合
1. `$ git log --oneline` : 対象コミットの特定
2. `$ git rebase -i <対象コミットの**１つ前**のハッシュ値>`
3. 変更するコミットのコマンドを「pick → r」に編集・保存・終了
4. コミットメッセージの編集・保存・終了
5. `$ git rebase --continue` : 対象が複数ある場合


## コミットに漏れの追加や内容修正をしたい
### 直前のコミットの場合
1. `$ git add A.txt` : 変更や漏れをステージング
2. `$ git commit --amend --no-edit` : メッセージ変更しない場合
3. `$ git commit --amend` : メッセージも変更する場合

### いくつか前のコミットの場合
1. `$ git log --oneline` : 対象コミットの特定
2. `$ git rebase -i <対象コミットの**１つ前**のハッシュ値>`
3. 変更するコミットのコマンドを「pick → e」に編集・保存・終了
4. `$ git add A.txt` : 変更や漏れをステージング
5. `$ git commit --amend (--no-edit)` : コミット
6. `$ git rebase --continue` : 対象が複数ある場合


## 今のコミットを取り消したい
### --soft : リポジトリだけ変更
コミットは取り消したいけど、インデックス・ワーキングエリアはそのままにしたい。  
1. `$ git reset --soft HEAD~` : HEADの１つ前の状態に巻き戻す

### --mixed : リポジトリ・インデックスを変更
コミットは取り消したいけど、ワーキングエリアだけはそのままにしたい。  
1. `$ git reset --mixed HEAD~` : HEADの１つ前の状態に巻き戻す

### --hard : まったくなかったことにしたい
コミットも取り消して、ワーキングエリアの状態も戻したい。  
1. `$ git reset --hard 40eeb32` : HEADの１つ前のハッシュ値でも可


## 直近のいくつかのコミットを取り消したい（途中のいくつかだけはrebaseで）
### --soft : リポジトリだけ変更
コミットは取り消したいけど、インデックス・ワーキングエリアはそのままにしたい。  
1. `$ git reset --soft HEAD~3` : HEADの3つ前の状態に巻き戻す

### --mixed : リポジトリ・インデックスを変更
コミットは取り消したいけど、ワーキングエリアだけはそのままにしたい。  
1. `$ git reset --mixed HEAD~3` : HEADの3つ前の状態に巻き戻す

### --hard : 全エリアを変更
コミットも取り消して、ワーキングエリアの状態も戻したい。  
1. `$ git reset --hard 40eeb32` : その状態にしたいコミットのハッシュ値でも可



## あのコミットをなくしたい
途中のあのコミットだけを削除したい場合。  

1. `$ git log --oneline` : 対象コミットの特定
2. `$ git rebase -i <対象コミットの**１つ前**のハッシュ値>`
3. 対象のコミットの行のコマンドを「pick → d」に変更して、保存・終了

```
$ git log --decorate --oneline
1588243 (HEAD -> master) 3 bye
2620a4a 2 morning ← このコミットを削除したい
40eeb32 1st hello

// rebase
$ git rebase -i HEAD~2（or 40eeb32）
... エディタが起動
pick 2620a4a 2 morning
pick 1588243 3 bye

... 対象コミットのpickをdに編集して、保存・終了
d 2620a4a 2 morning ← 削除 : d
pick 1588243 3 bye

// コンフリクトが発生した場合は、解消して
$ git add .
$ git rebase --continue
```



## コミットをまとめたい
いくつかのコミットをまとめて、１つのコミットにしたい場合。  
**１つ前** のコミットと統合される。

1. `$ git log --oneline` : 対象コミットの特定
2. `$ git rebase -i <対象コミットの**１つ前**のハッシュ値>`
3. （メッセージ変更あり）まとめたいコミット行のコマンドを「s」に変更して、保存・終了
4. （メッセージ変更なし）まとめたいコミット行のコマンドを「f」に変更して、保存・終了

```@git
$ git log --oneline
a990297 4 commit
72698fd 3 commit ← これと
38517d7 2 commit ← これを統合したい
1736724 1 commit

// rebase開始
$ git rebase -i HEAD~3
pick 38517d7 2 commit
pick 72698fd 3 commit　← これを２に統合（１つ前に統合されるので）
pick a990297 4 commit

// 編集して、保存・終了
pick 38517d7 2 commit
s 72698fd 3 commit → s:メッセージ編集あり／f:なし（１つ前のメッセージが適用）
pick a990297 4 commit

// メッセージ編集が表示される
# This is a combination of 2 commits.
# This is the 1st commit message:

2 commit

# This is the commit message #2:

3 commit

// 編集して、保存・終了
2-3 commit

// 確認
$ git log --oneline
f91881d 4 commit
a7c420d 2-3 commit
1736724 1 commit
```


## コミットの順番を変更したい
コミットの順番を入れ替えたい場合。  

1. `$ git log --oneline` : 対象コミットの特定
2. `$ git rebase -i <対象コミットの**１つ前**のハッシュ値>`
3. コミットの行を入れ替えて、保存・終了

```@git
$ git log --oneline
f02a616 4 commit
9b399bc 3 commit file add ← これと
12f55d3 2 commit          ← これの順番を入れ替えたい
8a337e8 1 commit

// rebase開始
$ git rebase -i HEAD~3
pick 12f55d3 2 commit
pick 9b399bc 3 commit file add ← この行を2 commitの上に移動する
pick f02a616 4 commit

// 編集し、保存・終了
pick 9b399bc 3 commit file add
pick 12f55d3 2 commit
pick f02a616 4 commit

// 結果
$ git log --oneline
d7f14ec 4 commit
00d216a 2 commit
a22ab93 3 commit file add
8a337e8 1 commit
```
