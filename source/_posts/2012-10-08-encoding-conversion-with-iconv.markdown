---
layout: post
title: "iconvで文字コードを変換する"
date: 2012-10-08 00:00
comments: true
categories: [国際化]
keywords: iconv, 文字化け, 文字コード
description: "iconvをつかって文字コードをSJIS-WINからUTF-8へ変換した手順の記録です。"

---

　文字コードの変換は、変換前の文字コードを`--from-code (-f)`に、変換後の文字コードを`--to-code (-t)`を指定して、さらに入力ファイルと出力ファイルを指定して使用する。

<!-- more -->

　例えば、Windowsで作成したテキストファイル（SJIS-WIN）をUTF-8に変換したい場合は次のようなコマンドになる。

    $ iconv -f SJIS-WIN -t UTF-8 coded_in_sjiswin.txt > coded_in_utf8.txt

　扱える文字コードについては`iconv --list`で一覧が出力される。日本語に関連しそうな行に見当をつけて、一覧を制限してみると次のような出力だった。

    $ iconv --list | grep -iP "ja|jp"
    
      CSEUCPKDFMTJAPANESE//
      CSISO2022JP//
      CSISO2022JP2//
      EBCDIC-JP-E//
      EBCDIC-JP-KANA//
      EUC-JP-MS//
      EUC-JP//
      EUCJP-MS//
      EUCJP-OPEN//
      EUCJP-WIN//
      EUCJP//
      ISO-2022-JP-2//
      ISO-2022-JP-3//
      ISO-2022-JP//
      ISO646-JP-OCR-B//
      ISO646-JP//
      ISO2022JP//
      ISO2022JP2//
      JP-OCR-B//
      JP//

### 参考

- [テキストファイルの文字コードを変換するには - Ubuntu Japanese Wiki](https://wiki.ubuntulinux.jp/UbuntuTips/FileHandling/ConvertTextfileCharacterEncoding)
