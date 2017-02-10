+++
draft = false
description = "ATOM vim-mode-plusのESCをちょっと便利にする。"
date = "2017-02-01T17:06:48+09:00"
categories = ["Setting"]
slug = "atom-vimmodeplus-autocomplete-esc"
title = "ATOM vim-mode-plusのESCで、autocomplete-plusのポップアップのみ閉じる方法"
tags = ["ATOM"]

+++

ATOMでvim-mode-plusとautocomplete-plusを導入済み。<br>
入力中にautocomplete-plusのポップアップをESCで閉じた際に、Vimのモードをそのままにしたい。<br>
何も設定しないと、ポップアップが閉じると同時にinsert-mode -> normal-modeになってしまう。<br><br>

追加で、autocomplete-plusの変換候補の移動／確定をTab/Enterに割り当てる。

## 環境
- macOS Sierra 10.12.3
- ATOM 1.13.1
- vim-mode-plus 0.82.0
- autocomplete-plus 2.33.1


## 症状
ATOMには「vim-mode-plus」を導入済み（autocomplete-plusはデフォルトインストール）<br>
↓<br>
insert-modeで入力中に、autocomplete-plusのポップアップが表示される<br>
↓<br>
候補の中に必要なものがないので、ESCでポップアップを閉じる<br>
↓<br>
ポップアップは閉じるが、insert-modeも抜けてnormal-modeになってしまう<br>
↓<br>
insert-modeのままにしたい


## 対応方法
ここを参照。  
[atom-vim-mode-plus TIPS](https://github.com/t9md/atom-vim-mode-plus/wiki/TIPS#in-insert-mode-hitting-escape-to-close-autocomplete-popup-result-in-normal-mode-but-want-to-remain-in-insert-mode)


## keymap.csonへの追加
以下をkeymap.csonに追加する。

```
'atom-text-editor.vim-mode-plus.insert-mode.autocomplete-active':
  'escape': 'autocomplete-plus:cancel'
```


## 追加：autocomplete-plusで「Tab：次候補」「Enter：確定」を有効にする
以下をkeymap.csonに追加する。<br>
__ATOM上で、autocomplete-plusの設定から「Use Core Movement Commands」のチェックを外す。__

```
# autocomplete-plusでtab:次候補 Enter:確定にする
# autocomplete-plusの設定で「Use Core Movement Commandsのチェックを外す
'body atom-text-editor.autocomplete-active':
  'ctrl-p': 'autocomplete-plus:move-up'
  'ctrl-n': 'autocomplete-plus:move-down'
  'tab': 'autocomplete-plus:move-down'
  'shift-tab': 'autocomplete-plus:move-up'
  'enter': 'autocomplete-plus:confirm'
```
 <br>
 以上
