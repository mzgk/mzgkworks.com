+++
tags = [
  "Swift",
  "iOS",
]
title = "Swiftの引数についてのメモ"
slug = "swift-argument"
categories = [
  "Programing",
]
date = "2016-10-26T16:27:37+09:00"
description = "Swiftの引数のメモ。"
draft = false

+++

Swiftの引数について、1.x時代から変わったりうろ覚えだった部分もあったのでメモ。  

## 環境
- Swift 3.0


## ラベル
- argument label : 関数呼び出し用（呼び出し時に使用）
  - 「\_」をつけることで省略可
- parameter label : 関数定義用（関数内部で使用）

### argument label : あり
```@Swift
// argument label : first, second
// parameter label : a, b
func addTwo(first a: Int, second b: Int)-> Int {
    return a + b
}

// 呼び出し
var value = addTwo(first: 1, second: 2)
// 結果 : 3
print(value)
```


### argument label : なし
```@Swift
// parameter label : a, b
func addTwo(_ a: Int, _ b: Int)-> Int {
    return a + b
}

// 呼び出し
var value = addTwo(1, 2)
// 結果 : 3
print(value)
```


## デフォルト引数
あたかじめ引数に値を設定しておくことで、呼び出し時に引数の省略が可能となる。  
省略された場合、定義されているデフォルト値が適用される。  

```@Swift
// aとbの文字列連結。セパレーターのcにデフォルト値（, ）を定義
func connectTwo(first a: String, second b: String, separator c: String = ", ")-> String {
    return a + c + b
}


// デフォルト引数を使う
var val = connectTwo(first: "A", second: "B")
// 結果 : A, B
print(val)


// デフォルト値を使わない
val = connectTwo(first: "A", second: "B", separator: "/ ")
// 結果 : A/ B
print(val)
```


## 可変引数
あらかじめ引数の数がわからない場合などに使用する。  
定義時は、型の後ろに「...」をつける。  
呼び出し時は、複数の値を「,（カンマ）」で区切って設定する。  
可変引数の定義は、１関数につき１つまで。  

```@Swift
// 最初の引数が可変
// 引数の値をセパレーターで区切って連結する関数
func write(inWord a: String..., separator c: String = ", ")-> String {
    var output = ""
    for index in 0..<a.count {
        output += a[index] + c
    }
    return output
}

// １つの場合
var val = write(inWord: "ABC")
// 結果 : ABC,
print(val)


// ３つの場合
val = write(inWord: "ABC", "DEF", "GHI")
// 結果 : ABC, DEF, GHI,
print(val)
```


## 引数の値の変更
引数の値は定数となるので、関数内での変更は不可。  
ただし、インスタンスの場合は参照値が格納されているので、インスタンスの参照値は変更不可だが、中のプロパティ値の変更は可能。  
※プロパティがvarで宣言されている場合のみ

### 通常
```@Swift
// 引数aの値は変更できない
func change(_ a: String)-> String {
  a = "XYZ"   // ← Cannot assign to value 'a' is a 'let' constant
}

change("ABC")
```


### インスタンスの場合
```@Swift
class Person {
    let name: String
    var age: Int    // ※ここがletの場合は、変更できない

    init(inName name:String, inAge age: Int) {
        self.name = name
        self.age = age
    }
}

// 引数で渡されたPerson型の引数のageプロパティを変更する関数
func update(inPerson person: Person) {
    person.age = 20
}

// Tomを10才で生成（Tom : 10）
var tom = Person(inName: "Tom", inAge: 10)
print("\(tom.name) : \(tom.age)")


// 更新する関数に渡す
update(inPerson: tom)
// 結果 : Tom : 20
print("\(tom.name) : \(tom.age)")
```


## InOutParameter
定義時に型の前に「inout」をつけることで、引数の値を関数内で変更し呼び出し元に連携することができる。  
呼び出し時は、引数の前に「&」をつける。

```@Swift
// 引数の値を入れ替える
func swap(first a: inout String, second b: inout String) {
    let tmp = a
    a = b
    b = tmp
}

var wordA = "ABC"
var wordZ = "XYZ"

// 入れ替え前 : wordA = ABC, wordZ = XYZ
print("wordA = \(wordA), wordZ = \(wordZ)")

// 入れ替え（引数に&がつく）
swap(first: &wordA, second: &wordZ)
// 入れ替え後 : wordA = XYZ, wordZ = ABC
print("wordA = \(wordA), wordZ = \(wordZ)")

```
