+++
categories = ["Setting"]
date = "2016-10-04T18:57:07+09:00"
description = "MacでSassの環境を整える。Ruby gem版"
draft = false
slug = "ruby-gem-install-sass"
tags = ["Sass", "Mac"]
title = "MacにSass環境を構築する（Ruby gem）"

+++
## 環境
- OS X 10.11.6 El Capitan


## 望むこと
- scssで書いて、動的にcssにコンパイル
- 環境周りのコストを少なく


## 選択肢は
- gulp : 使うほどタスクないし、プラグインや設定ファイルなどの初期学習コストが高い
- npm run : package.jsonが面倒
- Macなのでrubyがインストール済み
  - ` $ sudo gem install sass ` → sassコマンドか！


## 環境構築
### Ruby
存在確認  
```@bash
$ ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin15]
```


gemのアップデート  
```@bash
$ sudo gem update --system
...
```


### sass
存在確認
```@bash
$ sass -v
-bash: sass: command not found
```

インストール
```@bash
$ sudo gem install sass
...
1 gem installed

$ sass -v
Sass 3.4.22 (Selective Steve)
```


## コンパイル
### フォルダ構成  
```
.
|-- css   → ここにコンパイルされたmain.cssが生成される
`-- scss
    |-- base
    |   |-- _reset.scss   → _sanitize.scssを@import
    |   `-- lib
    |       `-- _sanitize.scss
    |-- components
    |   `-- _component.scss   → いろいろと記述
    `-- main.scss   → reset,componentのimportのみ
```

### コンパイル
```@bash
# scssディレクトリ以下のファイル更新を監視し、cssディレクトリへ出力
$ sass --watch --style expanded scss:css
```

### オプション
- watch : ファイルの更新を監視し、更新されたらコンパイル
- style : 生成されるcssの形式
  - nested : sassの階層をインデントで残す（デフォルト）
  - expanded : 普通のcss形式
  - compact : セレクタとプロパティが1行形式
  - compressed : 圧縮形式

※styleを途中で変更する場合は、一度生成された.cssを削除すること
