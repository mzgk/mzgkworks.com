+++
categories = ["Knowledge"]
date = "2016-09-02T16:00:48+09:00"
description = "HTTPのリクエストとレスポンスのメモ"
draft = false
slug = "http-request-response"
tags = ["HTTP"]
title = "HTTPのリクエストとレスポンスのメモ"

+++

HTTPプロトコルでやりとりしている中身のメモ。

## HTTPリクエスト
### 構成
- リクエストライン
- ヘッダ
- 空行
- ボディ

### 例（ボディなし）
```@bash
# telnetコマンドでexample.comのポート80にアクセス
$ telnet example.com 80
Trying 93.184.216.34...
Connected to example.com.
Escape character is '^]'.

# リクエストを入力（最後に空行を入れること）
GET / HTTP/1.1    # GETメソッドでパス「/」にHTTP1.1でリクエスト
Host: example.com # ヘッダ


```

### リクエスト
- GET     : URLで示されたリソースをサーバから取得
- POST  : クライアントからサーバへデータを送信
- PUT
- HEAD
- DELETE
- OPTION
- TRACE
- CONNECT


## HTTPレスポンス
### 構成
- レスポンスライン
- レスポンスヘッダ
- 空行
- ボディ

### 例
```@bash
HTTP/1.1 200 OK                 # レスポンスライン
Accept-Ranges: bytes            # ヘッダ
Cache-Control: max-age=604800
Content-Type: text/html
Date: Fri, 02 Sep 2016 06:11:51 GMT
Etag: "359670651+gzip"
Expires: Fri, 09 Sep 2016 06:11:51 GMT
Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
Server: ECS (cpm/F9D5)
Vary: Accept-Encoding
X-Cache: HIT
x-ec-custom-error: 1
Content-Length: 1270
                      # 空行
<!doctype html>         # ボディ
<html>
<head>
    <title>Example Domain</title>
...
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is established to be used for illustrative examples in documents. You may use this
    domain in examples without prior coordination or asking for permission.</p>
    <p><a href="http://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>
```

### 代表的なステータスコード
| コード | メッセージ            | 説明                                                       |
|:-------|:----------------------|:-----------------------------------------------------------|
| 200    | OK                    | リクエストの処理が成功                                     |
| 301    | Moved Permanently     | URLが恒久的に変更。Lacationヘッダに移転先URLが記載される。 |
| 302    | Found                 | URLが一時的に変更。他は同上。                              |
| 403    | Forbidden             | リクエスト先のURLにアクセス禁止。                          |
| 404    | Not Found             | リクエスト先のURLが見つからない。                          |
| 500    | Internal Server Error | サーバ側でエラー                                           |
| 503    | Service Unavailabel   | サーバが一時的に利用できない                               |
