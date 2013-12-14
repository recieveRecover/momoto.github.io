---
layout: post
title: "OpenSSLを使用してファイルをRSA暗号化する"
date: 2012-10-14 17:45
comments: true
categories: [暗号化, OpenSSL]
keywords: RSA, OpenSSL
description: "OpenSSLを使用してファイルをRSA暗号化する手引きです。"
breadcrumb:
  - title: Blog
    url: /
  - title: 暗号化
    url: /blog/categories/an-hao-hua/

---

OpenSSLは様々な暗号方式での暗号化、復号化をサポートするセキュリティライブラリ。
使用できる暗号方式は`openssl ciphers`で確認できる。

RSA暗号方式の暗号化、復号、署名、検証を利用する場合は`openssl rsautl`を使う。

<!-- more -->

### 暗号化

ファイルをRSA暗号方式で暗号化するには、秘密鍵または公開鍵のいずれかが必要。

#### ファイル*"plain.txt"*を、秘密鍵*"private_key"*を使用して暗号化する

    $ openssl rsautl -encrypt -inkey private_key -in plain.txt -out encrypted
    Enter pass phrase for private_key:

秘密鍵にパスフレーズが設定されていれば入力が必要。

#### ファイル*"plain.txt"*を、公開鍵*"public_key"*を使用して暗号化する

    $ openssl rsautl -encrypt -pubin -inkey public_key -in plain.txt -out encrypted

*-pubin*のオプションを加えることに注意。

上のコマンドで暗号化した場合、*encrypted*という名前でバイナリファイルとして出力される。
使用できる公開鍵は下のような形式になる。

    -----BEGIN PUBLIC KEY-----
    MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAM2twTjVLOYMK2Y7uKQ03CeSc8B+0GjJ
    xy0ir28fIfGb523ogDyWKDjMDx5pTBUpOgDYUgSpP/I6SzER1ArhcnMCAwEAAQ==
    -----END PUBLIC KEY-----

`unable to load Public Key`とエラーが出る場合は、鍵の形式が無効。

### 復号

暗号化されたファイルを復号するには、秘密鍵が必要。

#### 暗号化されたファイル*"encrypted"*を、秘密鍵*"private_key"*を使用して復号する

    $ openssl rsautl -decrypt -inkey private_key -in encrypted -out decrypted.txt
    Enter pass phrase for private_key:

復号に成功すると、*"decrypted.txt"*にその内容が出力される。

