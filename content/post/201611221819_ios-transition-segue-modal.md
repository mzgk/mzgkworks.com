+++
slug = "iso-transition-segue-modal"
categories = [
  "Programing"
]
date = "2016-11-22T18:19:42+09:00"
description = "ViewController間の画面遷移に関しての方針・使用方法・データ授受・ライフサイクルの再確認"
draft = false
tags = [
  "Swift",
  "iOS"
]
title = "iOSでの画面遷移（Segue/Modalの方針・データ授受・ライフサイクル）の確認"

+++

Single ViewController間の画面遷移に関して、以下の再確認のためのサンプル。

- 遷移方法（Segue・Modal）の使用方針
- VC間のデータを受け渡す方法
- ライフサイクルの確認


## 環境
- Xcode 8.1
- Swift 3.0


## 使用方針
### Segue
- Viewが提供する機能が完全に切り替わる場合に使用
- 遷移のアクションを発火させるUIから、遷移先のViewControllerへ直接Segueを接続しない
  - 遷移の発火がStoryBoardに埋もれるため
  - Triggered Segues : manualを使用してVC間を接続
  - 発火させるUIからは、コードでSegueを実行する
- 戻る処理はunwind Segueを使用
- データの受け渡し
  - 各VCのprepare(for:sender:)にて、インスタンスを生成して行う
- サンプル
  - [GitHub](https://github.com/mzgk/ManualSegue)

### Modal
- 元のViewから一時的に別機能を提供する場合に使用
- Segueを使用しないで、コードで表示させる
  - 機能の切り替えではないため
  - storyboard?.instantiateViewController(withIdentifier:) as! ModalのVCクラス名
  - self.present(\_:animated:completion:)
- 閉じるのは表示元の責務とし、Modal側でDelegateプロトコルを定義する
  - 表示元でDelegateプロトコルを実装し、Modalを閉じる
  - self.dismiss(animated: true, completion: nil)
- データの受け渡し
  - 元 → Modal : self.present(\_:animated:completion:)前に生成したModalのインスタンスに設定
  - Modal → 元 : Modal側でDelegateプロトコルを定義し、その引数を使用
- サンプル
  - [GitHub](https://github.com/mzgk/ModalWithCode)

<br>
<br>
SegueとModalの使い分け方針とデータの受け渡し方法をすっきりさせるためのアウトプット。  
ライフサイクルに関しては、シュミュレーターで実行したからなのかLayoutSubView()系が複数回コールされているのが、ちょっと違和感を感じる。
