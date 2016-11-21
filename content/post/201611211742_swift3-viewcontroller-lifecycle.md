+++
draft = false
tags = [
  "Swift",
  "iOS"
]
title = "Swift3 ViewControllerのライフサイクル"
slug = "swift3-viewcontroller-lifecycle"
categories = [
  "Programing"
]
date = "2016-11-21T17:42:49+09:00"
description = "Swift3でのViewControllerライフサイクルの再確認"

+++

ViewControllerのルートViewのライフサイクルを再確認。  
各メソッドに以下のようなデバッグログを埋め込んで確認。  
<br>
```@Swift
override func loadView() {
    super.loadView()
    print("VC1 :", #function)
    // #functionで関数名が表示される
}
```

## 環境
- Xcode 8.1
- Swift 3
- シュミュレーターで確認（iPhone6s 10.1）


## 起動 → ホームボタン → アプリアイコン
### 起動
1. application(_:didFinishLaunchingWithOptions:)
1. VC1 : loadView()
1. VC1 : viewDidLoad()
1. VC1 : viewWillAppear
1. applicationDidBecomeActive
1. VC1 : viewWillLayoutSubviews()
1. VC1 : viewDidLayoutSubviews()
1. VC1 : viewWillLayoutSubviews()
1. VC1 : viewDidLayoutSubviews()
1. VC1 : viewDidAppear

### ホームボタンをタップ
1. applicationWillResignActive
1. applicationDidEnterBackground

### アプリアイコンをタップ
1. applicationWillEnterForeground
1. applicationDidBecomeActive


## VC1（ボタンタップ） → VC2（モーダル表示）
### 画面遷移
1. VC1 : prepare(for:sender:)
1. VC2 : loadView()
1. VC2 : viewDidLoad()
1. VC1 : viewWillDisappear
1. VC2 : viewWillAppear
1. VC2 : viewWillLayoutSubviews()
1. VC2 : viewDidLayoutSubviews()
1. VC1 : viewWillLayoutSubviews()  ← 謎
1. VC1 : viewDidLayoutSubviews()   ← 謎
1. VC2 : viewWillLayoutSubviews()  ← 謎
1. VC2 : viewDidLayoutSubviews()   ← 謎
1. VC2 : viewDidAppear
1. VC1 : viewDidDisappear
