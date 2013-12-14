---
layout: post
title: "Windows 7にIIS 7.5をインストール"
date: 2012-11-30 20:56
comments: true
categories: [Microsoft Windows]
keywords: Microsoft Windows, Internet Information Services, IIS
description: "MicrosoftのWebサーバーソフトウェア Internet Information Services (IIS)インストールの手引きです。"

---

　MicrosoftのWebサーバーソフトウェア"Internet Information Services (IIS)"をWindows 7 Home Premiumにインストールした際の手順の記録です。

<!-- more -->

### IISのインストール

1.  スタートメニュー等から「コントロールパネル」へ

    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/01.png %}](/blog/images/2012-11-30/01.png)
2.  コントロールパネルから「プログラム」の画面へ

    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/02.png 600 %}](/blog/images/2012-11-30/02.png)
3.  プログラムから「Windowsの機能の有効化または無効化」の画面へ

    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/03.png 600 %}](/blog/images/2012-11-30/03.png)
4.  　Windowsの機能から「インターネット インフォメーション サービス」のチェックボックスをオンにする。
    サブフォルダのチェックボックスからはIISの追加機能（FTPサーバーやアプリケーション開発機能など）を指定できる。

    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/04.png 600 %}](/blog/images/2012-11-30/04.png)

    　必要な機能を選択した後は「OK」を押下するとIISのインストールがはじまる。
    インストールの完了後、ブラウザで自身のアドレス（http://localhost）へアクセスするとIISが動作していることを確認できる。

    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/05.png 600 %}](/blog/images/2012-11-30/05.png)

### IISマネージャーの起動

1.  Webサーバーの状態の確認や、起動/停止を操作できるIISマネージャーは、「ファイル名を指定して実行」から「InetMgr.exe」を指定して起動することができる
    
    [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/06.png "InetMgr.exeを開始する" %}](/blog/images/2012-11-30/06.png)
2.  [{% img /blog/images/2012-11-30-install-iis-7-dot-5-on-windows-7/07.png 600 "IISマネージャーの起動画面" %}](/blog/images/2012-11-30/07.png)

### 参考
- [technet.microsoft.com - Web サーバー (IIS)](http://technet.microsoft.com/ja-jp/library/cc753433%28v=ws.10%29.aspx)

<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4891006129" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=489100570X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
