---
layout: post
title: "Exporting a Digital Certificates from Firefox"
date: 2013-02-24 23:05
comments: true
categories: 
keywords:
---

###エクスポート（Firefox）
## Firefoxの場合
- 登録している証明書を表示する
[{% img /blg/ eages/2012-12-06/02.png 400 345 %}](/blg/images/exporting-a-digital-certificates-from-firefox/01.png)
Firefox &gt; 設定 &gt; 「詳細」 &gt; 「暗号化」 &gt; 「証明書を表示」

- 証明書マネージャからバックアップを取得する
[{% img /blg/ eages/2012-12-06/02.png 400 345 %}](/blg/images/exporting-a-digital-certificates-from-firefox/02.png)

- 証明書マネージャで、今までに登録してきた証明書を確認できる。verisign.comでインストールした個人用証明書は、「あなたの証明書」のタブで確認できる。
- 証明書のバックアップ用パスワードの設定
[{% img /blg/ eages/2012-12-06/02.png 400 197 %}](/blg/images/exporting-a-digital-certificates-from-firefox/03.png)

- 秘密鍵を保護するパスワードを入力する。ここで入力したパスワードは、他クライアントへインポートする際にも必要になる。
- バックアップの取得が完了
[{% img /blg/ eages/2012-12-06/02.png 400 128 %}](/blg/images/exporting-a-digital-certificates-from-firefox/04.png)

- バックアップはPKCS12形式（.p12）のファイルとしてエクスポートされる。エクスポートされたPKCS12ファイルを電子メールクライアント（Thunderbirdなど）にインポートすると、メールの署名・暗号化が行えるようになる。


