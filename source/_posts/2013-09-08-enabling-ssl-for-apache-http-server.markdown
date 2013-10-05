---
layout: post
title: "Apache HTTP ServerのSSL/TLSサポートを有効化する"
date: 2013-09-08 00:00
comments: true
categories: [WWW, Apache, 暗号化, OpenSSL]
keywords: Apache HTTP Server 2.2.15, httpd, OpenSSL, TLS, mod_ssl
description: "mod_sslをつかってApache HTTP ServerのSSL/TLSサポートを有効化します。"
breadcrumb:
  - title: Blog
    url: /
  - title: WWW
    url: /blog/categories/www/

---

　mod_sslをつかってApache HTTP ServerのSSL/TLSサポートを有効化します。
SSLを有効にするために必要なRSA秘密鍵と公開鍵証明書のうち、公開鍵証明書は第三者の認証局に署名してもらうのが通常ですが、この記事では検証のため自己発行証明書をつかいます。<!-- more -->

　手順としては(1) RSA秘密鍵を生成、(2) 証明書署名要求（CSR）を作成、(3) 公開鍵証明書を発行、その後、ウェブサーバでSSL/TLSを設定します。(3)の「公開鍵証明書の発行」は本来、認証局で行う手順です。

 1. ### OpenSSLをつかってRSA秘密鍵と証明書署名要求を用意する（申請者）

    　RSA秘密鍵の生成には`openssl genrsa -out <秘密鍵ファイル名> <鍵長>`のコマンドをつかいます。

        $ openssl genrsa -out localhost.key 2048
        Generating RSA private key, 2048 bit long modulus
        ...................................................+++
        ...............+++
        e is 65537 (0x10001)

    　2013年9月現在、1024bit鍵長のCSRの受付を停止した認証局もあるように、鍵長は2048bitが一般的であるようです。

    - [旧来仕様のサーバIDおよび公開鍵長1024bitのCSRの受付停止について｜日本ベリサイン](https://www.verisign.co.jp/ssl/about/20120531.html)
    - [1024bitの証明書について｜グローバルサイン (旧日本ジオトラスト株式会社)](https://jp.globalsign.com/knowledge/ssl/use/1024bit.html)

    　`openssl genrsa`のオプションによって、生成したRSA秘密鍵を別の暗号方式で暗号化することや、乱数生成につかわれるシード値をファイルで指定することもできます。
    RSA秘密鍵をAESやDESなどの共通鍵暗号方式で暗号化する場合は秘密鍵にパスフレーズを設定します。このパスフレーズはウェブサーバの起動時や証明書署名要求ファイル作成時に入力が必要になります。

    　続いて、RSA秘密鍵をもとに証明書署名要求ファイルを作成します。OpenSSLコマンドは`openssl req -new -key <秘密鍵ファイル名> -out <証明書署名要求ファイル名>`をつかいます。
    CSRの作成にはディスティングイッシュネームの入力が必要です。

        $ openssl req -new -key localhost.key -out localhost.csr
        You are about to be asked to enter information that will be incorporated
        into your certificate request.
        What you are about to enter is what is called a Distinguished Name or a DN.
        There are quite a few fields but you can leave some blank
        For some fields there will be a default value,
        If you enter '.', the field will be left blank.
        -----
        Country Name (2 letter code) [XX]:JP
        State or Province Name (full name) []:Tokyo
        Locality Name (eg, city) [Default City]:Shinjuku
        Organization Name (eg, company) [Default Company Ltd]:Localhost, Local Area Network
        Organizational Unit Name (eg, section) []:
        Common Name (eg, your name or your server's hostname) []:localhost
        Email Address []:

        Please enter the following 'extra' attributes
        to be sent with your certificate request
        A challenge password []:
        An optional company name []:

    - [ディスティングイッシュネームとは何ですか | 日本ベリサイン](https://www.verisign.co.jp/ssl/help/faq/110041/)
    - [ディスティングイッシュネームとは何ですか | グローバルサイン](https://jp.globalsign.com/support/faq/74.html)
    - [ディスティングイッシュネームとは何ですか？ | クロストラスト ](https://crosstrust.co.jp/support/faq/faq08/a002)

 2. ### 公開鍵証明書を発行する（認証局）

    　証明書署名要求ファイルと認証局の私有鍵をもとに公開鍵証明書を発行します。
    OpenSSLコマンドは`openssl ca -in <証明書署名要求ファイル名> -out <公開鍵証明書ファイル名>`をつかいます。

        $ openssl ca -in localhost.csr -out localhost.self-signed.crt
        Using configuration from /etc/pki/tls/openssl.cnf
        Enter pass phrase for /etc/pki/CA/private/cakey.pem:
        Check that the request matches the signature
        Signature ok
        Certificate Details:
                Serial Number:
                    ec:65:47:48:ff:6d:ff:cc
                Validity
                    Not Before: Sep  7 00:00:00 2013 GMT
                    Not After : Sep  7 00:00:00 2014 GMT
                Subject:
                    countryName               = JP
                    stateOrProvinceName       = Tokyo
                    organizationName          = Localhost, Local Area Network
                    commonName                = localhost
                X509v3 extensions:
                    X509v3 Basic Constraints:
                        CA:TRUE
                    Netscape Comment:
                        OpenSSL Generated Certificate
                    X509v3 Subject Key Identifier:
                        02:B3:E2:99:8B:C9:E8:F2:33:A7:27:1B:FD:D6:9E:64:C9:12:D2:7E
                    X509v3 Authority Key Identifier:
                        keyid:15:F3:B6:82:FD:BB:41:AF:F2:AE:D9:BD:E1:C0:2E:B6:A5:23:C6:FA

        Certificate is to be certified until Sep  7 00:00:00 2014 GMT (365 days)
        Sign the certificate? [y/n]:y


        1 out of 1 certificate requests certified, commit? [y/n]y
        Write out database with 1 new entries
        Data Base Updated

    　認証局は発行した公開鍵証明書を申請者に渡して、申請者は公開鍵証明書をウェブサーバに移動します。

 3. ### mod_sslを設定する（Apache HTTP Server）

    　[www.modssl.org](http://www.modssl.org/source/)やディストリビューションからmod_sslをインストールします。

        $ sudo yum install mod_ssl

    　上記の手順で用意したRSA秘密鍵と公開鍵証明書を、Apache実行ユーザが読み込めるファイルパスに設置します。同様にパーミッションも適宜、設定します。

        $ sudo mv localhost.key /etc/pki/tls/private/
        $ sudo mv localhost.self-signed.crt /etc/pki/tls/certs/
        $ sudo chmod 0600 /etc/pki/tls/certs/localhost.self-signed.crt
        $ sudo chmod 0600 /etc/pki/tls/private/localhost.key

    　Apacheの設定ファイル（`<ServerRoot>/conf.d/ssl.conf`など）を編集して、SSLCertificateFileディレクティブとSSLCertificateKeyFileディレクティブを設定します。

        SSLCertificateFile /etc/pki/tls/certs/localhost.self-signed.crt
        SSLCertificateKeyFile /etc/pki/tls/private/localhost.key

    　Apacheを再起動します。RSA秘密鍵を暗号化している場合はパスフレーズの入力が必要です。

        $ sudo apachectl -t
        Syntax OK
        $ sudo /etc/init.d/httpd restart
        Stopping httpd:                                            [  OK  ]
        Starting httpd: Apache/2.2.15 mod_ssl/2.2.15 (Pass Phrase Dialog)
        Some of your private key files are encrypted for security reasons.
        In order to read them you have to provide the pass phrases.

        Server localhost:443 (RSA)
        Enter pass phrase:

        OK: Pass Phrase Dialog successful.
                                                                   [  OK  ]

　ウェブサーバにアクセスしてみると、発行者不明（sec_error_unknown_issuer）のため接続を信頼されていませんが、接続は暗号化されているようです。

{% img /blog/images/2013-09-08-enabling-ssl-for-apache-http-server/01-untrusted-connection.png 500 安全性を確認できない接続 %}

{% img /blog/images/2013-09-08-enabling-ssl-for-apache-http-server/02-connection-encrypted.png 500 暗号化された接続 %}

　証明書情報を表示すると入力したディスティングイッシュネームを確認することができます。また、この記事の例では主体者と発行者が同一になっていることも確認できます。

{% img /blog/images/2013-09-08-enabling-ssl-for-apache-http-server/03-could-not-verify-this-certificate.png 500 信頼性を検証できない証明書 %}

## 参考

- [CA/ブラウザフォーラム パブリック証明書の発行および管理に関する基本要件v.1.0 | www.verisign.co.jp](https://www.verisign.co.jp/ssl/pdf/baseline_requirements1.0_jp.pdf)
- [CA/ブラウザフォーラム パブリック証明書発行及びマネジメントの基本要件 ver1.0 | jp.globalsign.com](https://jp.globalsign.com/repository/baseline_requirements_ja.pdf)
- [mod_ssl - Apache HTTP Server](http://httpd.apache.org/docs/2.2/mod/mod_ssl.html)
- [OpenSSL: Documents, Misc](http://www.openssl.org/docs/)
- [UNIXの部屋 コマンド検索:openssl (*BSD/Linux)](http://x68000.q-e-d.net/~68user/unix/pickup?openssl)
- [安全な接続ができませんでした | Firefox ヘルプ](https://support.mozilla.org/ja/kb/Secure%20Connection%20Failed)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=490531822X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=477415783X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
