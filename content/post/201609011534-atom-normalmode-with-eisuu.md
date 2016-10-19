+++
categories = ["Usage"]
date = "2016-09-01T15:34:24+09:00"
description = "Atom（vim-mode-plus）で、ノーマルモード移行時に日本語を自動的にオフにする方法。"
draft = false
slug = "atom-vim-normalmode-with-eisuu"
tags = ["Atom", "Mac"]
title = "Atom（vim-mode-plus）で、ノーマルモード移行時に日本語オフにする方法。"

+++

Atomにvim-mode-plusをインストールして使っているが、ノーマルモードに移行する時に日本語入力モードを引きずったままになるのが面倒。  
ノーマルモードになった時には、自動で英数入力モードになって、そのままカーソル移動とかしたい。

## 環境
- OS X 10.11.6
- Atom 1.10.0
- karabiner 10.21.0


## 方法
- [Karabiner](https://pqrs.org/osx/karabiner/index.html.ja)をMacにインストールする
- private.xmlに設定を書く


## 設定方法
設定の追加方法は、Karabiner公式サイトのマニュアルを参照のこと。  
https://pqrs.org/osx/karabiner/document.html.ja#privatexml

### private.xml
記述する内容は以下。
```@xml
<?xml version="1.0"?>
<root>
    <appdef>
        <appname>ATOM</appname>
        <equal>com.github.atom</equal>
    </appdef>
    <item>
        <name>Atom Into NormalMode With Eisuu</name>
        <identifier>private.app_atom_into_normalmode_with_eisuu</identifier>
        <only>ATOM</only>
        <autogen>
            --KeyToKey--
            KeyCode::ESCAPE,
            KeyCode::ESCAPE, KeyCode::JIS_EISUU
        </autogen>
        <autogen>
            --KeyToKey--
            KeyCode::BRACKET_LEFT, VK_CONTROL,
            KeyCode::ESCAPE, KeyCode::JIS_EISUU
        </autogen>
        <autogen>
            --KeyToKey--
            KeyCode::C, VK_CONTROL,
            KeyCode::ESCAPE, KeyCode::JIS_EISUU
        </autogen>
    </item>
</root>
```

## おまけ
Karabinerの設定で、jklhでの移動を他アプリでも使えるようにする方法。  
` s + jklh` でカーソル移動ができるようになる。  
公式サイトのマニュアルにあった。  
[Simple Vi Mode v2](https://pqrs.org/osx/karabiner/gallery.html.ja#simple-vi-mode-v2)
